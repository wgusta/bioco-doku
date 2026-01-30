# Technische API-Dokumentation

Diese Dokumentation beschreibt die REST API der bioco.ch Webseite im Detail. Sie richtet sich an Entwickler, die das System erweitern oder warten.

---

## 1. Architektur-Übersicht

### Headless CMS Konzept

Die bioco.ch Webseite verwendet ein Headless CMS Setup:

- **Backend**: ProcessWire (PHP) stellt Inhalte via REST API bereit
- **Frontend**: Next.js (React/TypeScript) konsumiert die API und rendert die Webseite
- **Kommunikation**: JSON über HTTP/HTTPS

```
┌─────────────────────────────────────────────────────────────────┐
│                         FRONTEND                                │
│  ┌─────────────────┐    ┌─────────────────┐                    │
│  │  Next.js Pages  │    │  Next.js API    │                    │
│  │  (SSR/SSG)      │    │  Routes         │                    │
│  └────────┬────────┘    └────────┬────────┘                    │
│           │                      │                              │
│           ▼                      ▼                              │
│  ┌─────────────────────────────────────────┐                   │
│  │     lib/processwire.ts (API Client)     │                   │
│  └─────────────────────────────────────────┘                   │
└─────────────────────────┬───────────────────────────────────────┘
                          │ HTTP Request
                          ▼
┌─────────────────────────────────────────────────────────────────┐
│                         BACKEND                                 │
│  ┌─────────────────────────────────────────┐                   │
│  │     site/api/.htaccess (URL Routing)    │                   │
│  └─────────────────────────────────────────┘                   │
│           │                                                     │
│           ▼                                                     │
│  ┌──────────┬──────────┬──────────┬──────────┐                 │
│  │ pages.php│navigation│ forms.php│  doi.php │                 │
│  │          │   .php   │          │          │                 │
│  └──────────┴──────────┴────┬─────┴──────────┘                 │
│                             │                                   │
│                             ▼                                   │
│  ┌─────────────────────────────────────────┐                   │
│  │     ProcessWire Modules                 │                   │
│  │  ┌─────────────┐  ┌─────────────┐      │                   │
│  │  │FormProcessor│  │ DOIManager  │      │                   │
│  │  └─────────────┘  └─────────────┘      │                   │
│  └─────────────────────────────────────────┘                   │
└─────────────────────────────────────────────────────────────────┘
```

### URL Routing

Die API-Endpunkte werden via Apache `.htaccess` auf die entsprechenden PHP-Dateien geroutet.

**Datei**: `site/api/.htaccess`

```apache
RewriteEngine On
RewriteBase /api/

# Seiteninhalte
RewriteCond %{REQUEST_URI} ^/api/pages
RewriteRule ^pages$ pages.php [L]

# Navigation
RewriteCond %{REQUEST_URI} ^/api/navigation
RewriteRule ^navigation$ navigation.php [L]

# Formulare (contact, subscribe, visit, waiting-list)
RewriteCond %{REQUEST_URI} ^/api/forms/(contact|subscribe|visit|waiting-list)
RewriteRule ^forms/(.*)$ forms.php?form_type=$1 [L]

# Double Opt-In Bestätigung
RewriteCond %{REQUEST_URI} ^/api/doi/confirm
RewriteRule ^doi/confirm$ doi.php?action=confirm [L]
```

### CORS Headers

Alle API-Endpunkte setzen folgende CORS-Header:

```php
header('Content-Type: application/json');
header('Access-Control-Allow-Origin: *');
header('Access-Control-Allow-Methods: GET');  // oder POST je nach Endpunkt
header('Access-Control-Allow-Headers: Content-Type');
```

---

## 2. API-Endpunkte

### 2.1 Pages API

**Endpunkt**: `GET /api/pages`

**Datei**: `site/api/pages.php`

**Beschreibung**: Liefert Seitendaten als JSON für das Frontend.

#### Request

| Parameter | Typ | Pflicht | Beschreibung |
|-----------|-----|---------|--------------|
| `path` | string | Nein | Pfad zur Seite (default: `/`) |

