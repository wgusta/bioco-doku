# Strukturhandbuch bioco.ch für Administratoren

Dieses Handbuch erklärt die Struktur der bioco.ch Webseite und dient als Nachschlagewerk für Redakteure und Administratoren.

---

## 1. Architektur der Webseite

Die bioco.ch Webseite besteht aus zwei getrennten Systemen:

### ProcessWire (das Redaktionssystem)

ProcessWire ist das Content Management System (CMS), in dem Inhalte verwaltet werden. Es funktioniert wie ein "Büro" oder "Lager", in dem alle Texte, Bilder und Einstellungen gespeichert sind.

- **Zugang**: `https://cms.bioco.ch/processwire/`
- **Funktion**: Inhalte erstellen, bearbeiten, löschen
- **Sprache**: PHP

### Next.js (das Schaufenster)

Next.js ist das Frontend, das die Webseite für Besucher darstellt. Es holt sich Daten vom ProcessWire und zeigt sie schön formatiert an.

- **Technologie**: React, TypeScript
- **Hosting**: Vercel oder Server
- **Funktion**: Darstellung für Besucher

### Verbindung: REST API

Die beiden Systeme kommunizieren über eine REST API. ProcessWire stellt Daten als JSON bereit, Next.js ruft diese ab und zeigt sie an.

```
┌─────────────────┐     API Request      ┌─────────────────┐
│                 │ ─────────────────▶   │                 │
│   Next.js       │                      │   ProcessWire   │
│   (Frontend)    │ ◀─────────────────   │   (Backend)     │
│                 │     JSON Response    │                 │
└─────────────────┘                      └─────────────────┘
```

---

## 2. Aktueller Entwicklungsstand

### Was funktioniert

| Funktion | Status | Beschreibung |
|----------|--------|--------------|
| Kontaktformular | Aktiv | Wird durch ProcessWire verarbeitet |
| Newsletter-Anmeldung | Aktiv | Mit Double Opt-In |
| Warteliste-Formular | Aktiv | Mit Double Opt-In |
| Besuchstag-Anmeldung | Aktiv | Mit Double Opt-In |
| Matomo Analytics | Aktiv | Cookieless Tracking |

### Was noch Entwicklung braucht

| Funktion | Status | Beschreibung |
|----------|--------|--------------|
| Seiteninhalt | Hardcodiert | Texte sind direkt im Code, nicht aus ProcessWire |
| Navigation | Hardcodiert | Menüpunkte sind im Code festgelegt |
| Bilder | Hardcodiert | Bildpfade sind im Code festgelegt |
| Aktuelles/Events | Hardcodiert | Daten in `AktuellesData.tsx` statt aus CMS |

**Wichtig**: Änderungen in ProcessWire werden derzeit **nicht** automatisch auf der Webseite sichtbar. Die Infrastruktur existiert, muss aber noch verbunden werden.

---

## 3. Feldwörterbuch: ProcessWire → Next.js

Diese Tabelle zeigt, welche ProcessWire-Felder welchen Next.js-Komponenten entsprechen.

### Basis-Felder

| ProcessWire Feld | Typ | Next.js Komponente | Beschreibung |
|------------------|-----|-------------------|--------------|
| `title` | Text | `Hero.tsx` → `<h1>` | Seitentitel, erscheint als Hauptüberschrift |
| `body` | Textarea/HTML | Seiteninhalt | Haupttext der Seite |
| `hero_image` | Bild | `Hero.tsx` → Hintergrundbild | Grosses Kopfbild der Seite |
| `hero_subtitle` | Text | `Hero.tsx` → `<p>` | Untertitel unter der Hauptüberschrift |

### Medien-Felder

| ProcessWire Feld | Typ | Next.js Komponente | Beschreibung |
|------------------|-----|-------------------|--------------|
| `logo_image` | Bild | `Header.tsx` → Logo | Webseiten-Logo im Kopfbereich |
| `gallery_images` | Bilder (mehrere) | `Gallery.tsx` | Bildergalerie mit mehreren Bildern |

### Layout-Felder

