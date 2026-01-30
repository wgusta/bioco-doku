# Governance: Inhalte planen und Änderungen beantragen

Dieses Dokument beschreibt, wie Inhaltsänderungen und technische Weiterentwicklungen der bioco.ch Webseite koordiniert werden.

---

## 1. Übersicht: Zwei Wege für Änderungen

| Art der Änderung | Werkzeug | Beispiele |
|------------------|----------|-----------|
| **Inhalte** (Texte, Bilder, Events) | Content Planning in ProcessWire | Neuer Aktuelles-Beitrag, Bildaustausch, Textkorrektur |
| **Funktionen** (Code, Design, Struktur) | GitHub Issues | Neue Seite, neues Formular, Design-Anpassung |

---

## 2. Content Planning (ProcessWire)

### Zugang

**URL**: [https://cms.bioco.ch/processwire/content-planning/](https://cms.bioco.ch/processwire/content-planning/)

Anmeldung mit ProcessWire-Zugangsdaten erforderlich.

### Was ist Content Planning?

Content Planning ist ein integriertes Tool in ProcessWire, das euch ermöglicht:

- **Inhaltsänderungen zu planen** bevor sie live gehen
- **Aufgaben zuzuweisen** an Teammitglieder
- **Termine zu setzen** für Veröffentlichungen
- **Status zu verfolgen** (geplant, in Arbeit, fertig)

### Wann Content Planning nutzen?

| Situation | Content Planning |
|-----------|------------------|
| Neuer Text für bestehende Seite | Ja |
| Neues Bild hochladen | Ja |
| Aktuelles/Event erstellen | Ja |
| Saisonale Anpassungen | Ja |
| Rechtschreibkorrektur | Ja |
| Neue Seite erstellen | Nein (GitHub Issue) |
| Neues Formular | Nein (GitHub Issue) |
| Design-Änderung | Nein (GitHub Issue) |

### Ablauf

```
1. Einloggen
   └─► cms.bioco.ch/processwire/

2. Content Planning öffnen
   └─► Menü: Content Planning

3. Aufgabe erstellen
   └─► Titel: Was soll geändert werden?
   └─► Beschreibung: Details zur Änderung
   └─► Zuweisung: Wer ist verantwortlich?
   └─► Termin: Bis wann?

4. Status aktualisieren
   └─► Geplant → In Arbeit → Fertig
```

---

## 3. GitHub Issues (Technische Änderungen)

### Was ist ein GitHub Issue?

Ein GitHub Issue ist eine Aufgabe oder ein Verbesserungsvorschlag im Code-Repository. Issues werden verwendet für:

- Neue Funktionen
- Fehlerbehebungen (Bugs)
- Design-Änderungen
- Strukturelle Anpassungen

### Wann ein Issue erstellen?

| Situation | Issue erstellen? |
|-----------|------------------|
| Neue Seite benötigt | Ja |
| Neues Formularfeld | Ja |
| Button funktioniert nicht | Ja |
| Seite lädt langsam | Ja |
| Design sieht falsch aus | Ja |
| Neuer Text für bestehende Seite | Nein (Content Planning) |
| Bild austauschen | Nein (Content Planning) |

### Issue erstellen: Schritt für Schritt

1. **GitHub öffnen**
   - Repository: `github.com/[organisation]/bioco-web-project`
   - Tab: **Issues**

2. **New Issue klicken**

3. **Titel wählen** (kurz und prägnant)
   
   Gut:
   - "Kontaktformular: Telefonnummer-Feld hinzufügen"
   - "Startseite: Hero-Bild lädt nicht auf Mobile"
   - "Neue Seite: FAQ erstellen"
   
   Schlecht:
   - "Problem"
   - "Funktioniert nicht"
   - "Bitte ändern"

4. **Beschreibung ausfüllen** (siehe Template unten)

5. **Labels hinzufügen** (falls vorhanden)
   - `bug` für Fehler
   - `enhancement` für Verbesserungen
   - `content` für Inhaltsstruktur

6. **Submit new issue**

### Issue-Template

```markdown
## Beschreibung
[Was soll geändert/hinzugefügt werden?]

## Begründung
[Warum ist diese Änderung nötig?]

## Betroffene Seite(n)
[URL oder Seitenname]

## Aktuelles Verhalten (bei Bugs)
[Was passiert jetzt?]

## Gewünschtes Verhalten
[Was soll passieren?]

## Screenshots (falls hilfreich)
[Bilder hier einfügen]

## Priorität
- [ ] Dringend (blockiert Arbeit)
- [ ] Normal (nächstes Quartal)
- [ ] Niedrig (irgendwann)
```

### Beispiel: Gutes Issue

```markdown
**Titel**: Kontaktformular: Dropdown für Betreff hinzufügen

## Beschreibung
Das Kontaktformular soll ein Dropdown-Menü für den Betreff 
erhalten, statt eines Freitextfelds.

## Begründung
Wir erhalten viele unspezifische Anfragen. Mit vordefinierten 
Optionen können wir Anfragen besser kategorisieren und schneller 
beantworten.

## Betroffene Seite(n)
- /kontakt

## Gewünschtes Verhalten
Dropdown mit Optionen:
- Mitgliedschaft
- Schnupperabo
- Besuchstag
- Allgemeine Frage
- Sonstiges

## Priorität
- [x] Normal (nächstes Quartal)
```

---

## 4. Kommunikation: Spezifisch sein

### Warum Spezifität wichtig ist

Unspezifische Anfragen führen zu:
- Rückfragen (Zeitverlust)
- Missverständnissen
- Falschen Umsetzungen
- Frustration

### Checkliste für gute Kommunikation

Bevor du eine Anfrage stellst, prüfe:

| Frage | Beispiel |
|-------|----------|
| **Was** genau soll geändert werden? | "Der dritte Absatz auf der Wir-Seite" |
| **Wo** befindet es sich? | "URL: bioco.ch/wir, Abschnitt 'Geschichte'" |
| **Warum** ist die Änderung nötig? | "Information ist veraltet seit Umzug 2025" |
| **Wie** soll es aussehen? | "Neuer Text: [vollständiger Text hier]" |
| **Wann** wird es benötigt? | "Vor dem Saisonstart im März" |

### Beispiele: Gut vs. Schlecht

**Schlecht**:
> "Das Bild auf der Seite ist falsch."

**Gut**:
> "Auf der Seite /ernte ist das Hero-Bild veraltet. Es zeigt noch das alte Feld. Bitte ersetzen durch das neue Bild 'geisshof_2025.jpg' (im Anhang). Sollte bis Ende Februar online sein."

---

**Schlecht**:
> "Kann man da was ändern?"

**Gut**:
> "Auf der Startseite im Abschnitt 'Wie funktioniert's' fehlt der Hinweis auf die Depot-Standorte. Bitte folgenden Satz ergänzen nach Schritt 4: 'Die Abholstandorte findest du unter Depots.'"

---

**Schlecht**:
> "Die Seite sieht komisch aus."

**Gut**:
> "Auf Mobile (iPhone 13) überlappt der Text im Hero-Bereich auf der Startseite mit dem Button. Screenshot im Anhang. Browser: Safari."

---

## 5. Quartals-Release-Zyklus

### Übersicht

Grössere Änderungen an der Webseite werden in **Quartals-Releases** gebündelt:

| Quartal | Zeitraum | Release-Datum |
|---------|----------|---------------|
| Q1 | Januar bis März | Ende März |
| Q2 | April bis Juni | Ende Juni |
| Q3 | Juli bis September | Ende September |
| Q4 | Oktober bis Dezember | Ende Dezember |

### Warum Quartals-Releases?

1. **Planbarkeit**: Ihr wisst, wann Änderungen live gehen
2. **Qualität**: Mehr Zeit für Tests und Korrekturen
3. **Bündelung**: Zusammenhängende Änderungen werden gemeinsam released
4. **Stabilität**: Weniger häufige Änderungen = weniger Risiko

### Was gehört in ein Quartals-Release?

| Im Quartals-Release | Sofort (Hotfix) |
|---------------------|-----------------|
| Neue Seiten | Kritische Sicherheitslücken |
| Neue Funktionen | Seite nicht erreichbar |
| Design-Änderungen | Formular funktioniert nicht |
| Strukturelle Anpassungen | Rechtlich notwendige Änderungen |
| Performance-Optimierungen | Falsche Kontaktdaten |

### Ablauf eines Quartals

```
Woche 1-8: Sammlung
└─► Issues erstellen
└─► Anforderungen klären
└─► Priorisierung

Woche 9-10: Entwicklung
└─► Umsetzung der Issues
└─► Tests auf Staging

Woche 11-12: Review & Release
└─► Abnahme durch Team
└─► Korrekturen
└─► Go-Live
```

### Fristen für Issue-Einreichung

| Für Release in | Issue einreichen bis |
|----------------|---------------------|
| Q1 (Ende März) | 15. Februar |
| Q2 (Ende Juni) | 15. Mai |
| Q3 (Ende September) | 15. August |
| Q4 (Ende Dezember) | 15. November |

Issues nach der Frist werden ins nächste Quartal verschoben (ausser Hotfixes).

---

## 6. Dokumentation aendern (docs.bioco.ch)

Die Dokumentations-Website (docs.bioco.ch) ist ein separates Projekt und wird anders verwaltet als die Haupt-Website.

### Wie funktioniert die Doku-Website?

Die Dokumentation wird automatisch aus Markdown-Dateien generiert:

1. Aenderungen werden im GitHub Repository `bioco-doku` vorgenommen
2. GitHub Actions baut die Website automatisch neu
3. Nach 1-2 Minuten ist die Aenderung live auf docs.bioco.ch

### Aenderungen beantragen

Es gibt zwei Wege:

**Option 1: GitHub Issue erstellen**

1. Gehe zu `github.com/[organisation]/bioco-doku`
2. Tab: **Issues**
3. Neues Issue erstellen mit:
   - Welche Seite soll geaendert werden?
   - Was genau soll geaendert werden?
   - Warum ist die Aenderung noetig?

**Option 2: Content Planning Aufgabe**

1. Gehe zu [cms.bioco.ch/processwire/content-planning/](https://cms.bioco.ch/processwire/content-planning/)
2. Neue Aufgabe erstellen
3. Im Titel "DOKU:" voranstellen
4. Beschreibung wie bei GitHub Issue

### Was kann geaendert werden?

| Aenderung | Beispiel |
|-----------|----------|
| Text korrigieren | Tippfehler, veraltete Info |
| Neue Anleitung | Erklaerung fuer neues Feature |
| Struktur anpassen | Seiten verschieben, umbenennen |
| Bilder/Screenshots | Neue oder aktualisierte Bilder |

---

## 7. Zusammenfassung: Entscheidungsbaum

```
Aenderung noetig?
|
+-- Inhalt auf bioco.ch (Texte, Bilder)?
|   +-- Content Planning (cms.bioco.ch)
|
+-- Dokumentation auf docs.bioco.ch?
|   +-- GitHub Issue im bioco-doku Repo
|   +-- ODER Content Planning Aufgabe (mit "DOKU:" Prefix)
|
+-- Technische Aenderung (Code, Design)?
|   +-- GitHub Issue im bioco-web-project Repo
|
+-- Fehler/Bug?
    +-- Kritisch (Seite down, Sicherheit)?
    |   +-- GitHub Issue + direkte Nachricht
    +-- Normal?
        +-- GitHub Issue (naechstes Quartal)
```

---

## 8. Kontakt

| Anliegen | Kontakt |
|----------|---------|
| Content Planning Fragen | [Admin-Team] |
| GitHub Issues (Website) | [Entwickler] |
| GitHub Issues (Doku) | [Entwickler] |
| Dringende Probleme | [Notfall-Kontakt] |

---

*Zuletzt aktualisiert: Januar 2026*