**Beispiel-Requests**:
```
GET /api/pages?path=/
GET /api/pages?path=/ernte
GET /api/pages?path=/wir
```

#### Response (Erfolg)

```json
{
  "id": 1,
  "title": "Willkommen bei biocò",
  "url": "/",
  "body": "<p>Inhalt der Seite...</p>",
  "hero_image": {
    "url": "/site/assets/files/1/hero.jpg",
    "description": "Gemüsefeld auf dem Geisshof"
  },
  "hero_subtitle": "Frisches Demeter-Gemüse",
  "sidebar_content": "<p>Sidebar-Inhalt...</p>",
  "gallery_images": [
    {
      "url": "/site/assets/files/1/bild1.jpg",
      "description": "Beschreibung 1"
    },
    {
      "url": "/site/assets/files/1/bild2.jpg",
      "description": "Beschreibung 2"
    }
  ],
  "footer_content": "<p>Footer-Inhalt...</p>",
  "css_variant": "default"
}
```

#### Response (Fehler)

**HTTP 404**:
```json
{
  "error": "Page not found"
}
```

#### Feld-Mapping

| JSON-Feld | ProcessWire-Feld | Typ | Beschreibung |
|-----------|------------------|-----|--------------|
| `id` | `$page->id` | int | Eindeutige Seiten-ID |
| `title` | `$page->title` | string | Seitentitel |
| `url` | `$page->url` | string | URL-Pfad |
| `body` | `$page->body` | string | Hauptinhalt (HTML) |
| `hero_image` | `$page->hero_image` | object | Kopfbild mit URL und Beschreibung |
| `hero_subtitle` | `$page->hero_subtitle` | string | Untertitel |
| `sidebar_content` | `$page->sidebar_content` | string | Seitenleisten-Inhalt |
| `gallery_images` | `$page->gallery_images` | array | Bildergalerie |
| `footer_content` | `$page->footer_content` | string | Fusszeilen-Inhalt |
| `css_variant` | `$page->css_variant` | string | CSS-Variante |

#### Quellcode-Logik

```php
// Pfad aus Query-Parameter holen
$path = $input->get('path', '/');

// Seite laden
if($path === '/') {
    $page = $pages->get('/');
} else {
    $page = $pages->get($path);
}

// 404 wenn Seite nicht existiert
if(!$page->id) {
    http_response_code(404);
    echo json_encode(['error' => 'Page not found']);
    exit;
}

// Pflichtfelder
$pageData = [
    'id' => $page->id,
    'title' => $page->title,
    'url' => $page->url,
    'body' => $page->body ? $page->body : '',
];

// Optionale Felder nur wenn vorhanden
if($page->hero_image) {
    $pageData['hero_image'] = [
        'url' => $page->hero_image->url,
        'description' => $page->hero_image->description,
    ];
}
// ... weitere optionale Felder
```

---

### 2.2 Navigation API

**Endpunkt**: `GET /api/navigation`

**Datei**: `site/api/navigation.php`

**Beschreibung**: Liefert die Navigationsstruktur als Array.

#### Request

Keine Parameter erforderlich.

#### Response

```json
[
  {
    "id": 10,
    "title": "Ernte",
    "url": "/ernte/"
  },
  {
    "id": 11,
    "title": "Abos",
    "url": "/abos/"
  },
  {
    "id": 12,
    "title": "Wir",
    "url": "/wir/"
  }
]
```

#### Quellcode-Logik

```php
$pages = wire('pages');
$home = $pages->get('/');

$navigation = [];

// Alle direkten Kinder der Startseite
if($home->children->count()) {
    foreach($home->children as $child) {
        $navigation[] = [
            'id' => $child->id,
            'title' => $child->title,
            'url' => $child->url,
        ];
    }
}

echo json_encode($navigation);
```

---

### 2.3 Forms API

**Endpunkt**: `POST /api/forms/{type}`

**Datei**: `site/api/forms.php`

**Beschreibung**: Verarbeitet Formular-Einreichungen und initiiert Double Opt-In.

