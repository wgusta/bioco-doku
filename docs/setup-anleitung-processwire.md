# Vollständige Setup-Anleitung: ProcessWire Website

Dies ist der "Bauplan" für das `bioco-web-project`. Er dokumentiert den gesamten Prozess vom leeren Server bis zu einer funktionierenden Staging- und Live-Umgebung, die mit Git verbunden ist.

**Das Ziel:** Ein professioneller "Git-First"-Workflow.
* **Lokal (PC):** Wir schreiben Code auf unserem Computer.
* **GitHub (Rezeptbuch):** Wir speichern den Code in einem zentralen Lager.
* **Server (Küche):** Der Server holt sich den Code von GitHub, um die Website anzuzeigen.

**Die Umgebungen:**
* `develop` (GitHub-Branch) -> `staging.bioco.ch` (Testküche)
* `main` (GitHub-Branch) -> `www.bioco.ch` (Live-Restaurant)

---

## Phase 1: Vorbereitung (Lokal & GitHub) 🧠

Zuerst bereiten wir den Code (das "Rezept") auf unserem PC und in unserem "Rezeptbuch" (GitHub) vor.

1.  **GitHub-Repo erstellen:**
    * Erstelle ein neues, **privates** Repository auf GitHub.com.
    * Name: `bioco-web-project` (mit "j").

2.  **Branches erstellen:**
    * Erstelle im Repo einen `develop`-Branch (basierend auf `main`).

