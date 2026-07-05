# Technische API-Dokumentation

Diese Dokumentation beschreibt die REST-API von bioco.ch. Sie richtet sich an Entwickler, die das System erweitern oder warten.

---

## 1. Architektur

* **Backend**: ProcessWire (PHP) liefert Inhalte als JSON.
* **Frontend**: Next.js 14 (React/TypeScript, Standalone) konsumiert die API und rendert die Website (SSG/ISR).
* **Kommunikation**: JSON über HTTPS.

Alle CMS-Endpunkte laufen über **einen einheitlichen Router**: die ProcessWire-Seite `/api/` mit Template `api` (`site/templates/api.php`, `urlSegments=1`). Der erste URL-Abschnitt wählt den Endpunkt, weitere Segmente sind Parameter. Eine ältere Aufteilung in `pages.php` / `forms.php` / `doi.php` über `site/api/.htaccess` ist abgelöst.

Der Frontend-Client liegt in `frontend/lib/cmsClient.ts` und `frontend/lib/processwire.ts`.

### Authentifizierung

* **Öffentliche Lese-Endpunkte** (`health`, `content/*`, `media-files`): kein Schlüssel nötig.
* **Andere Endpunkte**: Header `X-API-Key` mit dem konfigurierten Schlüssel.
* **Admin-Endpunkte** (Visual Editor): ProcessWire-Session (`requireAdminSession()`), also eingeloggt im CMS.

---

## 2. Öffentliche Endpunkte (GET)

Basis: `https://cms.bioco.ch/api`

| Endpunkt | Beschreibung |
|----------|--------------|
| `/api/health` | Health-Check |
| `/api/content/hero` | Hero-Daten der Startseite |
| `/api/content/homepage` | Abschnitte der Startseite |
| `/api/content/sections/{slug}` | Abschnitte einer Seite nach Name/Slug |
| `/api/content/page?path=` | Einzelne Seite nach Pfad |
| `/api/content/pages` | Liste aller Seiten |
| `/api/content/navigation` | Navigationsbaum |
| `/api/content/events` | Events (upcoming + past) |
| `/api/content/aktuelles` | News/Aktuelles-Beiträge |
| `/api/content/groups` | Arbeitsgruppen-Karten (Komponente `group_cards` auf Mitmachen) |
| `/api/content/instagram` | Instagram-Feed |
| `/api/content/settings` | Typografie- und Design-Tokens |
| `/api/content/page-path?id=` | Pfad-Lookup für Vorschau |
| `/api/media-files` | Dateiliste der Mediathek |

**Beispiel**: `GET /api/content/sections/abos` liefert die Abschnitte der Abo-Seite inklusive Komponenten (`section_component`) und deren `section_config`.

---

## 3. Admin-Endpunkte (POST, ProcessWire-Session)

Diese Endpunkte treiben den Visual Editor. Sie erfordern eine aktive CMS-Anmeldung.

| Endpunkt | Beschreibung |
|----------|--------------|
| `/api/content-save` | Abschnittsfelder speichern (per `sectionPwId`) |
| `/api/content-publish` | Entwurf publizieren; gibt zusätzlich `revalidated` + `revalidateStatus` zurück |
| `/api/sections-add` | Neuen Abschnitt im `content_sections`-Repeater anlegen |
| `/api/sections-reorder` | Abschnitte umsortieren |
| `/api/sections-delete` | Abschnitt löschen |
| `/api/collection-create` | Neuen Sammlungseintrag (Event) unter `/aktuelles/` per Datum anlegen; gibt `pwId` + `editUrl` zurück |
| `/api/content/event-to-recap` | Bevorstehenden Event in Rückblick (past) umwandeln |
| `/api/media-import`, `/api/media-import-batch` | Medien importieren |
| `/api/media-usage` | Verwendung eines Mediums prüfen |

### `collection-create`