#### Unterstützte Formulartypen

| Typ | URL | Beschreibung |
|-----|-----|--------------|
| `contact` | `/api/forms/contact` | Kontaktformular |
| `subscribe` | `/api/forms/subscribe` | Newsletter-Anmeldung |
| `visit` | `/api/forms/visit` | Besuchstag-Anmeldung |
| `waiting-list` | `/api/forms/waiting-list` | Warteliste |

#### Request Bodies

**Kontaktformular** (`contact`):
```json
{
  "name": "Max Mustermann",
  "email": "max@example.com",
  "phone": "+41 79 123 45 67",
  "subject": "Anfrage zu Mitgliedschaft",
  "message": "Ich interessiere mich für..."
}
```

| Feld | Typ | Pflicht | Validierung |
|------|-----|---------|-------------|
| `name` | string | Ja | Text, sanitized |
| `email` | string | Ja | Gültige E-Mail |
| `phone` | string | Nein | Text, sanitized |
| `subject` | string | Ja | Text, sanitized |
| `message` | string | Ja | Textarea, sanitized |

**Newsletter** (`subscribe`):
```json
{
  "email": "max@example.com",
  "name": "Max Mustermann",
  "privacy_accept": true
}
```

| Feld | Typ | Pflicht | Validierung |
|------|-----|---------|-------------|
| `email` | string | Ja | Gültige E-Mail |
| `name` | string | Nein | Text, sanitized |
| `privacy_accept` | boolean | Ja | Muss `true` sein |

**Besuchstag** (`visit`):
```json
{
  "name": "Max Mustermann",
  "email": "max@example.com",
  "phone": "+41 79 123 45 67",
  "visit_date": "2026-03-15",
  "participants": 2,
  "notes": "Wir kommen mit Kindern",
  "privacy_accept": true
}
```

| Feld | Typ | Pflicht | Validierung |
|------|-----|---------|-------------|
| `name` | string | Ja | Text, sanitized |
| `email` | string | Ja | Gültige E-Mail |
| `phone` | string | Ja | Text, sanitized |
| `visit_date` | string | Ja | Datum (Y-m-d) |
| `participants` | int | Ja | >= 1 |
| `notes` | string | Nein | Textarea, sanitized |
| `privacy_accept` | boolean | Ja | Muss `true` sein |

**Warteliste** (`waiting-list`):
```json
{
  "name": "Max Mustermann",
  "email": "max@example.com",
  "phone": "+41 79 123 45 67",
  "interest": "Ganzes Abo",
  "notes": "Ab Herbst 2026",
  "privacy_accept": true
}
```

| Feld | Typ | Pflicht | Validierung |
|------|-----|---------|-------------|
| `name` | string | Ja | Text, sanitized |
| `email` | string | Ja | Gültige E-Mail |
| `phone` | string | Ja | Text, sanitized |
| `interest` | string | Ja | Text, sanitized |
| `notes` | string | Nein | Textarea, sanitized |
| `privacy_accept` | boolean | Ja | Muss `true` sein |

#### Response (Erfolg)

**HTTP 200**:
```json
{
  "success": true
}
```

#### Response (Fehler)

**HTTP 400**:
```json
{
  "success": false,
  "error": "Bitte füllen Sie alle Pflichtfelder aus."
}
```

Mögliche Fehlermeldungen:
- `"Invalid form type"` (ungültiger Formulartyp)
- `"Bitte füllen Sie alle Pflichtfelder aus."`
- `"Bitte geben Sie eine gültige E-Mail-Adresse ein."`
- `"Bitte akzeptieren Sie die Datenschutzbestimmungen."`
- `"Fehler beim Versenden der E-Mail."`

#### Quellcode-Logik

