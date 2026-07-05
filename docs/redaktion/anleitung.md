# Anleitung: Inhalte auf bioco.ch bearbeiten

Diese Anleitung zeigt, wie du Inhalte auf `bioco.ch` selbstständig änderst. Du brauchst keine Programmierkenntnisse.

**Jede Seite der Website ist im CMS bearbeitbar.** Es gibt keine Texte mehr, die fest im Code stehen. Einzige Ausnahme: `/doi-confirm`, die technische Bestätigungsseite für Newsletter-Anmeldungen — sie hat keinen redaktionellen Inhalt.

Du arbeitest mit zwei Werkzeugen:

* **Visual Editor** ([cms.bioco.ch/visual-editor/](https://cms.bioco.ch/visual-editor/)): das empfohlene Werkzeug. Du bearbeitest die Seite direkt in einer Live-Vorschau und siehst sofort, wie das Ergebnis aussieht.
* **ProcessWire Admin** ([cms.bioco.ch/processwire/](https://cms.bioco.ch/processwire/)): das klassische Backend. Hier liegen alle Felder im Detail. Der Visual Editor verlinkt für Spezialfälle direkt dorthin.

---

## 1. Anmelden

1. Öffne `https://cms.bioco.ch/processwire/`.
2. Gib Benutzernamen und Passwort ein, klicke auf **Anmelden**.
3. Klicke oben in der Navigationsleiste auf **Visual Editor**. Der Editor öffnet sich in einem neuen Tab.

---

## 2. Der Visual Editor im Überblick

Der Visual Editor wurde im Sommer 2026 neu gebaut. Das Konzept ist gleich geblieben — auf die Seite klicken, direkt oder in der Seitenleiste bearbeiten, publizieren — aber er reagiert schneller und geht sorgfältiger mit deinen Entwürfen um.

* **Oben (Werkzeugleiste)**: Moduswahl **Edit** / **Browse**, Schaltflächen **Neu laden**, **Vorlagen**, **PW Admin**, **Zurück** sowie eine Statusanzeige.
* **Links (Seitenleiste)**: aktuelle Seite, Liste der Abschnitte, Feld-Editor mit der Feldzuordnung, unten **Entwurf verwerfen** und **Publizieren**.
* **Rechts (Vorschau)**: die echte Website. Im Modus **Edit** klickst du Inhalte direkt an, im Modus **Browse** verhält sich die Vorschau wie die normale Website.

---

## 3. Eine Seite finden

Du wechselst die Seite, indem du **in der Vorschau ganz normal navigierst** — über das Menü, Links oder Buttons, genau wie auf der echten Website. Eine separate Seitenauswahl gibt es bewusst nicht: was du in der Vorschau siehst, bearbeitest du auch.

Die Seitenleiste zeigt immer an, auf welcher Seite du gerade bist und ob sie bearbeitbar ist.

---

## 4. Text und Felder bearbeiten

1. Stelle sicher, dass der Modus **Edit** aktiv ist.
2. Klicke in der Vorschau auf den Text oder Abschnitt, den du ändern möchtest. Der Abschnitt wird hervorgehoben.
3. Bearbeite Titel, Eyebrow oder Fliesstext direkt. Fliesstext öffnet einen kleinen Rich-Text-Editor (fett, Listen, Links).
4. Deine Änderungen bleiben zunächst als **lokaler Entwurf** im Browser. Erst **Publizieren** macht sie öffentlich.

Mit `Ctrl+Z` (Mac: `Cmd+Z`) machst du eine Änderung rückgängig, mit `Shift` dazu stellst du sie wieder her.

---

## 5. Wer besitzt welches Feld: Visual Editor oder ProcessWire

Für jeden Abschnitt zeigt die Seitenleiste zwei Gruppen:

* **Visual Editor** (grün): Felder, die du direkt in der Vorschau bearbeitest — Titel, Eyebrow, Text, Layout und Farbschema, Buttons sowie Bilder aus der Mediathek.
* **ProcessWire** (orange): Felder, die besser im Backend bearbeitet werden, zum Beispiel die eigentliche Bilddatei oder die vollständige Feldansicht. Jede Zeile hat einen Knopf **→ In PW öffnen**, der genau dieses Feld im ProcessWire-Editor öffnet — dort werden alle anderen Felder ausgeblendet, damit du dich nicht verirrst.

**→ In PW öffnen** funktioniert nur ohne offenen Entwurf. Publiziere zuerst oder verwirf den Entwurf.

---

## 6. Abschnitte (Blöcke) verwalten

Eine Seite besteht aus **Abschnitten**, die wie Bausteine übereinanderliegen. Jeder Abschnitt hat Titel, Text, Bild, Layout, Farbschema und bis zu zwei Buttons — alles deutsch beschriftet, alles im Visual Editor änderbar.

* **Hinzufügen**: Knopf **Abschnitt hinzufügen** in der Seitenleiste, oder **Vorlagen** in der Werkzeugleiste für vorgefertigte Bausteine.
* **Kopieren / Löschen**: die kleinen Symbole an jedem Abschnitt in der Liste.
* **Sortieren**: Abschnitt in der Liste mit der Maus an die neue Position ziehen.

Manche Abschnitte sind **Komponenten**: interaktive Bausteine wie Formulare, Karten, der Saisonkalender, Akkordeon-Einträge, nummerierte Schritte, Portal-Kacheln oder die Event-Liste. Du platzierst und konfigurierst sie im Visual Editor (die Optionen erscheinen als zusätzliche Felder). Was in den Bausteinen selbst steht — etwa die Formulartexte — pflegt das Technik-Team. Das [Strukturhandbuch](strukturhandbuch.md) listet alle Komponenten auf.

---

## 7. Bilder

Bilder verwaltest du über die **Mediathek**. Beim Klick auf ein Bild im Visual Editor öffnet sich ein Overlay, in dem du ein Bild aus der Mediathek wählst, den Alt-Text setzt und Helligkeit, Kontrast oder Sättigung anpasst. Neue Bilddateien lädst du in der Mediathek hoch und wählst sie dann aus; der direkte Upload in einzelne Seitenfelder ist bewusst deaktiviert.

---

## 8. Events und Beiträge (Sammlung Aktuelles)

Events sind keine Abschnitte, sondern eigene Seiten unter **Aktuelles**. Navigiere in der Vorschau zu **Aktuelles**. Die Seitenleiste zeigt dann statt der Abschnitte ein **Sammlungs-Panel**:

* Eine Liste aller Events mit Datum und Status (bevorstehend / vergangen). Jeder Eintrag hat **→ In PW öffnen**, um alle Event-Felder im Backend zu bearbeiten.
* Oben **Neuen Event erstellen**: Datum wählen, Knopf drücken. Der Event wird angelegt und direkt in ProcessWire geöffnet, wo du Titel, Ort, Beschreibung und Anmeldung ergänzt.

Einleitungstext und Abschluss-Abschnitt der Aktuelles-Seite selbst bearbeitest du ganz normal als Abschnitte.

---

## 9. Publizieren und Live-Schaltung

1. Wenn deine Änderungen fertig sind, klicke auf **Publizieren**.
2. Die Statusanzeige zeigt das Ergebnis:
   * **Publiziert & live**: gespeichert und auf der Website aktualisiert.
   * **Publiziert, aber Build nicht aktualisiert** (rot): gespeichert, aber die Website hat die Aktualisierung nicht bestätigt. Lade nach kurzer Zeit neu; bleibt es rot, melde dich beim Technik-Team.
3. Mit **Entwurf verwerfen** verwirfst du ungespeicherte Änderungen und kehrst zum publizierten Stand zurück.

Hat jemand anderes die Seite inzwischen geändert, meldet der Editor den Konflikt und fragt pro Feld, ob deine Version oder die vom Server gelten soll.

!!! note "Warum nicht sofort sichtbar?"
    Die Website ist aus Geschwindigkeitsgründen zwischengespeichert. Beim Publizieren stösst das CMS gezielt eine Aktualisierung der betroffenen Seiten an. In der Regel erscheint die Änderung innerhalb weniger Sekunden.

---

## 10. Wann doch direkt in ProcessWire?

Nutze das ProcessWire Admin (oder den Knopf **→ In PW öffnen**), wenn du:

* eine neue **Bilddatei** hochladen oder ersetzen willst,
* alle Felder eines Events bearbeiten willst,
* Formular-Einsendungen ansehen oder exportieren willst,
* Seiten anlegen oder löschen willst.

Für die alltägliche Textpflege bleibt der Visual Editor der schnellste Weg.

---

*Zuletzt aktualisiert: Juli 2026*