Legt eine `event`-Seite unter `/aktuelles/` an. Body: `{ "type": "event", "date": "YYYY-MM-DD" }`. Setzt Standardwerte (`event_status=upcoming`, erste `event_type`-Option, `event_start`/`event_end` aus dem Datum), stösst eine Revalidation an und liefert:

```json
{ "success": true, "pwId": 1850, "title": "Neuer Event 01.09.2026", "editUrl": "…/page/edit/?id=1850" }
```

### `content-publish`

Speichert den Entwurf, baut den kanonischen Zustand neu und revalidiert die Seite **synchron**, damit der Visual Editor zurückmelden kann, ob die Änderung die Live-Website erreicht hat:

```json
{ "success": true, "fingerprint": "…", "sections": [ … ], "revalidated": true, "revalidateStatus": 200 }
```

Ist `revalidated` false, zeigt der Visual Editor den roten Status „Publiziert, aber Build nicht aktualisiert".

---

## 4. Formular-Endpunkte (POST)

| Endpunkt | Beschreibung |
|----------|--------------|
| `/api/forms/contact` | Kontaktformular |
| `/api/forms/subscribe` | Newsletter (Double-Opt-In) |
| `/api/forms/visit` | Anmeldung Besuchstag |
| `/api/forms/waiting-list` | Warteliste |
| `/api/forms/event-signup` | Event-Anmeldung |

Formulare prüfen Cloudflare Turnstile (`TURNSTILE_SECRET_KEY`) serverseitig.

---

## 5. Revalidation (CMS → Build)

Damit CMS-Änderungen ohne Deployment live gehen:

1. **Frontend-Route** `POST /api/revalidate` (Next.js, `frontend/app/api/revalidate/route.ts`): akzeptiert `path | paths | tag | tags | layout`, geschützt über `REVALIDATE_SECRET`. Antwort enthält `revalidated: true`.
2. **CMS-Auslöser** in `site/ready.php`: Beim Speichern einer Seite/eines Repeater-Items wird die Revalidation der betroffenen Pfade eingereiht (entprellt, mit Sammel-Flush über `LazyCron`).
3. **Synchroner Helfer** `biocoRevalidatePathsNow($paths)`: vom `content-publish`-Endpunkt genutzt, liefert ein strukturiertes Ergebnis (`ok`, `status`, `error`).

Vertrag: `start.sh: REVALIDATE_SECRET` muss `site/config.php: nextRevalidateSecret` entsprechen; Ziel-URL `http://127.0.0.1:49154/api/revalidate`.

---

## 6. Komponenten-Registry

Welche `section_component`-Schlüssel das Frontend rendert, steht in `site/templates/component-registry.json` (gelesen von `frontend/lib/componentRegistry.ts`). Ein Eintrag definiert Schlüssel, Label, Ziel-Komponente, CMS-Felder, `defaultConfig` und ein `configSchema` (Feldtypen `select`, `range`, `text`, `number`) für den Visual-Editor-Konfigurator. Beispiel: `pricing_table` (Abo-Preistabelle, drei Stufen).

Seit Juli 2026 neu bzw. erweitert: `accordion_item`, `steps`, `link_tiles`, `group_cards` sowie `events_feed` mit `configSchema` (`variant`: `standard`/`banner`, `limit`). Details und wie neue Komponenten dazukommen: [CMS-Editierbarkeit & Visual Editor](cms-editierbarkeit.md).

---

## 7. Sicherheit und Wartung

* Geheimnisse über `getenv()` in `site/config.php` (nicht im Repo).
* Admin-Endpunkte nur mit ProcessWire-Session; Schreibzugriffe ohne Session werden mit „Authentication required" abgewiesen.
* Nach Änderungen an `api.php`: CMS-Templates deployen (siehe Deploy-Skript). PHP-OPcache kann alte Bytecodes halten, daher nach dem Deployment prüfen.

---

*Zuletzt aktualisiert: Juli 2026*