```php
// Formulartyp aus URL-Segment oder Query-Parameter
$formType = $input->urlSegment1 ?: $input->get('form_type');

// Validierung des Typs
if(!in_array($formType, ['contact', 'subscribe', 'visit', 'waiting-list'])) {
    http_response_code(400);
    echo json_encode(['success' => false, 'error' => 'Invalid form type']);
    exit;
}

// FormProcessor-Modul laden
$formProcessor = $modules->get('FormProcessor');

// JSON-Body parsen
$postData = json_decode(file_get_contents('php://input'), true);

// Je nach Typ verarbeiten
switch($formType) {
    case 'contact':
        $result = $formProcessor->processContactForm((object)$postData);
        break;
    case 'subscribe':
        $result = $formProcessor->processSubscribeForm((object)$postData);
        break;
    // ... weitere Typen
}
```

---

### 2.4 DOI Confirm API

**Endpunkt**: `GET /api/doi/confirm`

**Datei**: `site/api/doi.php`

**Beschreibung**: Bestätigt Double Opt-In Token.

#### Request

| Parameter | Typ | Pflicht | Beschreibung |
|-----------|-----|---------|--------------|
| `token` | string | Ja | 64-Zeichen Hex-Token |

**Beispiel**:
```
GET /api/doi/confirm?token=abc123def456...
```

#### Response (Erfolg)

**HTTP 200**:
```json
{
  "success": true,
  "form_type": "contact"
}
```

#### Response (Fehler)

**HTTP 400**:
```json
{
  "success": false,
  "error": "Ungültiger oder abgelaufener Bestätigungslink."
}
```

Mögliche Fehlermeldungen:
- `"Kein Token angegeben."`
- `"Ungültiger oder abgelaufener Bestätigungslink."`
- `"Invalid action"`

---

## 3. ProcessWire Module

### 3.1 FormProcessor

**Datei**: `site/modules/FormProcessor/FormProcessor.module.php`

**Beschreibung**: Verarbeitet alle Formular-Einreichungen und integriert mit DOIManager.

#### Öffentliche Methoden

| Methode | Parameter | Rückgabe | Beschreibung |
|---------|-----------|----------|--------------|
| `processContactForm($post)` | object | array | Kontaktformular verarbeiten |
| `processSubscribeForm($post)` | object | array | Newsletter-Anmeldung |
| `processVisitDayForm($post)` | object | array | Besuchstag-Anmeldung |
| `processWaitingListForm($post)` | object | array | Warteliste-Anmeldung |
| `finalizeSubmission($token)` | string | array | DOI-Bestätigung abschliessen |

#### Verarbeitungsablauf

```
1. Eingabe-Sanitization
   └─► ProcessWire Sanitizer ($this->sanitizer->text(), ->email(), etc.)

2. Validierung
   └─► Pflichtfelder prüfen
   └─► E-Mail-Format prüfen
   └─► Datenschutz-Akzeptanz prüfen

3. Form-Daten vorbereiten
   └─► Array mit allen Feldern + Timestamp erstellen

4. DOI initiieren
   └─► DOIManager->initiateDOI() aufrufen
   └─► Bestätigungs-E-Mail wird gesendet

5. Tracking (optional)
   └─► MatomoTracker->trackEvent() wenn installiert

6. Rückgabe
   └─► ['success' => true, 'doi_token' => '...'] oder
   └─► ['success' => false, 'error' => '...']
```

#### Nach DOI-Bestätigung

Wenn `finalizeSubmission($token)` aufgerufen wird:

| Formulartyp | Aktion |
|-------------|--------|
| `contact` | Admin-Benachrichtigung per E-Mail |
| `subscribe` | Bestätigungs-E-Mail an Abonnent |
| `visit` | Admin-Benachrichtigung per E-Mail |
| `waiting_list` | Admin-Benachrichtigung per E-Mail |

#### E-Mail Templates

Templates werden aus `site/templates/emails/` geladen:
- `doi-confirmation.php` (Bestätigungs-E-Mail)
- `form-notification.php` (Admin-Benachrichtigung)
- `newsletter-confirmed.php` (Newsletter-Bestätigung)

---

### 3.2 DOIManager

**Datei**: `site/modules/DOIManager/DOIManager.module.php`

**Beschreibung**: Verwaltet Double Opt-In Prozess mit Token-Generierung und Datenbankpersistenz.

#### Datenbank-Tabelle

**Tabelle**: `doi_tokens`

