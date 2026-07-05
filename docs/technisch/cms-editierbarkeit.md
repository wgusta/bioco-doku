# CMS-Editierbarkeit & Visual Editor (Architektur)

Seit Juli 2026 (PR #69, Tracks F & G im Repo-`ROADMAP.md`) ist jedes sichtbare Inhaltselement von bioco.ch in ProcessWire editierbar, und der Visual Editor wurde neu gebaut. Diese Seite beschreibt, was das für Entwickler bedeutet.

---

## 1. No-Fallback-Architektur

Alle Routen sind dünne CMS-Seiten: sie holen ihre Abschnitte über `getPageSections(slug)` (`frontend/lib/processwire.ts`) und rendern sie über den `SectionRenderer`. Beispiel `frontend/app/impressum/page.tsx`:

```tsx
export default async function ImpressumPage() {
  const cmsSections = await getPageSections('impressum')
  return <CmsVisualEditorPage sections={cmsSections} />
}
```

Die Regeln:

* **Kein deutscher Text lebt in JSX.** Inhalte kommen ausschliesslich aus dem CMS.
* **Kein hartkodierter Fallback verdeckt das CMS.** Liefert die API nichts, ist die Seite leer — das ist gewollt, damit fehlender CMS-Inhalt sofort auffällt statt still vom Code überdeckt zu werden.
* **Interaktive Widgets bleiben Code**, werden aber als registrierte `section_component` platziert (Formulare, Karten, Feeds, Rechner).

Durchgesetzt wird das von Tests:

* `frontend/lib/editabilityAudit.ts` klassifiziert jede Route als `cms` oder `hardcoded`; `frontend/tests/editability-audit.test.ts` erzwingt Vollständigkeit (neue `page.tsx` ohne Eintrag lässt den Test fehlschlagen) und dass **nur `/doi-confirm`** hartkodiert bleibt (Funktionsroute für die Double-Opt-In-Bestätigung, Strings sind UI-Zustände, kein Inhalt).
* Paritätstests (`frontend/tests/cms-pages-parity.test.tsx`, `homepage-aktuelles-parity.test.tsx`) rendern die Content-Seeds durch den `SectionRenderer` und prüfen, dass jede Überschrift, jeder Absatz und jede Komponente der früher hartkodierten Seite ankommt — und dass der Seitenquellcode die deutschen Textsignaturen **nicht mehr** enthält (Source-Purity).

Praktische Folge fürs Debugging: «CMS-Änderung wird nicht sichtbar» liegt nicht mehr an hartkodierten Seiten. Prüfe stattdessen den API-Output (`/api/content/sections/{slug}`) und die Revalidation.

---

## 2. Content-Seeds und Content-Freeze-Migration

Der früher hartkodierte Seiteninhalt wurde byte-genau nach `cms/content-seed/*.json` extrahiert (17 Dateien, eine pro Route; Schema in `cms/content-seed/README.md`). Die Seeds sind die einzige Quelle für die PW-Migration **und** für die Paritätstests — Inhalt dort nur ändern, wenn sich der Website-Text bewusst ändern soll.

`site/templates/migrate-content-freeze.php` überträgt die Seeds nach ProcessWire. Sicherheitsmodell:

* **dry-run ist Default**, geschrieben wird nur mit `mode=apply`.
* **Idempotent**, gematcht über `section_id` pro Seite.
* **«CMS gewinnt»**: nicht-leere PW-Felder werden ohne `force=1` nie überschrieben.
* **Additiv**: neue Sections kommen ans Ende; gelöscht oder umsortiert wird nie.
* **Guards**: nur im PW-Bootstrap, nur HTTPS, nur mit Superuser-Session oder Token.

Ablauf: Skript + Seeds per rsync auf den Server, Bootstrap-Datei im CMS-Webroot anlegen, dann `dry-run → Review → apply → verify → Bootstrap löschen`. Achtung OPcache: nach dem rsync kann PHP-FPM noch alten Bytecode ausführen — Reset nur per Web-Request im Vhost-Root, nicht per CLI.

Das vollständige Runbook mit allen Befehlen, Statusspalten und Rollback steht im Repo: [`docs/content-freeze-migration.md`](https://github.com/gemuesegenossenschaft-bioco/bioco-web-project/blob/main/docs/content-freeze-migration.md).

---

## 3. Visual Editor: neuer Aufbau

Vorher war `visual-editor.php` ein 3782-Zeilen-File mit einem untypisierten ~2850-Zeilen-IIFE. Jetzt:

* **`site/templates/visual-editor.php`** ist ein dünner Bootstrap (~240 Zeilen): Auth-Gate, Config-JSON (Seitenliste, Collections, Registry, Focus-Fields), HTML-Skelett, `<script>`-Tag. `ob_end_clean` am Anfang und `exit` am Ende bleiben Pflicht.
* **`frontend/lib/visual-editor/`** ist das typisierte Fundament: `protocol.ts` formalisiert das `bioco:visual-editor:*`-postMessage-Protokoll zwischen Shell und iframe (Discriminated Unions, Origin-Validierung), `shellState.ts` ist der pure, unit-getestete State-Reducer der Shell (`idle → dirty → saving → published | error`).
* **`frontend/visual-editor-shell/`** ist die eigentliche Shell-App in TypeScript (Draft-Handling, Konflikt-Auflösung, PW-Fokus-Deeplinks, Config-Editor, Undo/Redo). Alle deutschen UI-Strings liegen in `strings.ts`.
* Gebaut wird sie mit esbuild: `npm run build:ve-shell` (in `frontend/`) schreibt das minifizierte Bundle nach **`site/templates/visual-editor-app.js`**. Das Bundle ist eingecheckt, weil der Server nicht bauen kann.

Unverändert übernommen (bewährt und getestet): das Protokollvokabular, `visualEditorContract.ts`, `visual-editor-focus-fields.json`, die `data-ve-*`-Marker, `content-publish` mit Fingerprint-Konkurrenzerkennung, die Collections-Abstraktion und alle Constraints aus `CLAUDE.md` (u. a.: kein Page-Picker — Seitenwechsel läuft über echte Navigation im iframe).

---

## 4. Neue Komponenten in der Registry

Neu in `site/templates/component-registry.json` (Renderer in `frontend/components/sections/RegisteredSectionComponents.tsx` bzw. eigene Komponenten):

| Schlüssel | Zweck |
|-----------|-------|
| `accordion_item` | aufklappbarer Eintrag (`details`/`summary`), Titel = Zeile, Text = Inhalt |
| `steps` | bis zu 4 nummerierte Schritte, Titel/Text pro Schritt über `configSchema` |
| `link_tiles` | Kachel-Raster mit Symbol, Titel, Text, Link (bis 4 Kacheln, `configSchema`) |
| `group_cards` | Arbeitsgruppen-Karten (Mitmachen); Karteninhalte live aus `/api/content/groups` |

`events_feed` hat neu ein `configSchema`: `variant` (`standard` / `banner` — Banner rendert die kompakte Liste) und `limit` (1–12).

Neue Komponente = Registry-Eintrag (mit `configSchema`) + Renderer + Eintrag in `componentRenderers` und `layoutOwnedKeys` (`frontend/lib/componentRenderers.tsx`). Siehe auch [API-Dokumentation, Abschnitt 6](api.md#6-komponenten-registry).

---

## 5. Deploy-Änderung

`scripts/deploy.sh` baut vor dem CMS-Upload das Shell-Bundle und prüft es:

```bash
npm run build:ve-shell
test -s site/templates/visual-editor-app.js
grep -q "bioco:visual-editor:" site/templates/visual-editor-app.js
```

**`visual-editor-app.js` gehört in beide rsync-Listen** — im Deploy-Skript und bei den manuellen Befehlen aus `CLAUDE.md` (Full Deploy und CMS-only Deploy). Wer nur `visual-editor.php` deployt, serviert eine Shell ohne (oder mit veralteter) App; der Bootstrap hängt zwar einen Cache-Buster (`?v=<md5>`) an, kann die Datei aber nicht ersetzen.

Die Content-Freeze-Migration läuft **vor** dem Frontend-Deploy auf dem Server (Bootstrap-Datei, danach löschen) — siehe Abschnitt 2.

---

*Zuletzt aktualisiert: Juli 2026*
