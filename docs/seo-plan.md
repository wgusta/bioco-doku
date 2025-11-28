# SEO Plan für biocò.ch

!!! info "Was ist dieser Plan?"
    Dieses Dokument ist unsere Auslegeordnung für die Google-Suche.
    Es hilft euch beim Schreiben (Redakteure) und beim Coden (Entwickler).

## Übersicht

Wir wollen, dass Menschen in der Region Baden-Brugg uns finden, wenn sie nach gesundem, regionalem Gemüse suchen. Dafür setzen wir auf eine Kombination aus technischer Optimierung und guten Inhalten.

## 1. Suchbegriffe & Kennzahlen (Keywords)

Hier sehen wir, wonach Menschen tatsächlich suchen. Das hilft uns, die richtigen Worte in unseren Texten zu verwenden. Ich kann zwar aktuell nicht sagen, wie sich die Zahlen nächsten Monat entwickeln, aber diese Schätzungen geben eine gute Richtung vor.

### Google Search Metrics (Geschätzt für die Schweiz)

| Suchbegriff | Relevanz | Suchvolumen (geschätzt) | Erklärung |
| :--- | :--- | :--- | :--- |
| **Gemüseabo** | Hoch | ~1'000 - 3'000 / Mt. | Der häufigste Begriff für unser Angebot. |
| **Bio Gemüse** | Hoch | ~1'000 - 5'000 / Mt. | Sehr allgemein, aber wichtig. |
| **Solidarische Landwirtschaft** | Mittel | ~500 - 1'000 / Mt. | Unser Kernkonzept (Solawi). |
| **Demeter Gemüse** | Mittel | ~200 - 500 / Mt. | Zeigt hohes Qualitätsbewusstsein. |
| **Gemüseabo Baden** | Sehr Hoch | ~50 - 100 / Mt. | Weniger Suchen, aber genau unsere Zielgruppe! |
| **Gemüseabo Brugg** | Sehr Hoch | ~20 - 50 / Mt. | Ebenfalls unsere direkte Zielgruppe. |

!!! tip "Tipp für Redakteure"
    Verwendet diese Begriffe natürlich in Überschriften und Texten. Schreibt aber immer für Menschen, nicht für Suchmaschinen!

---

## 2. Inhalte (On-Page SEO)

Das wichtigste für Google sind gute, relevante Inhalte. Und wenn ich so frech sein darf: Gerne sehe ich lebendige Texte, die Lust auf unser Gemüse machen. Das klingt spannend! Geht das? 🙂

### 2.1 Wichtige Seiten & ihre Ziele

*   **Homepage (`/`):** Das "Schaufenster". Muss klar machen: Wer sind wir? (Gemüsegenossenschaft), Wo sind wir? (Baden-Brugg/Geisshof), Was bieten wir? (Gemüseabo/Solawi).
*   **Ernte (`/ernte`):** Zeigt, was gerade wächst. Wichtig für Begriffe wie "Saisonales Gemüse".
*   **Abos (`/abos`):** Hier entscheiden sich die Leute. Klarer Fokus auf "Gemüseabo" und "Wöchentlicher Korb".
*   **Depots (`/depots`):** Entscheidend für die lokale Suche ("Gemüse abholen Baden").
*   **Wir (`/wir`):** Baut Vertrauen auf. "Genossenschaft", "Team", "Geisshof".

### 2.2 Bilder optimieren

Google kann Bilder nicht "sehen", sondern nur lesen, wie wir sie beschreiben.

!!! check "Aufgabe für Redakteure"
    *   Gebt jedem Bild einen sinnvollen Dateinamen **bevor** ihr es hochladet (z.B. `karotten-ernte-geisshof.jpg` statt `IMG_1234.JPG`).
    *   Füllt (wenn technisch möglich) den "Alt-Text" (Alternativtext) aus. Beschreibt kurz, was auf dem Bild zu sehen ist.

---

