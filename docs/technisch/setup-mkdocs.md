# Vollständige Setup-Anleitung: Doku-Seite (MkDocs)

Dies ist der "Bauplan" für das `bioco-doku`-Projekt. Er dokumentiert den automatisierten "Git-First"-Workflow für die Doku-Seite `docs.bioco.ch`.

**Das Ziel:** Ein extrem einfacher Workflow, der keine Server-Uploads per FTP oder File Manager erfordert.
1.  Änderungen lokal auf dem PC im `bioco-doku`-Ordner vornehmen.
2.  Mit GitHub Desktop "committen" und "pushen".
3.  GitHub Actions (ein Roboter) baut die Seite automatisch neu und veröffentlicht sie live.

---

## Phase 1: Lokales Setup (Dein PC)

Alle diese Schritte passieren auf deinem lokalen Computer und müssen nur *einmal* durchgeführt werden.

=== "Schritt 1: Werkzeuge installieren (Terminal)"

    Wir benötigen `pip3` (den Python-Installer), um MkDocs und das Theme zu installieren.
    (Falls `pip3` nicht gefunden wird, muss `xcode-select --install` ausgeführt werden, um die Apple Command Line Tools zu installieren.)
    
    ```bash
    # 1. Installiert MkDocs
    pip3 install mkdocs

    # 2. Installiert das "Material" Design-Theme
    pip3 install mkdocs-material
    ```

=== "Schritt 2-3: Projekt klonen & initialisieren"

    2.  **GitHub-Repo klonen:**
        * Erstelle ein neues, **öffentliches** (Public) Repository auf GitHub.com. Name: `bioco-doku`.
        * Klone dieses leere Repo mit **GitHub Desktop** auf deinen PC (z.B. in `~/Projekte/bioco-doku`).
    3.  **MkDocs-Projekt initialisieren (Terminal):**
        * Navigiere im Terminal *in* deinen leeren `bioco-doku`-Ordner: `cd ~/Projekte/bioco-doku`
        * Führe `mkdocs new .` aus (mit dem Punkt), um die Dateien im aktuellen Ordner zu erstellen:
        ```bash
        mkdocs new .
        ```
    * Dein Ordner enthält jetzt `mkdocs.yml` und `docs/`.

=== "Schritt 4A: `mkdocs.yml` (Haupt-Konfig)"

    Ersetze den Inhalt der `mkdocs.yml` mit dieser Konfiguration (oder der oben angepassten Version):

    ```yaml
    site_name: Bioco Web-Projekt Doku
    site_url: [https://docs.bioco.ch/](https://docs.bioco.ch/)
    theme:
      name: material
      language: de
      logo: assets/logo.png
    # ... (Rest der Konfiguration siehe Datei 'mkdocs.yml' oben) ...
    nav:
      - Start: index.md
      # ... (Rest der Navigation siehe Datei 'mkdocs.yml' oben) ...
    ```

=== "Schritt 4B: `.gitignore` (Ignorierliste)"

    Erstelle eine `.gitignore`-Datei und füge hinzu:
    
    ```text
    site/
    ```

=== "Schritt 4C: `requirements.txt` (Werkzeugliste)"

    Erstelle eine `requirements.txt`-Datei und füge hinzu:
    
    ```text
    mkdocs
    mkdocs-material
    ```

=== "Schritt 4D: `.github/workflows/deploy.yml` (Roboter)"

    Erstelle den Ordner `.github`, darin den Ordner `workflows`. Erstelle *darin* die Datei `deploy.yml` und füge diesen Code ein:

    ```yaml
    name: Deploy MkDocs to GitHub Pages
    on:
      push:
        branches:
          - main  # oder 'master', je nach Haupt-Branch
    jobs:
      deploy:
        runs-on: ubuntu-latest
        steps:
          - uses: actions/checkout@v4
          - name: Set up Python
            uses: actions/setup-python@v5
            with:
              python-version: '3.10'
          - name: Install dependencies
            run: |
              pip install -r requirements.txt
          - name: Deploy
            run: mkdocs gh-deploy --force --clean
    ```

---

## Phase 2: GitHub-Konfiguration (Website)

Diese Schritte sind nur *einmal* auf GitHub.com nötig.

1.  **Code hochladen (GitHub Desktop):**
    * Öffne GitHub Desktop. Alle neuen Dateien sind sichtbar.
    * Schreibe eine Commit-Nachricht (z.B. "Initiales Doku-Setup mit Actions").
    * Klicke **"Commit to main"** und dann **"Push origin"**.
2.  **Schreibrechte für die Action geben:**
    * Die erste Action wird fehlschlagen! Wir müssen ihr Schreibrechte geben.
    * Gehe auf GitHub.com zum `bioco-doku`-Repo -> **"Settings"**.
    * Klicke links auf "Actions" -> **"General"**.
    * Scrolle runter zu "Workflow permissions".
    * Wähle **"Read and write permissions"** und klicke "Save".
3.  **Action erneut starten:**
    * Gehe zum **"Actions"**-Tab.
    * Klicke auf den fehlgeschlagenen (roten) Workflow.
    * Klicke oben rechts auf **"Re-run jobs"**.
    * Warte, bis der Job grün (erfolgreich) ist. Er erstellt jetzt einen neuen Branch `gh-pages`.
4.  **GitHub Pages aktivieren:**
    * Gehe zurück zu **"Settings"** -> **"Pages"**.
    * **Source (Quelle):** Wähle "Deploy from a branch".
    * **Branch:** Wähle den neuen Branch **`gh-pages`** aus und klicke "Save".
5.  **Domain verbinden:**
    * Trage im Feld "Custom domain" deine Domain ein: `docs.bioco.ch`
    * Klicke **"Save"**.

---

## Phase 3: Server-Konfiguration (cPanel DNS)

!!!info "Kein Terminal, kein File Manager"
    Für die Doku-Seite brauchen wir **keinen** SSH-Zugriff, kein Terminal und keinen File Manager auf dem Server. Die einzige Aktion findet im DNS-Editor statt.

1.  **DNS-Zone öffnen:**
    * Gehe im cPanel zu "Domains" -> **"Zone Editor"**.
    * Klicke bei `bioco.ch` auf "Manage" (Verwalten).
2.  **Alte Einträge löschen:**
    * **Lösche** alle Einträge, die `docs.bioco.ch` im Namen haben (Typ A, TXT, SRV etc.), die cPanel automatisch erstellt hat.
3.  **Neuen CNAME-Eintrag erstellen:**
    * Klicke "+ Add Record" und erstelle diesen **einen** Eintrag:
    * **Name:** `docs`
    * **Type:** `CNAME`
    * **Record (Ziel):** `"DEIN-GITHUB-KONTO".github.io`
    * Klicke "Save Record".

---

## Phase 4: Dein Workflow (Taegliche Arbeit)

Ab jetzt ist dein Workflow für die Doku extrem einfach:

1.  Öffne den `bioco-doku`-Ordner auf deinem PC.
2.  Bearbeite die `.md`-Dateien im `docs/`-Verzeichnis.
3.  Starte `mkdocs serve` im Terminal, um eine Live-Vorschau unter `http://127.0.0.1:8000` zu sehen (optional).
4.  Wenn du fertig bist: Öffne **GitHub Desktop**.
5.  Schreibe eine Commit-Nachricht (z.B. "Governance-Seite aktualisiert").
6.  Klicke **"Commit to main"** und dann **"Push origin"**.
7.  Warte 1-2 Minuten. Die GitHub Action baut die Seite neu. `https://docs.bioco.ch` ist automatisch auf dem neuesten Stand.