3.  **ProcessWire-Dateien lokal holen:**
    * Erstelle einen leeren Ordner `bioco-web-project` auf deinem PC.
    * Klone das leere GitHub-Repo mit **GitHub Desktop** in diesen Ordner.
    * Lade die ProcessWire-ZIP-Datei von [processwire.com](https://processwire.com/) herunter.
    * Entpacke die ZIP. Kopiere **alle** Dateien (`index.php`, `wire/`, `site-blank/` etc.) in deinen lokalen `bioco-web-project`-Ordner.
    * Benenne den Ordner `site-blank` sofort in `site` um.

4.  **`.gitignore`-Datei erstellen:**
    * Erstelle im `bioco-web-project`-Ordner eine neue Datei namens `.gitignore`.
    * Füge diesen Inhalt ein, um sensible Dateien zu ignorieren:
        ```text
        # ProcessWire sensible/generierte Dateien
        /site/config.php
        /site/assets/
        /site/templates/cache/
        /site/templates/logs/
        /site/templates/sessions/
        /site/templates/backups/
        
        # OS-Dateien
        .DS_Store
        Thumbs.db
        ```

5.  **Code zu GitHub pushen (GitHub Desktop):**
    * Öffne GitHub Desktop. Du siehst alle neuen PW-Dateien.
    * Schreibe eine Commit-Nachricht (z.B. "Initial ProcessWire core files").
    * **Commite** die Änderungen.
    * **Pushe** die Änderungen (`Push origin`).
    * Wechsle in GitHub Desktop zum `develop`-Branch (`Current Branch` -> `develop`) und stelle sicher, dass er auch gepusht wird. (Du musst eventuell "Update from main" oder "Merge" klicken, damit beide Branches identisch sind).

---

## Phase 2: Server-Vorbereitung (cPanel) 🔧

Jetzt bereiten wir die "Küche" (Novatrend-Server) vor.

1.  **Datenbanken & Benutzer erstellen:**
    * Gehe zu "MySQL®-Datenbanken".
    * Erstelle 3 Datenbanken: `bioco_live`, `bioco_staging`, `bioco_matomo`.
    * Erstelle 1 Benutzer: `bioco_wgusta`.
    * Weise `bioco_wgusta` **"ALLE RECHTE"** (ALL PRIVILEGES) für alle 3 Datenbanken zu.
    * ![cPanel DB-Übersicht](Bildschirmfoto%202025-10-23%20um%2008.32.19.png)

2.  **Staging-Subdomain erstellen:**
    * Gehe zu "Subdomains".
    * Erstelle `staging.bioco.ch`.
    * Notiere den Pfad: `public_html/bioco_staging`.

---

## Phase 3: Der cPanel-SSH-Hack (Der Spezialschlüssel) 🔑

Das ist der **wichtigste und komplizierteste** Teil. Wir müssen eine sichere Verbindung zwischen Server und GitHub herstellen.

!!!warning "Das cPanel-Problem"
    Die cPanel-UI ("SSH Access") *zwingt* uns, ein Passwort für SSH-Schlüssel zu verwenden (siehe Screenshot). Das automatische Git-Tool ("Git Version Control") *benötigt* aber einen Schlüssel *OHNE* Passwort. Die Werkzeuge widersprechen sich.
    
    ![cPanel erzwingt Passwort](Bildschirmfoto%202025-10-22%20um%2021.36.55.jpg)

**Lösung: Wir erstellen den Schlüssel manuell im Server-Terminal.**

1.  **Terminal öffnen:** Gehe im cPanel zu "Erweitert" -> **"Terminal"**.
2.  **Aufräumen:** Lösche alle kaputten Schlüssel-Reste (falls vorhanden).
    ```bash
    # (Dieser Befehl löscht id_rsa, id_rsa.pub etc.)
    rm /home/bioco/.ssh/id_rsa*
    rm /home/bioco/.ssh/authorized_keys
    ```
3.  **Schlüssel OHNE Passwort erstellen:** Führe diesen Befehl aus, um einen neuen Schlüssel zu erstellen und das Passwort (`-N ""`) explizit leer zu lassen.
    ```bash
    ssh-keygen -t rsa -b 2048 -f /home/bioco/.ssh/id_rsa -N ""
    ```
4.  **Schlüssel im cPanel "autorisieren":**
    * Gehe zur UI "Sicherheit" -> "SSH Access".
    * Der neue Schlüssel `id_rsa` ist jetzt sichtbar. Klicke bei `id_rsa.pub` auf **"Manage"** -> **"Authorize"**.
5.  **Schlüssel zu GitHub hinzufügen:**
    * Gehe zurück ins **Terminal**.
    * Zeige den öffentlichen Schlüssel an: `cat /home/bioco/.ssh/id_rsa.pub`
    * Kopiere die gesamte Ausgabe (beginnt mit `ssh-rsa AAAA...`).
    * Gehe zu **GitHub.com** -> `bioco-web-project`-Repo -> "Settings" -> "Deploy Keys".
    * Klicke "Add deploy key".
    * **Title:** `Novatrend Server`
    * **Key:** Füge den kopierten Schlüssel ein.
    * **Haken setzen:** **"Allow write access"**.
    * Klicke "Add key".
6.  **Verbindung testen (Terminal):**
    * Teste die Verbindung mit:
        `git ls-remote git@github.com:wgusta/bioco-web-project.git`
    * Antworte auf die `(yes/no)`-Frage mit `yes`.
    * **Erfolg:** Du siehst eine Liste deiner Branches. Die Verbindung steht.

---

## Phase 4: Staging-Seite installieren (`staging.bioco.ch`) 🚀

Jetzt bauen wir die Testküche.

1.  **Code holen (Klonen):**
    * Gehe im cPanel zu "Dateien" -> **"Git™ Version Control"**.
    * Klicke **"Create"**.
    * **Clone URL:** `git@github.com:wgusta/bioco-web-project.git` (mit "j")
    * **Repository Path:** `public_html/bioco_staging`
    * **Repository Name:** `Bioco Staging`
    * Klicke "Create".
2.  **Branch wechseln:**
    * Gehe auf **"Manage"** für das "Bioco Staging"-Repo.
    * Ändere den "Checked-Out Branch" auf **`develop`** und klicke "Update".
3.  **Server-Fehlerbehebung (Standard-Setup):**
    Wir müssen jetzt 4 Dinge im "File Manager" reparieren:
    * **403-Fehler (Ordner):** Rechtsklick auf `public_html/bioco_staging` -> "Change Permissions" -> auf **`755`** setzen.
    * **403-Fehler (Datei):** Rechtsklick auf `public_html/bioco_staging/.htaccess` -> "Change Permissions" -> auf **`644`** setzen.
    * **PHP 7.4-Fehler:** Rechtsklick auf `public_html/bioco_staging/.htaccess` -> **"Edit"**. Füge den PHP 8.2 Code-Block **ganz oben** ein:
        ```apacheconfig
        # BEGIN PHP 8.2 ERZWINGEN
        <IfModule mime_module>
          AddHandler application/x-httpd-ea-php82 .php .php8 .phtml
        </IfModule>
        # END PHP 8.2 ERZWINGEN
        ```
    * **Installer-Fehler:** Rechtsklick auf Ordner `public_html/bioco_staging/site` -> "Change Permissions" -> **Temporär auf `777`** setzen.
4.  **Installation (Browser):**
    * Öffne `http://staging.bioco.ch` im Browser.
    * Der Installer startet (alle Haken grün, siehe Screenshot).
    * ![Installer Check](Bildschirmfoto%202025-10-23%20um%2008.25.54.jpg)
    * Gib die **Staging-Datenbankdaten** ein:
        * DB Name: `bioco_staging`
        * DB User: `bioco_wgusta`
        * DB Pass: (Dein Datenbank-Passwort)
    * Folge dem Installer, erstelle deinen Admin-User und **SPEICHERE DAS PASSWORT**.
5.  **Aufräumen (Sicherheit):**
    * Gehe zurück in den "File Manager".
    * Setze die Berechtigung des `.../bioco_staging/site`-Ordners von `777` zurück auf **`755`**.

---

## Phase 5: Live-Seite installieren (`www.bioco.ch`) 🍽️

Wiederhole Phase 4 exakt, aber für die Live-Seite.

1.  **Code holen (Klonen):**
    * **Vorbereitung:** Verschiebe den `bioco_staging`-Ordner (und `matomo`) *temporär* aus `public_html` heraus (z.B. in einen `TEMP`-Ordner), damit `public_html` leer ist.
    * Klone das Repo (wie in 4.1) in den Pfad: `public_html`
    * Name: `Bioco Live`
    * Branch: Bleibt auf `main` (Standard).
    * **Aufräumen:** Verschiebe `bioco_staging` (und `matomo`) zurück in `public_html`.
2.  **Server-Fehlerbehebung:**
    * Führe Schritt 4.3 (Permissions & PHP) exakt so für `public_html` und `public_html/.htaccess` und `public_html/site` durch.
3.  **Installation (Browser):**
    * Öffne `http://www.bioco.ch` im Browser.
    * Gib die **Live-Datenbankdaten** ein:
        * DB Name: `bioco_live`
        * DB User: `bioco_wgusta`
        * DB Pass: (Dein Datenbank-Passwort)
    * Schliesse die Installation ab.
4.  **Aufräumen (Sicherheit):**
    * Setze die Berechtigung des `public_html/site`-Ordners zurück auf **`755`**.

---

## Phase 6: Abschluss (Doku & Matomo) 📊

1.  **Matomo:**
    * Installiere Matomo im Ordner `public_html/matomo`.
    * Verbinde es beim Installer mit der `bioco_matomo`-Datenbank.
2.  **Doku:**
    * Die Doku (`docs.bioco.ch`) ist ein **separates Projekt** (`bioco-doku`).
    * Es wird nicht per cPanel, sondern automatisch per **GitHub Actions** auf **GitHub Pages** veröffentlicht.
    * Im cPanel ("Zone Editor") wird nur ein `CNAME`-Eintrag (`docs` -> `wgusta.github.io`) benötigt.