# MkDocs in diesem Repository (archiviert)

Die **kanonische Handbuch-Pflege** laeuft in **ProcessWire** unter `/internal-docs/` im Repo **bioco-web-project**. Siehe `ARCHIVED.md` im Root dieses Repos und das Kapitel „Internal handbook“ in `HANDOFF.md` in bioco-web-project.

Dieses Repo behaelt **MkDocs** und den Ordner `docs/` nur als **historische Referenz** und fuer lokale Vorschau.

## Lokale Vorschau (optional)

```bash
pip3 install -r requirements.txt
mkdocs serve
```

Oeffne `http://127.0.0.1:8000`.

## Deploy und CI

Automatische Deploy-Workflows nach **Novatrend** oder bei **Push auf main** sind **deaktiviert**. Die Workflow-Dateien unter `.github/workflows/` sind nur noch per **`workflow_dispatch`** startbar (Notfall oder bewusste Ausnahme).

GitHub Pages Fallback: `.github/workflows/deploy.yml` ebenfalls nur manuell.

DNS und Hosting fuer **docs.bioco.ch** am Provider abbauen, sobald die CMS-Migration abgeschlossen ist.