```sql
CREATE TABLE IF NOT EXISTS `doi_tokens` (
    `id` int(11) NOT NULL AUTO_INCREMENT,
    `token` varchar(64) NOT NULL,
    `email` varchar(255) NOT NULL,
    `form_type` varchar(50) NOT NULL,
    `form_data` text NOT NULL,
    `created` int(11) NOT NULL,
    `expires` int(11) NOT NULL,
    `confirmed` tinyint(1) DEFAULT 0,
    `confirmed_at` int(11) DEFAULT NULL,
    PRIMARY KEY (`id`),
    UNIQUE KEY `token` (`token`),
    KEY `email` (`email`),
    KEY `expires` (`expires`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4
```

| Spalte | Typ | Beschreibung |
|--------|-----|--------------|
| `id` | int | Auto-Increment ID |
| `token` | varchar(64) | Eindeutiger Hex-Token |
| `email` | varchar(255) | E-Mail-Adresse |
| `form_type` | varchar(50) | Formulartyp |
| `form_data` | text | JSON-kodierte Formulardaten |
| `created` | int | Unix-Timestamp Erstellung |
| `expires` | int | Unix-Timestamp Ablauf |
| `confirmed` | tinyint | 0 = unbestätigt, 1 = bestätigt |
| `confirmed_at` | int | Unix-Timestamp Bestätigung |

#### Öffentliche Methoden

**`initiateDOI($email, $formType, $formData)`**

Startet DOI-Prozess:
1. Generiert 64-Zeichen Hex-Token (`bin2hex(random_bytes(32))`)
2. Setzt Ablaufzeit auf 24 Stunden
3. Speichert in Datenbank
4. Sendet Bestätigungs-E-Mail

```php
$result = $doiManager->initiateDOI(
    'max@example.com',
    'contact',
    ['name' => 'Max', 'message' => '...']
);
// => ['success' => true, 'token' => 'abc123...']
```

**`confirmDOI($token)`**

Bestätigt Token:
1. Prüft ob Token existiert
2. Prüft ob nicht abgelaufen (`expires > time()`)
3. Prüft ob noch nicht bestätigt (`confirmed = 0`)
4. Markiert als bestätigt
5. Gibt Formulardaten zurück

```php
$data = $doiManager->confirmDOI('abc123...');
// => [
//     'email' => 'max@example.com',
//     'form_type' => 'contact',
//     'form_data' => ['name' => 'Max', ...]
// ]
// oder false wenn ungültig
```

**`cleanupExpiredTokens()`**

Löscht abgelaufene, unbestätigte Tokens. Sollte per Cron aufgerufen werden.

```php
$deleted = $doiManager->cleanupExpiredTokens();
// => Anzahl gelöschter Einträge
```

#### Token-Generierung

```php
private function generateToken() {
    do {
        // 32 Bytes = 64 Hex-Zeichen
        $token = bin2hex(random_bytes(32));
        
        // Eindeutigkeit prüfen
        $exists = /* DB-Abfrage */;
    } while($exists);
    
    return $token;
}
```

---

## 4. Next.js Integration

### 4.1 API Client

**Datei**: `frontend/lib/processwire.ts`

TypeScript-Client für ProcessWire API:

```typescript
const API_URL = process.env.NEXT_PUBLIC_PROCESSWIRE_API_URL 
             || process.env.PROCESSWIRE_API_URL 
             || 'http://localhost/api'

export interface PageData {
  id: number
  title: string
  url: string
  body?: string
  logo_image?: { url: string; description: string }
  hero_image?: { url: string; description: string }
  hero_subtitle?: string
  sidebar_content?: string
  gallery_images?: Array<{ url: string; description: string }>
  footer_content?: string
  css_variant?: string
  children?: PageData[]
}

export async function getPageData(path: string): Promise<PageData | null> {
  const response = await fetch(`${API_URL}/pages${path}`, {
    next: { revalidate: 60 }, // Cache 60 Sekunden
  })
  if (!response.ok) return null
  return await response.json()
}

export async function getNavigation(): Promise<PageData[]> {
  const response = await fetch(`${API_URL}/navigation`, {
    next: { revalidate: 300 }, // Cache 5 Minuten
  })
  if (!response.ok) return []
  return await response.json()
}
```

