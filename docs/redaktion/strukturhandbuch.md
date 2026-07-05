# Strukturhandbuch bioco.ch

Dieses Handbuch erklärt die Struktur der Website und dient als Nachschlagewerk für Redakteure und Administratoren.

---

## 1. Architektur

Die Website besteht aus zwei Systemen auf demselben Server (Novatrend cPanel):

* **ProcessWire** (PHP): das Redaktionssystem (Headless CMS). Hier liegen alle Inhalte. Zugang: `https://cms.bioco.ch/processwire/`.
* **Next.js 14** (React/TypeScript): das Frontend, das die Website für Besucher darstellt.

Next.js holt die Inhalte über eine **einheitliche REST-API** aus ProcessWire (`/api/content/*`) und rendert die Seiten statisch vor (SSG/ISR). Beim Publizieren stösst das CMS eine gezielte Aktualisierung der betroffenen Seiten an (**On-Demand-Revalidation**), sodass Änderungen ohne neues Deployment live gehen.

```
Redakteur ──▶ Visual Editor ──▶ ProcessWire (Inhalte)
                                      │  Revalidation
                                      ▼
                               Next.js (Website)  ──▶ Besucher
```

---

## 2. Aktueller Stand

**Alle Seiten sind CMS-gesteuert.** Seit Juli 2026 gibt es keine Seiten mehr, deren Texte fest im Code stehen — jede Seite besteht aus CMS-Abschnitten (`content_sections`) und wird im Visual Editor gepflegt. Einzige Ausnahme ist `/doi-confirm`, die technische Bestätigungsseite für Newsletter-Anmeldungen: sie zeigt nur Funktionszustände, keinen redaktionellen Inhalt.

| Bereich | Stand |
|---------|-------|
| Seiteninhalte (Abschnitte) | CMS-gesteuert über `content_sections`, im Visual Editor bearbeitbar |
| Navigation | aus ProcessWire (`/api/content/navigation`) |
| Events / Aktuelles | eigene Seiten unter `/aktuelles/` (Template `event`), Sammlungs-Editor im Visual Editor |
| Formulare | in ProcessWire verarbeitet (mit Double-Opt-In und Captcha); Formulartexte pflegt das Technik-Team |
| Revalidation | aktiv; Publizieren aktualisiert die Live-Seite automatisch |
| Matomo Analytics | aktiv (cookieless) |

---

## 3. Inhaltsmodell: Abschnitte (`content_sections`)

Eine CMS-gesteuerte Seite hält ihre Inhalte in einem Wiederholungsfeld `content_sections`. Jeder Eintrag ist ein **Abschnitt** mit diesen Feldern:

| Feld | Bedeutung |
|------|-----------|
| `section_title` | Überschrift des Abschnitts |
| `section_eyebrow` | kleine Zeile über der Überschrift |
| `section_text` | Fliesstext (Rich Text / CKEditor) |
| `section_image`, `image_alt` | Bild aus der Mediathek und Alt-Text |
| `button_text` / `button_href` / `button_variant` | erster Button |
| `button2_text` / `button2_href` / `button2_variant` | zweiter Button |
| `section_image_brightness/contrast/saturate` | Bildanpassungen |
| `section_component` | Schlüssel einer Komponente (siehe Abschnitt 4) |
| `section_config` | JSON-Konfiguration der Komponente |

Das Layout (Bild links/rechts, Banner, Galerie, nur Text) ergibt sich aus den gefüllten Feldern bzw. der gewählten Komponente. Überschriften können auch direkt im Rich Text stehen.

---

## 4. Komponenten (Registry)

Setzt ein Abschnitt das Feld `section_component`, rendert das Frontend eine registrierte Komponente. Die Zuordnung steht in `site/templates/component-registry.json`.

