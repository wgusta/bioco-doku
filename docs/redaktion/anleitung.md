# Anleitung für Redakteure: Inhalte auf bioco.ch bearbeiten

Diese Anleitung zeigt, wie Sie Inhalte auf `bioco.ch` selbstständig ändern. Sie brauchen keine Programmierkenntnisse.

Sie arbeiten mit zwei Werkzeugen:

* **Visual Editor**: das empfohlene Werkzeug. Sie bearbeiten die Seite direkt in einer Live-Vorschau und sehen sofort, wie das Ergebnis aussieht.
* **ProcessWire Admin**: das klassische Backend (`https://cms.bioco.ch/processwire/`). Hier liegen alle Felder im Detail. Der Visual Editor verlinkt für Spezialfälle direkt dorthin.

---

## 1. Anmelden

1. Öffnen Sie `https://cms.bioco.ch/processwire/`.
2. Geben Sie Benutzernamen und Passwort ein, klicken Sie auf **Anmelden**.
3. Klicken Sie oben in der Navigationsleiste auf **Visual Editor**. Der Editor öffnet sich in einem neuen Tab.

---

## 2. Der Visual Editor im Überblick

Der Visual Editor zeigt links eine Seitenleiste und rechts die echte Website als Vorschau.

* **Oben (Werkzeugleiste)**: Moduswahl **Edit** / **Browse**, Schaltflächen **Neu laden**, **Vorlagen**, **PW Admin**, **Zurück** sowie eine Statusanzeige.
* **Links (Seitenleiste)**: aktuelle Seite, Seitenliste zum Springen, Liste der Abschnitte der Seite, Feld-Editor mit der Feldzuordnung.
* **Rechts (Vorschau)**: die Website. Im Modus **Edit** klicken Sie Inhalte direkt an, im Modus **Browse** verhält sich die Vorschau wie die normale Website.

Sie wechseln die Seite, indem Sie in der Vorschau ganz normal navigieren oder links eine Seite aus der Liste wählen.

---

## 3. Text und Felder bearbeiten

1. Stellen Sie sicher, dass der Modus **Edit** aktiv ist.
2. Klicken Sie in der Vorschau auf den Text oder Abschnitt, den Sie ändern möchten. Der Abschnitt wird hervorgehoben.
3. Bearbeiten Sie Titel, Eyebrow oder Fliesstext direkt. Fliesstext öffnet einen kleinen Rich-Text-Editor (fett, Listen, Links).
4. Ihre Änderungen bleiben zunächst als **lokaler Entwurf** im Browser. Erst **Publizieren** macht sie öffentlich.

---

## 4. Wer besitzt welches Feld: Visual Editor oder ProcessWire

Für jeden Abschnitt zeigt die Seitenleiste zwei Gruppen:

* **Visual Editor** (grün): Felder, die Sie direkt in der Vorschau bearbeiten, zum Beispiel Titel, Eyebrow, Text, Layout und Thema, Hintergrundfarbe, Buttons sowie Bilder aus der Mediathek.
* **ProcessWire** (orange): Felder, die in komplexeren Fällen besser im Backend bearbeitet werden, zum Beispiel die eigentliche Bilddatei oder die vollständige Feldansicht. Jede Zeile hat einen Knopf **→ In PW öffnen**, der genau dieses Feld im ProcessWire-Editor öffnet.

So sehen Sie immer, was Sie im Visual Editor erledigen und was ins Backend gehört.

---

## 5. Abschnitte (Blöcke) verwalten

Eine Seite besteht aus **Abschnitten**, die wie Bausteine übereinanderliegen.

* **Hinzufügen**: Knopf **Abschnitt hinzufügen** in der Seitenleiste.
* **Kopieren / Löschen**: die kleinen Symbole an jedem Abschnitt in der Liste.
* **Sortieren**: Abschnitt in der Liste mit der Maus an die neue Position ziehen.

Manche Abschnitte sind **Komponenten** mit besonderem Aussehen, zum Beispiel die Preis-Tabelle auf der Abo-Seite oder die Event-Liste. Deren Optionen erscheinen als zusätzliche Felder im Abschnitt-Overlay. Das Strukturhandbuch listet alle Komponenten auf.

---

## 6. Bilder

Bilder verwalten Sie über die **Mediathek**. Beim Klick auf ein Bild im Visual Editor öffnet sich ein Overlay, in dem Sie ein Bild aus der Mediathek wählen, den Alt-Text setzen und Helligkeit, Kontrast oder Sättigung anpassen. Neue Bilddateien laden Sie in der Mediathek hoch und wählen sie dann aus; der direkte Upload in einzelne Seitenfelder ist bewusst deaktiviert.

---

## 7. Events und Beiträge (Sammlung Aktuelles)

Events sind keine Abschnitte, sondern eigene Seiten unter **Aktuelles**. Navigieren Sie in der Vorschau zu **Aktuelles**. Die Seitenleiste zeigt dann statt der Abschnitte ein **Sammlungs-Panel**:

* Eine Liste aller Events mit Datum und Status (bevorstehend / vergangen). Jeder Eintrag hat **→ In PW öffnen**, um alle Event-Felder im Backend zu bearbeiten.
* Oben **Neuen Event erstellen**: Datum wählen, Knopf drücken. Der Event wird angelegt und direkt in ProcessWire geöffnet, wo Sie Titel, Ort, Beschreibung und Anmeldung ergänzen.

---

## 8. Publizieren und Live-Schaltung

1. Wenn Ihre Änderungen fertig sind, klicken Sie auf **Publizieren**.
2. Die Statusanzeige zeigt das Ergebnis:
   * **Publiziert & live**: gespeichert und auf der Website aktualisiert.
   * **Publiziert, aber Build nicht aktualisiert** (rot): gespeichert, aber die Website hat die Aktualisierung nicht bestätigt. Laden Sie nach kurzer Zeit neu; bleibt es rot, melden Sie sich beim Technik-Team.
3. Mit **Entwurf verwerfen** verwerfen Sie ungespeicherte Änderungen und kehren zum publizierten Stand zurück.

!!! note "Warum nicht sofort sichtbar?"
    Die Website ist aus Geschwindigkeitsgründen zwischengespeichert. Beim Publizieren stösst das CMS gezielt eine Aktualisierung der betroffenen Seiten an (On-Demand-Revalidation). In der Regel erscheint die Änderung innerhalb weniger Sekunden.

---

## 9. Wann doch direkt in ProcessWire?

Nutzen Sie das ProcessWire Admin (oder den Knopf **→ In PW öffnen**), wenn Sie:

* eine neue **Bilddatei** hochladen oder ersetzen,
* alle Felder eines Events bearbeiten,
* Formular-Einsendungen ansehen oder exportieren,
* Seiten anlegen oder löschen, die es im Visual Editor noch nicht gibt.

Für die alltägliche Textpflege bleibt der Visual Editor der schnellste Weg.

---

*Zuletzt aktualisiert: Juni 2026*
