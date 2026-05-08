# BaSyx ConceptDescription-Manager (CD-Manager)

![CD-Manager Wireframe](PROJECT/SRS/images/wireframe.png)

> Ein Web-UI-Plugin für das [Eclipse BaSyx](https://eclipse.dev/basyx/) Ökosystem,
> das **Concept Descriptions (CDs) sicht-, durchsuch- und editierbar** macht — direkt
> über die bestehende [BaSyx AAS Web UI](https://github.com/eclipse-basyx/basyx-aas-web-ui).

---

## Inhaltsverzeichnis

<!-- TOC -->
* [Was ist der CD-Manager?](#was-ist-der-cd-manager)
* [Warum dieses Projekt?](#warum-dieses-projekt)
* [Funktions-Überblick](#funktions-überblick)
* [Architektur](#architektur)
  * [Systemkontext (Black-Box)](#systemkontext-black-box)
  * [Modul-Struktur](#modul-struktur)
* [Tech-Stack](#tech-stack)
* [Ordnerstruktur](#ordnerstruktur)
* [Quick-Start für Entwickler](#quick-start-für-entwickler)
* [Dokumentation](#dokumentation)
* [UI-Bausteine im Detail](#ui-bausteine-im-detail)
* [Out of Scope](#out-of-scope)
<!-- TOC -->

---

## Was ist der CD-Manager?

Der CD-Manager ist eine **Erweiterung der BaSyx AAS Web UI** um eine vollständige
Verwaltungsoberfläche für **Concept Descriptions** des
[BaSyx ConceptDescription-Repositories](https://github.com/eclipse-basyx/basyx-java-server-sdk/tree/main/basyx.conceptdescriptionrepository).

**Concept Description (CD)** = standardisierte Beschreibung eines real existierenden
Konzepts (z. B. „Betriebsspannung", „Drehmoment") nach
[DataSpecificationIEC61360](https://reference.opcfoundation.org/v104/AMI/v100/docs/8.2/).
CDs bilden das **Common Data Dictionary (CDD)** der
[Asset Administration Shell (AAS)](https://industrialdigitaltwin.org/content-hub/aasspecifications)
und werden von Modulen (Submodel-Elementen) referenziert, um Bedeutungen
maschinenlesbar zu transportieren.

## Warum dieses Projekt?

BaSyx liefert ein **vollständiges Backend** für CDs (CRUD-API, Repository, DB) —
aber **keine UI**. Wer CDs anlegen, ändern oder importieren möchte, muss heute
direkt mit der REST-API arbeiten.

Dieses Plugin schließt die Lücke: Es macht CDs für Anwender ohne API-Kenntnisse
zugänglich und integriert sie in den bestehenden AAS-Workflow.

## Funktions-Überblick

| Bereich              | Funktion                                                                  | Status     |
|----------------------|---------------------------------------------------------------------------|------------|
| **Datentabelle**     | CDs paginiert anzeigen, sortieren, filtern, durchsuchen                   | Required   |
| **Attribut-Auswahl** | Sichtbare Spalten der Tabelle dynamisch wählen (max. 4 gleichzeitig)      | Required   |
| **Detailansicht**    | CD vollständig anzeigen und im Bearbeitungsmodus inline editieren         | Required   |
| **CRUD**             | Create / Read / Update / Delete mit Pflichtfeld- und Duplikat-Validierung | Required   |
| **Manuelles Anlegen**| Formular zum Erstellen einer neuen CD                                     | Required   |
| **Datei-Import**     | CDs aus JSON/AASX-Dateien importieren (Drag-and-Drop)                     | Optional   |
| **Repo-Cloning**     | Komplettes externes CD-Repository klonen mit Konflikt-Dialog              | Optional   |
| **IEC-CDD-Import**   | CDs per URL aus IEC Common Data Dictionary scrapen und mappen             | Optional   |

Die vollständige Anforderungsliste steht in
[SRS](PROJECT/SRS/TINF24F_1-SRS-1v1.md) und [CRS (Lastenheft)](PROJECT/TINF24F_1-CRS-5v0.md).

## Architektur

### Systemkontext (Black-Box)

Der CD-Manager wird als Plugin in die bestehende Web UI eingebettet und
spricht ausschließlich gegen das BaSyx CD-Repository — Datenbank und Backend
bleiben unverändert.

![System Black-Box](PROJECT/SAS/images/black-box.png)

### Modul-Struktur

Die Plugin-Architektur ist in eigenständige, lose gekoppelte Komponenten
aufgeteilt (Attribute Selector, Data Table, Detail View, CD/CDD Importer,
CD Exporter, CD Store):

![Komponenten-Struktur](PROJECT/SAS/images/component-structure-uml.png)

Details zu jeder Komponente, Schnittstellen und Verantwortlichkeiten siehe
[SAS – System Architecture Specification](PROJECT/SAS/TINF24f_1-SAS-2v0.md).

## Tech-Stack

| Schicht           | Technologie              | Hinweis                                                                |
|-------------------|--------------------------|------------------------------------------------------------------------|
| Framework         | **Vue.js 3**             | bestehende [BaSyx AAS Web UI](https://github.com/eclipse-basyx/basyx-aas-web-ui) |
| Build / Dev-Server| **Vite**                 |                                                                        |
| Sprache           | **TypeScript**           |                                                                        |
| Backend (extern)  | **BaSyx CD-Repository**  | Java-Server, läuft im Docker-Container                                 |
| Container         | **Docker / Compose**     | Setup unter `development/setupFiles`                                   |
| Linux-Toolchain   | **WSL2 + Node via NVM**  | für Windows-Hosts                                                      |

## Ordnerstruktur

```
Team1-BaSyx-ConceptDescription-Manager/
├── README.md                  ← du bist hier
├── images/                    ← Bilder dieser README
├── PROJECT/                   ← alle Projekt-Artefakte (Pflichtdokumente)
│   ├── TINF24F_1-CRS-5v0.md   ← Lastenheft (Customer Requirements)
│   ├── TINF24F_1-BC-2v0.md    ← Business Case
│   ├── SRS/                   ← Software Requirements + Wireframes
│   ├── SAS/                   ← System Architecture (UMLs, Module)
│   ├── STP/                   ← Software Test Plan / Reports
│   ├── MOD/                   ← Modul-Dokumentationen
│   ├── MeetingMinutes.md      ← Sitzungsprotokolle (Index)
│   └── Projektablaufplan.md   ← Zeitplan
├── development/               ← Entwickler-Setup & Hilfen
│   ├── developer_README.md    ← Schritt-für-Schritt Setup
│   └── setupFiles/            ← docker-compose.yml etc.
└── linkedDocuments/           ← extern verlinkte Dateien (z. B. Meetings)
```

## Quick-Start für Entwickler

Vollständige Anleitung: **[development/developer_README.md](development/developer_README.md)**.

Kurzfassung (Linux / WSL):

```bash
# 1. CD-Repository (Backend) starten
cd development/setupFiles
docker compose up -d

# 2. Frontend-Repo klonen und starten
git clone https://github.com/DHBW-TINF24F/Team1-basyx-aas-web-ui.git
cd Team1-basyx-aas-web-ui
npm install
npm run dev
```

Voraussetzungen:

- WSL2 mit Ubuntu (auf Windows-Hosts)
- Docker Engine + Compose
- Node.js (über NVM, aktuelle LTS)

## Dokumentation

| Dokument                   | Pfad                                                              | Inhalt                                          |
|----------------------------|-------------------------------------------------------------------|-------------------------------------------------|
| **Lastenheft (CRS)**       | [`PROJECT/TINF24F_1-CRS-5v0.md`](PROJECT/TINF24F_1-CRS-5v0.md)    | Was und warum, Use Cases, MoSCoW-Anforderungen  |
| **Pflichtenheft (SRS)**    | [`PROJECT/SRS/`](PROJECT/SRS/TINF24F_1-SRS-1v1.md)                | Funktionale Anforderungen, Wireframes           |
| **Architektur (SAS)**      | [`PROJECT/SAS/`](PROJECT/SAS/TINF24f_1-SAS-2v0.md)                | Black-Box, Module, Kommunikationsarchitektur    |
| **Test-Plan (STP)**        | [`PROJECT/STP/`](PROJECT/STP/)                                    | Teststrategie, Testfälle, Reports               |
| **Modul-Doku (MOD)**       | [`PROJECT/MOD/`](PROJECT/MOD/)                                    | Detail-Doku einzelner Module (z. B. CDD-Import) |
| **Business Case**          | [`PROJECT/TINF24F_1-BC-2v0.md`](PROJECT/TINF24F_1-BC-2v0.md)      | Wirtschaftliche Begründung                      |
| **Meeting Minutes**        | [`PROJECT/MeetingMinutes.md`](PROJECT/MeetingMinutes.md)          | Index aller Sitzungsprotokolle                  |
| **Projektablaufplan**      | [`PROJECT/Projektablaufplan.md`](PROJECT/Projektablaufplan.md)    | Zeit- und Meilensteinplan                       |

## UI-Bausteine im Detail

Die Wireframes der einzelnen UI-Bereiche dienen als verbindliche Vorlage für die
Implementierung. Die vollständigen Anforderungen je Baustein stehen im
[SRS](PROJECT/SRS/TINF24F_1-SRS-1v1.md).

| Baustein                  | Vorschau                                                  |
|---------------------------|-----------------------------------------------------------|
| Header-Navigation         | ![Header](PROJECT/SRS/images/header_navigation.png)       |
| Attribut-Selector (links) | ![Attribute Selector](PROJECT/SRS/images/attribute_selector.png) |
| Datentabelle              | ![Tabelle](PROJECT/SRS/images/table.png)                  |
| Detail- / Edit-Ansicht    | ![Detail View](PROJECT/SRS/images/detail_view.png)        |
| Erstellen / Importieren   | ![Add or Import](PROJECT/SRS/images/add_or_import.png)    |
| Repository-Cloning        | ![Cloning](PROJECT/SRS/images/cloning.png)                |

## Out of Scope

Folgendes wird in diesem Projekt **nicht** verändert:

- Das BaSyx CD-Repository-Backend (Java)
- Die REST-API der CD-Repository-Schnittstelle
- Das Datenbank-Schema der CD-Persistenz

Es wird ausschließlich die Web UI erweitert — die übrige Infrastruktur bleibt
unangetastet.