| Schlüssel | Funktion |
|-----------|----------|
| `pricing_table` | Abo-Preistabelle mit drei Stufen (Halb, Standard, Doppel); Werte über `section_config` |
| `page_intro` | Einleitungsblock mit konfigurierbarer Breite/Ausrichtung |
| `media_text`, `cards_grid`, `gallery_strip`, `text_columns` | Layout-Bausteine |
| `timeline_header`, `timeline_item`, `cta_band` | Zeitleiste und Aktionsband |
| `accordion_item` | aufklappbarer Akkordeon-Eintrag (Titel = Zeile, Text = Inhalt) |
| `steps` | bis zu vier nummerierte Schritte mit Kreis-Nummer |
| `link_tiles` | Kachel-Raster mit Symbol, Titel, Text und Link (z. B. Kundenportal) |
| `events_feed` | Liste der nächsten Events; Darstellung **Standard** oder **Banner** (kompakt), Anzahl einstellbar |
| `group_cards` | Karten der Arbeitsgruppen auf Mitmachen (Karteninhalte kommen live aus dem CMS) |
| `schnuppertage` | Schnuppertage-Block |
| `pricing_calculator` | interaktiver Preisrechner |
| `saisonkalender` | Erntekalender |
| `gallery` | Bildergalerie |
| `depot_map`, `geisshof_map` | Karten |
| `contact_form`, `membership_form`, `subscribe_form`, `visit_day_form`, `waiting_list_form` | Formulare |

Komponenten mit Optionen (zum Beispiel `pricing_table`, `steps`, `link_tiles`, `events_feed`) zeigen ihre Felder im Visual Editor als zusätzliche Eingaben (Text-, Zahl- oder Auswahlfelder), gespeichert in `section_config`.

Der wiederkehrende Block «Möchtest du uns kennenlernen?» ist auf jeder Seite ein eigener Abschnitt (`section_id: kennenlernen-cta`) und kann pro Seite angepasst werden.

---

## 5. Events und Aktuelles

Events sind eigene Seiten mit dem Template `event` unter `/aktuelles/`. Wichtige Felder: `event_start`, `event_end`, `event_status` (bevorstehend/vergangen), `event_location`, `event_summary`, `body`, `event_card_image`, `event_media`, `event_signup_enabled`, `event_signup_notes`.

Im Visual Editor werden Events über das **Sammlungs-Panel** verwaltet (öffnet sich auf der Seite Aktuelles): Liste aller Einträge, je Eintrag **→ In PW öffnen**, und **Neuen Event erstellen** über einen Datumswähler. Das separate Blog-Template `news_item` existiert, wird aktuell aber nicht verwendet.

---

## 6. Seitenübersicht (Auswahl)

Alle Seiten sind CMS-gesteuert und im Visual Editor bearbeitbar.

| URL | Seite |
|-----|-------|
| `/` | Startseite |
| `/abos` | Abos und Preise |
| `/gemuese` | Gemüse und Ernte |
| `/solawi` | Solidarische Landwirtschaft |
| `/mitmachen`, `/bioco-werden` | Mitglied werden |
| `/aktuelles` | News und Events |
| `/standorte-depots` | Abholstationen |
| `/wir` | Über biocò |
| `/kontakt` | Kontakt |
| `/anmeldung`, `/warteliste`, `/newsletter`, `/tag-der-offenen-tuer` | Anmeldungen |
| `/kundenportal` | Kundenportal (Portal-Kacheln) |
| `/impressum`, `/datenschutz`, `/statuten` | Rechtliches |

Weitere CMS-Seiten ausserhalb dieser Liste werden über eine Catch-all-Route automatisch aus ProcessWire gerendert. `/doi-confirm` ist die einzige Seite ohne bearbeitbaren Inhalt (technische Bestätigungsseite).

---

## 7. Caching und Revalidation

Next.js speichert Seiten zwischen (ISR). Beim Speichern in ProcessWire stösst ein Hook (`site/ready.php`) eine Revalidation der betroffenen Pfade an; das Publizieren im Visual Editor löst sie zusätzlich direkt aus und meldet zurück, ob die Website die Aktualisierung bestätigt hat. Details: siehe [API-Dokumentation](../technisch/api.md).

---

## 8. Glossar

| Begriff | Erklärung |
|---------|-----------|
| **Abschnitt / Block** | Baustein einer Seite (`content_sections`) |
| **Komponente** | Abschnitt mit besonderem Aussehen und eigenen Optionen |
| **Visual Editor** | Live-Vorschau-Editor unter `/visual-editor/` |
| **Headless CMS** | CMS ohne eigene Darstellung, liefert nur Daten |
| **ISR / Revalidation** | gezielte Aktualisierung zwischengespeicherter Seiten |
| **Template** | Vorlage für einen Seitentyp in ProcessWire |
| **DOI** | Double-Opt-In, zweistufige Bestätigung bei Anmeldungen |

---

*Zuletzt aktualisiert: Juli 2026*