### 4.2 API Routes (Proxy)

**Verzeichnis**: `frontend/app/api/forms/`

Next.js API Routes dienen als Proxy zu ProcessWire:

```
frontend/app/api/
├── forms/
│   ├── contact/route.ts
│   ├── subscribe/route.ts
│   ├── visit/route.ts
│   └── waiting-list/route.ts
└── doi/
    └── confirm/route.ts
```

**Beispiel**: `contact/route.ts`

```typescript
import { NextRequest, NextResponse } from 'next/server'

const PROCESSWIRE_API = process.env.PROCESSWIRE_API_URL || 'http://localhost/api'

export async function POST(request: NextRequest) {
  try {
    const body = await request.json()

    // Frontend-Validierung
    if (!body.name || !body.email || !body.subject || !body.message) {
      return NextResponse.json(
        { success: false, error: 'Bitte füllen Sie alle Pflichtfelder aus.' },
        { status: 400 }
      )
    }

    // Weiterleitung an ProcessWire
    const response = await fetch(`${PROCESSWIRE_API}/forms/contact`, {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(body),
    })

    const data = await response.json()

    if (data.success) {
      return NextResponse.json({ success: true })
    } else {
      return NextResponse.json(
        { success: false, error: data.error || 'Es ist ein Fehler aufgetreten.' },
        { status: 400 }
      )
    }
  } catch (error) {
    return NextResponse.json(
      { success: false, error: 'Es ist ein Fehler aufgetreten.' },
      { status: 500 }
    )
  }
}
```

### 4.3 Umgebungsvariablen

| Variable | Beschreibung | Beispiel |
|----------|--------------|----------|
| `PROCESSWIRE_API_URL` | Backend API URL (serverseitig) | `https://www.bioco.ch/api` |
| `NEXT_PUBLIC_PROCESSWIRE_API_URL` | Backend API URL (clientseitig) | `https://www.bioco.ch/api` |

**Konfiguration** in `next.config.js`:
```javascript
env: {
  PROCESSWIRE_API_URL: process.env.PROCESSWIRE_API_URL || 'https://staging.bioco.ch/api',
}
```

---

## 5. Datenfluss-Diagramme

### 5.1 Formular-Einreichung

```
┌──────────────────────────────────────────────────────────────────┐
│ 1. BENUTZER FÜLLT FORMULAR AUS                                   │
└──────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌──────────────────────────────────────────────────────────────────┐
│ 2. NEXT.JS FRONTEND                                              │
│    ContactForm.tsx → fetch('/api/forms/contact', { body })       │
└──────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌──────────────────────────────────────────────────────────────────┐
│ 3. NEXT.JS API ROUTE                                             │
│    app/api/forms/contact/route.ts                                │
│    - Frontend-Validierung                                        │
│    - Weiterleitung an ProcessWire                                │
└──────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌──────────────────────────────────────────────────────────────────┐
│ 4. PROCESSWIRE API                                               │
│    site/api/forms.php                                            │
│    - Routing nach Formulartyp                                    │
│    - FormProcessor-Modul aufrufen                                │
└──────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌──────────────────────────────────────────────────────────────────┐
│ 5. FORMPROCESSOR MODULE                                          │
│    - Input Sanitization                                          │
│    - Validierung                                                 │
│    - DOIManager.initiateDOI() aufrufen                          │
└──────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌──────────────────────────────────────────────────────────────────┐
│ 6. DOIMANAGER MODULE                                             │
│    - Token generieren (64 Hex-Zeichen)                          │
│    - In Datenbank speichern (doi_tokens)                        │
│    - Bestätigungs-E-Mail senden                                 │
└──────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌──────────────────────────────────────────────────────────────────┐
│ 7. RESPONSE                                                      │
│    { "success": true }                                           │
└──────────────────────────────────────────────────────────────────┘
```

