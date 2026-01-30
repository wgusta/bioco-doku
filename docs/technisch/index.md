# Technische Dokumentation

Dieser Bereich richtet sich an Entwickler und technische Administratoren.

---

## Schnellzugriff

| Dokument | Beschreibung |
|----------|--------------|
| [ProcessWire Setup](setup-processwire.md) | Server-Installation, Git-Workflow, Deployment |
| [MkDocs Setup](setup-mkdocs.md) | Dokumentations-Website einrichten |
| [API Dokumentation](api.md) | REST API Endpunkte, Module, Datenfluss |

---

## Architektur-Uebersicht

Die bioco.ch Webseite besteht aus mehreren Komponenten:

| Komponente | Technologie | URL |
|------------|-------------|-----|
| Website (Frontend) | Next.js | bioco.ch |
| CMS (Backend) | ProcessWire | cms.bioco.ch |
| Dokumentation | MkDocs | docs.bioco.ch |
| Analytics | Matomo | bioco.ch/matomo |

---

## Repositories

| Repository | Beschreibung |
|------------|--------------|
| bioco-web-project | ProcessWire Backend und Next.js Frontend |
| bioco-doku | Diese Dokumentations-Website |

---

## Umgebungen

| Umgebung | Branch | URL |
|----------|--------|-----|
| Staging | develop | staging.bioco.ch |
| Live | main | www.bioco.ch |