| ProcessWire Feld | Typ | Next.js Komponente | Beschreibung |
|------------------|-----|-------------------|--------------|
| `sidebar_content` | Textarea | Seitenleiste | Zusätzlicher Inhalt neben dem Haupttext |
| `footer_content` | Textarea | `Footer.tsx` | Inhalt im Fussbereich |
| `css_variant` | Text | Stylesheet-Auswahl | Wählt eine Design-Variante |

### Bild-Eigenschaften

Jedes Bild in ProcessWire hat zusätzliche Eigenschaften:

| Eigenschaft | Beschreibung |
|-------------|--------------|
| `url` | Pfad zum Bild |
| `description` | Bildbeschreibung (für Alt-Text und Barrierefreiheit) |

---

## 4. Komponentenübersicht

### Layout-Komponenten

| Komponente | Datei | Funktion |
|------------|-------|----------|
| Header | `Header.tsx` | Kopfbereich mit Logo und Navigation |
| Footer | `Footer.tsx` | Fussbereich mit Links und Kontaktdaten |
| Navigation | `Navigation.tsx` | Hauptmenü |
| MobileMenu | `MobileMenu.tsx` | Menü für Mobilgeräte |
| Hero | `Hero.tsx` | Grosses Kopfbild mit Titel |

### Inhalts-Komponenten

| Komponente | Datei | Funktion |
|------------|-------|----------|
| CTA | `CTA.tsx` | Call-to-Action Button |
| CardHeader | `CardHeader.tsx` | Überschrift für Karten |
| Gallery | `Gallery.tsx` | Bildergalerie |
| InfoTooltip | `InfoTooltip.tsx` | Info-Hinweise |

### Spezial-Komponenten

| Komponente | Datei | Funktion |
|------------|-------|----------|
| PricingCalculator | `PricingCalculator.tsx` | Preisrechner für Abos |
| Saisonkalender | `Saisonkalender.tsx` | Kalender für Erntezeiten |
| BasketVisualization | `BasketVisualization.tsx` | Visualisierung des Gemüsekorbs |
| DepotMap | `DepotMap.tsx` | Karte der Abholstationen |
| GeisshofMap | `GeisshofMap.tsx` | Karte zum Geisshof |
| AktuellesItem | `AktuellesItem.tsx` | Einzelner Aktuelles-Eintrag |
| EventsBanner | `EventsBanner.tsx` | Banner für Events |

### Formular-Komponenten

| Komponente | Datei | Funktion |
|------------|-------|----------|
| ContactForm | `forms/ContactForm.tsx` | Kontaktformular |
| SubscribeForm | `forms/SubscribeForm.tsx` | Newsletter-Anmeldung |
| WaitingListForm | `forms/WaitingListForm.tsx` | Warteliste |
| VisitDayForm | `forms/VisitDayForm.tsx` | Besuchstag-Anmeldung |
| MembershipForm | `forms/MembershipForm.tsx` | Mitgliedschaftsformular |

---

## 5. Seitenübersicht

### Hauptseiten

| URL | Seite | Beschreibung | Besondere Komponenten |
|-----|-------|--------------|----------------------|
| `/` | Startseite | Willkommen, Übersicht | Hero, AktuellesItem, EventsBanner |
| `/ernte` | Ernte | Was wächst gerade | Saisonkalender, BasketVisualization |
| `/abos` | Abos | Abo-Modelle und Preise | PricingCalculator |
| `/wir` | Wir | Über biocò und den Geisshof | GeisshofMap |
| `/anpacken` | Anpacken | Mitarbeit auf dem Feld | — |
| `/mitmachen` | Mitmachen | Mitglied werden | MembershipForm |
| `/aktuelles` | Aktuelles | News und Events | AktuellesTabs, AktuellesItem |

### Informationsseiten

| URL | Seite | Beschreibung | Besondere Komponenten |
|-----|-------|--------------|----------------------|
| `/depots` | Depots | Abholstationen | DepotMap |
| `/kontakt` | Kontakt | Kontaktinformationen | ContactForm |
| `/warteliste` | Warteliste | Warteliste-Anmeldung | WaitingListForm |
| `/tag-der-offenen-tuer` | Tag der offenen Tür | Event-Anmeldung | VisitDayForm |
| `/newsletter` | Newsletter | Newsletter-Anmeldung | SubscribeForm |

### Rechtliche Seiten

