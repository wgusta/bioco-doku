Bauplan: Bioco Website (ProcessWire & Git-Workflow)

Dies ist die Schritt-für-Schritt-Anleitung, wie wir die bioco.ch-Website auf dem Novatrend (cPanel)-Server aufgesetzt haben.

Unser Ziel: Wir bauen die Website zweimal:

Staging (Testküche): Unter staging.bioco.ch. Hier probieren wir alles aus, ohne dass es öffentliche Besucher sehen.

Live (Restaurant): Unter www.bioco.ch. Das ist die fertige, öffentliche Seite.

Unsere Werkzeuge:

GitHub (Unser "Rezeptbuch"): Ein zentraler Ort im Internet, an dem wir den Code (unsere "Rezepte") sicher speichern. Wir benutzen GitHub Desktop, um Änderungen einfach zu "pushen" (speichern).

cPanel (Unsere "Küche"): Die Verwaltungsoberfläche unseres Servers, auf dem die Website läuft.

ProcessWire (Unser "Kochsystem"): Das Content Management System (CMS), das wir benutzen.

Phase 1: Vorbereitung (Rezeptbuch füllen)

Alles beginnt auf unserem lokalen PC und auf GitHub.

GitHub-Repo (bioco-web-project) erstellen: Wir legen ein privates Repository (unser "Rezeptbuch") auf GitHub.com an.

Zwei Kapitel anlegen (Branches): In unserem Rezeptbuch brauchen wir zwei Kapitel:

main: Für die fertigen Rezepte (Live-Seite).

develop: Für Test-Rezepte (Staging-Seite).

ProcessWire lokal holen: Wir laden die ProcessWire-ZIP-Datei auf unseren PC herunter und entpacken sie.

Dateien vorbereiten: Wir kopieren alle ProcessWire-Dateien (index.php, wire/, site-blank/ etc.) in den Projektordner auf unserem PC.

Git-Regeln (.gitignore): Wir erstellen eine .gitignore-Datei, die Git sagt: "Bitte ignoriere sensible Dateien wie site/config.php (Passwörter) und site/assets/ (Uploads)."

Hochladen (Push): Wir benutzen GitHub Desktop, um dieses "Grundrezept" (alle ProcessWire-Dateien) in beide Kapitel (main und develop) unseres GitHub-Rezeptbuchs hochzuladen ("pushen").

Phase 2: Server-Vorbereitung (Küche einrichten)

Jetzt loggen wir uns im cPanel ein und bereiten die Küche vor.

Speisekammern (Datenbanken) anlegen:

Wir gehen zu "MySQL-Datenbanken".

Wir legen drei leere Datenbanken (Speisekammern) an:

bioco_live (für das Restaurant)

bioco_staging (für die Testküche)

bioco_matomo (für die Besucher-Statistik)

Wir legen einen "Koch" (Benutzer) an: bioco_wgusta.

Wir geben bioco_wgusta "ALLE RECHTE" (den Generalschlüssel) für diese drei Datenbanken.
[Bild der cPanel-Datenbankübersicht]

Adresse (Subdomain) anlegen:

Wir gehen zu "Subdomains".

Wir erstellen staging.bioco.ch und weisen ihm einen Ordner zu (z.B. public_html/bioco_staging).

Phase 3: Der Server-Terminal-Hack (Der Spezial-Schlüssel)

Hier kam unser grosses Problem. Um unser Rezeptbuch (GitHub) automatisch mit der Küche (cPanel) zu verbinden, braucht der Server einen GitHub-Schlüssel.

!!!warning "Das Problem: cPanel zwingt uns zu einem Passwort"
Die cPanel-Grafikoberfläche ("SSH Access") zwingt uns, ein Passwort für jeden neuen Schlüssel zu erstellen (wie du im Screenshot gezeigt hast).

Aber das automatische Git-Tool von cPanel (`Git Version Control`) *kann* kein Passwort eingeben. Es funktioniert **nur** mit einem schlüssellosen Zugang. Die cPanel-eigenen Werkzeuge sind also kaputt und widersprechen sich.


Die Lösung: Der Terminal-Workaround