## 3. Technische Basis (Technical SEO)

Dies ist der technische Teil, hier habe ich (Güney) die Basis gelegt. Da unsere Website nicht so kompliziert ist, sind das nur einige Grundkonfigurationen, die beim definitiven Aufschalten der Website einmalig gemacht werden müssen. Der Vollständigkeit halber: Das muss gemacht werden. Dieser Teil ist vor allem für unser Entwickler-Team wichtig. Auch als digitaler Projektleiter finde ich: Das braucht es weiterhin.

!!! abstract "Technische Details (für Entwickler)"
    Die Website ist technisch bereits sehr gut aufgestellt:
    
    *   **Server-Side Rendering (SSR):** Inhalte sind sofort für Google lesbar.
    *   **Performance:** Schnelle Ladezeiten durch Next.js Optimierungen.
    *   **Mobile First:** Die Seite funktioniert perfekt auf Handys (wichtiges Ranking-Signal).
    *   **Strukturierte Daten:** Wir sagen Google im Code explizit "Wir sind eine lokale Organisation in Gebenstorf" (Schema.org).

### Meta-Tags (Die Vorschau in Google)

Jede Seite hat einen Titel und eine Beschreibung, die in den Suchergebnissen angezeigt wird.

*   **Titel:** Kurz und knackig (max. 60 Zeichen).
*   **Beschreibung:** Ein werbender Satz, der zum Klicken anregt (max. 160 Zeichen).

**Beispiel Homepage:**
> **Titel:** biocò | Bio-Gemüse aus der Region Baden-Brugg
>
> **Beschreibung:** Gemüsegenossenschaft biocò: Frisches Demeter-Gemüse aus solidarischer Landwirtschaft. Wöchentliche Gemüsekörbe vom Geisshof in Gebenstorf.

---

## 4. Nächste Schritte & Roadmap

Was wir noch tun müssen, um noch besser gefunden zu werden. Damit könnt ihr dann selbst oder mit externen Partnern klare Projekte definieren und aufgleisen.

### Kurzfristig (Sofort bis 2 Wochen)
*   [ ] **Sitemap & Robots.txt:** Technisches "Inhaltsverzeichnis" für Google erstellen.
*   [ ] **Bilder-Texte:** Alle Bilder auf der Seite prüfen und beschriftet (Alt-Texte).

### Mittelfristig (1-3 Monate)
*   [ ] **Google Search Console:** Einrichten, um zu sehen, über welche Begriffe wir tatsächlich gefunden werden.
*   [ ] **Content-Updates:** Regelmäßig Neuigkeiten (Hofpost) veröffentlichen. Google mag "lebendige" Seiten.
*   [ ] **Verlinkungen:** Partner und lokale Verzeichnisse bitten, auf biocò.ch zu verlinken.

### Langfristig
*   [ ] **FAQ-Sektion:** Fragen beantworten, die Leute oft stellen (z.B. "Wie funktioniert Solawi?").
*   [ ] **Analyse:** Regelmäßig prüfen, ob die Strategie aufgeht und nachjustieren. Bis Ende Jahr schauen wir sicher mal ohne Kosten (nur Zeit von uns), danach weiss ich noch nicht, wie es aussieht und wir müssten dann situativ schauen.

---

## 5. Technische Checkliste (Status Quo)

| Feature | Status | Erklärung |
| :--- | :--- | :--- |
| **Server-Side Rendering** | ✅ Fertig | Google kann alles lesen. |
| **Mobile-Optimierung** | ✅ Fertig | Handyfreundlich. |
| **Strukturierte Daten** | ✅ Fertig | Infos für Google aufbereitet. |
| **Sitemap.xml** | ⏳ Offen | Inhaltsverzeichnis fehlt noch. |
| **Robots.txt** | ⏳ Offen | Wegweiser für Google fehlt noch. |
| **Bilder-Texte (Alt)** | ⏳ Offen | Müssen noch gepflegt werden. |