| URL | Seite | Beschreibung |
|-----|-------|--------------|
| `/impressum` | Impressum | Rechtliche Angaben |
| `/datenschutz` | Datenschutz | Datenschutzerklärung |
| `/statuten` | Statuten | Genossenschaftsstatuten |

### Spezialseiten

| URL | Seite | Beschreibung |
|-----|-------|--------------|
| `/anmeldung` | Anmeldung | Mitgliedschaftsanmeldung |
| `/anmeldung/danke` | Danke | Bestätigungsseite nach Anmeldung |
| `/doi-confirm` | DOI Bestätigung | Double Opt-In Bestätigung |
| `/kundenportal` | Kundenportal | Mitgliederbereich (extern) |
| `/intranet` | Intranet | Interner Bereich (extern) |

---

## 6. Was Admins jetzt tun können

### Formulare verwalten

Alle Formular-Einreichungen werden in ProcessWire gespeichert:

1. Einloggen unter `https://cms.bioco.ch/processwire/`
2. Im Seitenbaum die Formular-Einreichungen finden
3. Einträge ansehen, exportieren oder löschen

### ProcessWire-Inhalte pflegen

Auch wenn die Inhalte noch nicht dynamisch angezeigt werden, können sie bereits vorbereitet werden:

1. Seiten im Seitenbaum anlegen
2. Felder ausfüllen (title, body, hero_image, etc.)
3. Bilder hochladen

**Hinweis**: Diese Inhalte werden erst sichtbar, wenn die Entwicklung abgeschlossen ist.

---

## 7. Entwicklungsbedarf

Folgende Arbeiten sind nötig, um ProcessWire-Inhalte dynamisch anzuzeigen:

### Priorität 1: Grundfunktionen

| Aufgabe | Beschreibung | Betroffene Dateien |
|---------|--------------|-------------------|
| Page-Daten laden | `getPageData()` in Seiten verwenden | Alle `page.tsx` Dateien |
| Navigation dynamisch | `getNavigation()` verwenden | `Navigation.tsx` |
| Hero dynamisch | Daten aus API statt Props | `Hero.tsx`, alle Seiten |

### Priorität 2: Erweiterungen

| Aufgabe | Beschreibung |
|---------|--------------|
| Aktuelles aus CMS | News und Events aus ProcessWire laden |
| Bildergalerie | Galerie-Bilder aus ProcessWire |
| Inhaltsblöcke | Repeater/PageTable für flexible Inhalte |

### Priorität 3: Optimierungen

| Aufgabe | Beschreibung |
|---------|--------------|
| Caching optimieren | Revalidation-Zeiten anpassen |
| Preview-Modus | Vorschau unveröffentlichter Inhalte |
| Fehlerbehandlung | Fallbacks bei API-Fehlern |

---

## 8. Technische Dokumentation

Für Entwickler und technisch Interessierte gibt es eine separate, detaillierte Dokumentation:

**[API-Dokumentation](../technisch/api.md)** enthaelt:

- Architektur-Übersicht und Datenfluss
- Alle API-Endpunkte im Detail
- ProcessWire Module (FormProcessor, DOIManager)
- Next.js Integration
- Sicherheit und Wartung

---

## 9. Glossar

| Begriff | Erklärung |
|---------|-----------|
| **API** | Application Programming Interface, Schnittstelle zwischen Systemen |
| **CMS** | Content Management System, System zur Inhaltsverwaltung |
| **DOI** | Double Opt-In, zweistufige Bestätigung bei Anmeldungen |
| **Frontend** | Der sichtbare Teil der Webseite |
| **Backend** | Der unsichtbare Teil (Datenbank, Verwaltung) |
| **Hardcodiert** | Im Programmcode fest eingebaut, nicht änderbar ohne Entwickler |
| **Headless CMS** | CMS ohne eigene Darstellung, liefert nur Daten |
| **JSON** | JavaScript Object Notation, Datenformat für API |
| **Next.js** | React-Framework für Webseiten |
| **ProcessWire** | Open-Source CMS (PHP) |
| **REST API** | Standardisierte Schnittstelle für Web-Dienste |
| **Revalidation** | Automatische Aktualisierung von gecachten Daten |
| **Template** | Vorlage für Seitentypen in ProcessWire |

---

*Zuletzt aktualisiert: Januar 2026*