Wir mussten die grafische Oberfläche umgehen und den Schlüssel direkt im cPanel Terminal (die "Kommandozeile") erstellen.

Das war der wichtigste Schritt des ganzen Setups.

# 1. Wir loggen uns ins cPanel "Terminal" ein.

# 2. Wir löschen alle kaputten Schlüssel-Reste,
#    die von der Grafikoberfläche erstellt wurden.
rm /home/bioco/.ssh/id_rsa*
rm /home/bioco/.ssh/authorized_keys

# 3. DER HACK: Wir erstellen einen neuen Schlüssel (ssh-keygen)
#    und zwingen ihn (-N ""), KEIN Passwort zu haben.
#    (Ersetze 'bioco' mit dem cPanel-Benutzernamen)
ssh-keygen -t rsa -b 2048 -f /home/bioco/.ssh/id_rsa -N ""

# 4. Erfolg! Der Schlüssel (id_rsa) wurde ohne Passwort erstellt.


Danach mussten wir diesen neuen Schlüssel "aktivieren":

cPanel: Zurück zur "SSH Access"-Oberfläche. Der neue Schlüssel war da. Wir mussten auf "Manage" -> "Authorize" klicken.

GitHub: Wir haben den Public Key (cat /home/bioco/.ssh/id_rsa.pub im Terminal) kopiert und ihn in GitHub bei unserem bioco-web-project als "Deploy Key" (mit Schreibrechten!) eingetragen.

Jetzt hatten wir eine funktionierende, automatische Verbindung.

Phase 4: Staging-Seite installieren (Die Testküche)

Jetzt haben wir alles verbunden und konnten die Testküche aufbauen.

Code holen (Klonen):

In "Git Version Control" klonen wir unser GitHub-Repo (mit der SSH-URL git@github.com:...) in den Ordner public_html/bioco_staging.

Wir klicken "Manage" und wechseln den Branch auf develop.

Fehlerbehebung (Server einrichten):
Die frische Installation funktionierte nicht sofort. Wir mussten 3 Fehler beheben:

Fehler 1: 403 Forbidden (Türsteher-Problem).

Lösung: Im "File Manager" die Berechtigungen korrigiert:

Ordner public_html/bioco_staging/ auf 755 (Jeder darf reinsehen, nur wir dürfen ändern).

Datei public_html/bioco_staging/.htaccess auf 644 (Jeder darf lesen).

Fehler 2: Leere Seite (Falsche PHP-Version 7.4).

Lösung: Die .htaccess-Datei bearbeitet und PHP 8.2 erzwungen:

<IfModule mime_module>
  AddHandler application/x-httpd-ea-php82 .php .php8 .phtml
</IfModule>


Fehler 3: Installer-Fehler (config.php nicht schreibbar).

Lösung: Im "File Manager" die Berechtigung für den Ordner public_html/bioco_staging/site/ temporär auf 777 (Jeder darf alles) gesetzt.

Installation (Browser):

Wir rufen staging.bioco.ch auf. Der Installer (mit allen grünen Haken) startet.

Wir geben die Datenbank-Daten ein:

DB Name: bioco_staging

DB User: bioco_wgusta

DB Pass: (Unser geheimes Passwort)

Wir legen den Admin-User an und speichern das Passwort.

Aufräumen (WICHTIG):

Zurück im "File Manager": Wir setzen die Berechtigung des site/-Ordners von 777 (unsicher) zurück auf 755 (sicher).

Phase 5: Live-Seite installieren (Das Restaurant)

Wir haben Phase 4 komplett wiederholt, aber mit den "Live"-Daten:

Code holen: Den main-Branch in den public_html-Ordner geklont.

Fehlerbehebung: Die exakt gleichen 3 Fehler (403, PHP, 777) im public_html-Ordner behoben.

Installation: Den Installer auf www.bioco.ch aufgerufen und die bioco_live-Datenbank (mit dem bioco_wgusta-Benutzer) verbunden.

Aufräumen: Den public_html/site-Ordner zurück auf 755 gesetzt.

Ergebnis: Beide Seiten laufen, sind sicher und sauber mit GitHub verbunden.