### 5.2 DOI-Bestätigung

```
┌──────────────────────────────────────────────────────────────────┐
│ 1. BENUTZER KLICKT LINK IN E-MAIL                                │
│    https://bioco.ch/doi-confirm?token=abc123...                  │
└──────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌──────────────────────────────────────────────────────────────────┐
│ 2. NEXT.JS SEITE                                                 │
│    app/doi-confirm/page.tsx                                      │
│    - Token aus URL lesen                                         │
│    - API aufrufen                                                │
└──────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌──────────────────────────────────────────────────────────────────┐
│ 3. NEXT.JS API ROUTE                                             │
│    app/api/doi/confirm/route.ts                                  │
│    - Weiterleitung an ProcessWire                                │
└──────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌──────────────────────────────────────────────────────────────────┐
│ 4. PROCESSWIRE API                                               │
│    site/api/doi.php                                              │
│    - Token validieren                                            │
│    - FormProcessor.finalizeSubmission() aufrufen                 │
└──────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌──────────────────────────────────────────────────────────────────┐
│ 5. DOIMANAGER.confirmDOI()                                       │
│    - Datenbank: Token suchen                                     │
│    - Prüfen: nicht abgelaufen, nicht bestätigt                  │
│    - Markieren: confirmed = 1, confirmed_at = now                │
│    - Formulardaten zurückgeben                                   │
└──────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌──────────────────────────────────────────────────────────────────┐
│ 6. FORMPROCESSOR.finalizeSubmission()                            │
│    Je nach Formulartyp:                                          │
│    - contact: Admin-E-Mail senden                                │
│    - subscribe: Bestätigungs-E-Mail an User                     │
│    - visit/waiting_list: Admin-E-Mail senden                    │
│    - Matomo-Tracking                                             │
└──────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌──────────────────────────────────────────────────────────────────┐
│ 7. RESPONSE                                                      │
│    { "success": true, "form_type": "contact" }                   │
└──────────────────────────────────────────────────────────────────┘
```

---

## 6. Sicherheit

### Input Sanitization

ProcessWire Sanitizer wird für alle Eingaben verwendet:

| Methode | Verwendung |
|---------|------------|
| `$sanitizer->text()` | Einzeilige Texte (name, subject) |
| `$sanitizer->email()` | E-Mail-Adressen |
| `$sanitizer->textarea()` | Mehrzeilige Texte (message, notes) |
| `$sanitizer->date()` | Datumswerte |

### E-Mail-Validierung

Doppelte Validierung:
1. `$sanitizer->email()` (ProcessWire)
2. `filter_var($email, FILTER_VALIDATE_EMAIL)` (PHP)

### Token-Sicherheit

- 64 Hex-Zeichen (256 Bit Entropie)
- Kryptografisch sichere Generierung (`random_bytes()`)
- Eindeutigkeitsprüfung in Datenbank
- Automatischer Ablauf nach 24 Stunden

### CORS

Aktuell: `Access-Control-Allow-Origin: *`

**Empfehlung für Produktion**: Einschränken auf erlaubte Domains:
```php
$allowed = ['https://bioco.ch', 'https://www.bioco.ch'];
$origin = $_SERVER['HTTP_ORIGIN'] ?? '';
if(in_array($origin, $allowed)) {
    header("Access-Control-Allow-Origin: $origin");
}
```

---

## 7. Wartung

### Abgelaufene Tokens aufräumen

Der DOIManager bietet `cleanupExpiredTokens()`. Empfehlung: täglich per Cron ausführen.

**ProcessWire LazyCron** (in `site/ready.php`):
```php
$wire->addHookAfter('LazyCron::everyDay', function($event) {
    $doiManager = $event->wire->modules->get('DOIManager');
    $deleted = $doiManager->cleanupExpiredTokens();
    $event->wire->log->save('doi', "Deleted $deleted expired tokens");
});
```

### Logging

Fehler werden via ProcessWire `$this->error()` geloggt. Logs finden sich in:
- `site/assets/logs/errors.txt`

---

*Zuletzt aktualisiert: Januar 2026*
