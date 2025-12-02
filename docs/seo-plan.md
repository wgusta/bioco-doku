# SEO Plan für biocò.ch

!!! info "Über dieses Dokument"
    Dies ist die zentrale Strategie für die Suchmaschinenoptimierung (SEO) von biocò.ch. 
    Es dient als Leitfaden für **Redaktion** (Inhalte) und **Entwicklung** (Technik).

    **Ziel:** Menschen in der Region Baden-Brugg sollen uns finden, wenn sie nach regionalem Bio-Gemüse suchen.

---

## 📚 SEO-Grundlagen (für Einsteiger)

Suchmaschinenoptimierung kann komplex wirken, basiert aber auf einfachen Prinzipien. Hier sind die wichtigsten Konzepte für unser Projekt, basierend auf den [offiziellen Google Richtlinien](https://developers.google.com/search/docs/fundamentals/seo-starter-guide?hl=de).

### 🔍 Suchintention (Search Intent)

Das Wichtigste ist nicht das Keyword selbst, sondern *warum* jemand sucht.

*   **Informational:** "Was wächst im März?" → Will Infos → Landingpage: `/gemuese`
*   **Transactional:** "Gemüseabo kaufen" → Will kaufen → Landingpage: `/abos`
*   **Navigational:** "bioco login" → Will zu uns → Homepage

### 👑 Content is King

Google liebt Inhalte, die für Menschen geschrieben sind, nicht für Maschinen.

*   Schreibe natürlich und hilfreich.
*   Beantworte Fragen der Nutzer.
*   Vermeide "Keyword-Stuffing" (unnötiges Wiederholen von Begriffen).

### 📍 Local SEO

Für uns als lokale Genossenschaft entscheidend.

*   Leute suchen "in meiner Nähe" oder "in Baden".
*   Google nutzt Standortdaten.
*   Wichtig: Konsistente Adressdaten auf der Website.

### ⚡ Technische Basis

Die beste Website nützt nichts, wenn Google sie nicht lesen kann.

*   Schnelle Ladezeiten.
*   Mobilfreundlichkeit (Mobile First).
*   Saubere Struktur (Sitemap, robots.txt).

!!! tip "Google Empfehlung"
    > "Erstellen Sie Inhalte in erster Linie für Nutzer, nicht für Suchmaschinen."
    > 
    > – [Google Search Central](https://developers.google.com/search/docs/fundamentals/creating-helpful-content?hl=de)

---

## 1. Keyword-Strategie

Unsere Analyse zeigt, dass wir uns auf **Nischen-Begriffe** und **lokale Suche** konzentrieren sollten. Gegen grosse nationale Anbieter ("Bio Gemüse Schweiz") zu konkurrieren ist schwer und ineffizient.

### Fokus-Keywords (Primär)

Diese Begriffe beschreiben unser Kernangebot und haben die höchste Relevanz.

| Suchbegriff | Suchvolumen/Mt. | Intention | Zielseite |
|:------------|:---------------:|:----------|:----------|
| **Gemüseabo** | Hoch | Kauf | `/abos` |
| **Bio Gemüse** | Hoch | Kauf | `/abos` |
| **Solidarische Landwirtschaft** | Mittel | Info | `/solawi`, `/wir` |
| **Solawi** (Abkürzung) | Mittel | Info | `/solawi` |
| **Gemüseabo Baden/Brugg** | Nische (Lokal) | Kauf | `/abos`, `/standorte-depots` |

### Ergänzende Keywords (Sekundär)

Wichtige Variationen, die wir im Textfluss verwenden.

| Suchbegriff | Kontext | Zielseite |
|:------------|:--------|:----------|
| **Bio Bauernhof** | Herkunft betonen | `/wir`, `/standorte-depots` |
| **Bio Gemüse Kiste/Lieferung** | Synonyme für Abo | `/abos` |
| **Saisonales Gemüse** | Zeitbezug | `/gemuese` |
| **Regional einkaufen** | Wertebezug | Homepage |

!!! warning "Vermeide Kannibalisierung"
    Versuche nicht, mit *jeder* Seite für *jedes* Keyword zu ranken. 
    **Eine Seite = Ein Hauptthema.**

---

## 2. Content-Planung (On-Page SEO)

Damit Google unsere Seiten richtig einordnet, brauchen sie eine klare Struktur. Hier ist der Plan für die wichtigsten Seiten inkl. vollständiger Meta-Daten.

### 🏠 Homepage (`/`)

**Sichtbarer Name:** Home  
**URL:** `/`  
**Navigation:** Hauptmenü

*   **Ziel:** Überblick & Navigation.
*   **Fokus-Keywords:** "Gemüsegenossenschaft", "Baden-Brugg", "Geisshof"
*   **Meta-Titel:** `biocò | Bio-Gemüse aus der Region Baden-Brugg`
*   **Meta-Beschreibung:** `Gemüsegenossenschaft biocò: Frisches Demeter-Gemüse aus solidarischer Landwirtschaft. Wöchentliche Gemüsekörbe vom Geisshof in Gebenstorf.`
*   **Google-Tipp:** Klare `<h1>` Überschrift und beschreibende Texte "above the fold" (ohne Scrollen sichtbar).

### 🥬 Gemüse (`/gemuese`)

**Sichtbarer Name:** Gemüse  
**URL:** `/gemuese` (⚠️ vorher `/ernte`)  
**Navigation:** Hauptmenü

*   **Ziel:** Informationssuchende abholen.
*   **Fokus-Keywords:** "Saisonales Gemüse", "Saisonkalender", "Was wächst jetzt"
*   **Meta-Titel:** `Saisonales Demeter Gemüse | Was wächst gerade | biocò`
*   **Meta-Beschreibung:** `Entdecke unser saisonales Bio-Gemüse in Demeter-Qualität. Frisch vom Geisshof in Gebenstorf für die Region Baden-Brugg.`
*   **Strategie:** Monatliche Aktualität signalisiert Google "frischen Content".
*   **⚠️ Migration:** 301-Redirect von `/ernte` → `/gemuese` erforderlich

### 📦 Abos (`/abos`)

**Sichtbarer Name:** Abos  
**URL:** `/abos`  
**Navigation:** Hauptmenü

*   **Ziel:** Conversion (Mitglied werden).
*   **Fokus-Keywords:** "Gemüseabo", "Bio Gemüse", "Gemüseabo Baden/Brugg"
*   **Sekundäre Keywords:** "Bio Gemüse Kiste", "Bio Gemüse Lieferung", "Biogemüse bestellen"
*   **Meta-Titel:** `Gemüseabo Baden | Demeter Gemüse wöchentlich | biocò`
*   **Meta-Beschreibung:** `Gemüseabo für die Region Baden-Brugg: Wöchentlich frisches Bio-Gemüse in Demeter-Qualität. Solidarische Landwirtschaft vom Geisshof Gebenstorf.`
*   **Strategie:** Klare Handlungsaufforderungen (CTAs) und Trust-Elemente.

### 📍 Standorte (`/standorte-depots`)

**Sichtbarer Name:** Standorte  
**URL:** `/standorte-depots` (⚠️ vorher `/depots`)  
**Navigation:** Hauptmenü

*   **Ziel:** Lokale Auffindbarkeit.
*   **Fokus-Keywords:** "Gemüse abholen [Ort]", "Depot Baden/Brugg"
*   **Sekundäre Keywords:** "Bio Bauernhof" (im Kontext von Abholstationen)
*   **Meta-Titel:** `Standorte & Depots Baden-Brugg | Gemüse abholen | biocò`
*   **Meta-Beschreibung:** `Gemüseabholung in Baden, Brugg und Umgebung. Finde dein Depot für frisches Bio-Gemüse aus solidarischer Landwirtschaft vom Geisshof Gebenstorf.`
*   **Strategie:** Adressen als Text (nicht nur Bild) hinterlegen für Local SEO.
*   **⚠️ Migration:** 301-Redirect von `/depots` → `/standorte-depots` erforderlich

### 👥 Wir (`/wir`)

**Sichtbarer Name:** Wir  
**URL:** `/wir`  
**Navigation:** Hauptmenü

*   **Ziel:** Vertrauen aufbauen, Geschichte erzählen.
*   **Fokus-Keywords:** "Bio Bauernhof", "Genossenschaft", "Team", "Geisshof"
*   **Sekundäre Keywords:** "Solidarische Landwirtschaft" (mit Link zu `/solawi`)
*   **Meta-Titel:** `Über uns | Bio Bauernhof Baden | biocò Gemüsegenossenschaft`
*   **Meta-Beschreibung:** `biocò Gemüsegenossenschaft: Seit 2014 bewirtschaften wir einen Bio Bauernhof auf dem Geisshof Gebenstorf. Demeter-zertifiziertes Gemüse für Baden-Brugg.`
*   **Strategie:** Natürliche Erwähnung "Wir bewirtschaften einen Bio Bauernhof..." im Text.
*   **🔗 Wichtig:** Link zu `/solawi` einfügen (z.B. "Mehr über solidarische Landwirtschaft erfahren")

### 🚜 Solawi (`/solawi`)

**Sichtbarer Name:** Solidarische Landwirtschaft  
**URL:** `/solawi`  
**Navigation:** ⚠️ **KEINE** (Orphaned Page - nur via Link von `/wir`)

*   **Ziel:** Aufklärung & Konzept-Erklärung.
*   **Fokus-Keywords:** "Solidarische Landwirtschaft", "Solawi", "SoLaWi"
*   **Meta-Titel:** `Was ist Solidarische Landwirtschaft (SoLaWi)? | biocò`
*   **Meta-Beschreibung:** `Solidarische Landwirtschaft (Solawi/SoLaWi): Gemeinsam Verantwortung tragen für regionales Bio-Gemüse. Erfahre mehr über unser Konzept auf dem Geisshof.`
*   **Strategie:** Ausführliche Texte ("Long Form Content", 500+ Wörter) ranken oft gut für Erklär-Suchanfragen.
*   **🔗 Wichtig:** Diese Seite erscheint NICHT im Hauptmenü, wird aber von `/wir` verlinkt.

---

## 3. Technische Checkliste

Diese technischen Maßnahmen stellen sicher, dass Google die Seite optimal indizieren kann.

### 🤖 Indexierung
*   [ ] **robots.txt:** Weist Crawler an, was sie besuchen dürfen. [Google Doku](https://developers.google.com/search/docs/crawling-indexing/robots/intro?hl=de).
*   [ ] **sitemap.xml:** Eine Landkarte aller Seiten für Google. [Google Doku](https://developers.google.com/search/docs/crawling-indexing/sitemaps/overview?hl=de).

### 🏷️ Meta-Daten (Snippets)
Das sind die Texte, die in den Google-Suchergebnissen erscheinen. Sie müssen zum Klicken anregen ("Click-Through-Rate").

!!! quote "Beispiel: Solawi Seite"
    **Titel:** Was ist Solidarische Landwirtschaft (SoLaWi)? | biocò
    **Beschreibung:** Solidarische Landwirtschaft: Gemeinsam Verantwortung tragen für regionales Bio-Gemüse. Erfahre mehr über unser Konzept auf dem Geisshof.

### 🧩 Strukturierte Daten (Schema.org)
Wir helfen Google, den Inhalt maschinenlesbar zu verstehen.

*   **Typ:** `LocalBusiness` oder `Organization`.
*   **Nutzen:** Kann zu "Rich Snippets" führen (z.B. Anzeige von Logo und Adresse direkt in der Suche).
*   **Referenz:** [Google zu strukturierten Daten](https://developers.google.com/search/docs/appearance/structured-data/intro-structured-data?hl=de).

### 🖼️ Bilder-SEO
*   **Dateinamen:** Sprechend wählen (`karotten-baden.jpg` statt `IMG_001.jpg`).
*   **Alt-Text:** Bildinhalt kurz beschreiben (Barrierefreiheit & SEO).

---

## 4. Monitoring & Tools

SEO ist kein einmaliges Projekt, sondern ein Prozess. Wir nutzen folgende Tools zur Überwachung:

1.  **Google Search Console:** Das wichtigste Tool. Zeigt an:
    *   Wie oft wir in der Suche erscheinen.
    *   Über welche Begriffe Besucher kommen.
    *   Technische Fehler auf der Seite.
    *   [Zur Search Console](https://search.google.com/search-console/about?hl=de)

2.  **Matomo Analytics:**
    *   Zeigt Nutzerverhalten *auf* der Seite (Datenschutzfreundlich).
    *   Hilft zu verstehen, welche Inhalte funktionieren.

---

## ✅ Nächste Schritte (ToDo)

| Aufgabe | Bereich | Prio |
|:--------|:--------|:-----|
| **Content:** Keywords in neue Texte integrieren | Redaktion | Hoch |
| **Tech:** `sitemap.xml` & `robots.txt` erstellen | Entwicklung | Hoch |
| **Tech:** Schema.org (JSON-LD) implementieren | Entwicklung | Mittel |
| **Setup:** Search Console & Matomo verbinden | Admin | Mittel |
| **Content:** Bilder mit Alt-Texten versehen | Redaktion | Laufend |

!!! success "Fazit"
    Unser größter Hebel ist nicht technisches Micro-Management, sondern **relevanter, lokaler Content**, der die Fragen unserer Zielgruppe beantwortet. Die Technik muss lediglich sicherstellen, dass dieser Content für Google barrierefrei zugänglich ist.
