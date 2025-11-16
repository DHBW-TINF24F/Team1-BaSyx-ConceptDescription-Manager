# Projektablaufplan

## Versionskontrolle

| Version | Datum      | Autor | Anmerkung                                                                 |
|--------|------------|-------|----------------------------------------------------------------------------|
| 1.0    | 11.10.2025 | Anna  | Arbeitsversion (übertragen von unserem Arbeitsumfeld für Organisation (Teams)) |

## Rahmen & Rollen

**Zeitplan:** Software-engineering Semester 3 – Team 1: BaSyx ConceptDescription Manager  
**Zeitraum:** 26.09.2025 bis [Ende des vierten Semesters]

### Abkürzungen

| Kürzel | Rolle                                 | Person    |
|--------|---------------------------------------|-----------|
| PL     | Projektleiter                         | Anna      |
| PM     | Projektmanager                        | Niklas    |
| TM     | Testmanager                           | Priyanshu |
| SA     | Systemarchitekt                       | Chris     |
| TR     | Technischer Redakteur (Dokumentation) | Lütfi     |

## Zeitplan / Tasks

| Task                                                        | Beschreibung                                                                                                                                                                        | Zuständige Person | Start    | End      | Tage |
|-------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------|----------|----------|------|
| **MEILENSTEIN I**                                           |                                                                                                                                                                                     |             |          |          |  14  |
| Projektinitialisierung und Analyse                          |                                                                                                                                                                                     |             | 9/26/25  | 10/11/25 |      |
| Repo & Projekt-Setup                                        | Repo nach Schema TINF24F/Team1-Basyx-CD-Manager anlegen; Issue-Tracker & Projects-Board (je Dokument ein Review-Issue; Meeting Minutes als fortlaufendes Issue); README (Build/Run) | PL          |          |          |      |
| Einarbeitung                                                | AASX-Explorer installieren & BaSyx lokal lauffähig; Tutorials durcharbeiten & Lücken notieren; AAS/ConceptDescriptions (IEC 61360) verstehen; AAS-Brain sichten                     | SA / Alle   |          |          |      |
| Anforderungen sichten/prüfen                                |                                                                                                                                                                                     | PM, PL      |          |          |      |
| CRS (Lastenheft) erstellen                                  | funktionale & nicht-funktionale Anforderungen, Use-Cases/Skizzen; IDs für Traceability vergeben.                                                                                    | PM, PL      |          |          |      |
| Risiko-Register                                             | Die größten / wichtigsten Risiken und Gegenmaßnahmen klären                                                                                                                         | PL          |          |          |      |
| SRS v1 (Pflichtenheft) erstellen                            |                                                                                                                                                                                     | PM, SA      |          |          |      |
| **MEILENSTEIN II**                                          |                                                                                                                                                                                     |             | 10/11/25 |          |  30  |
| Forks & Buildchain                                          | BaSyx-Repos forken; lokale Buildchain (Java/Spring/Node) lauffähig machen.                                                                                                          | SA          |          |          |      |
| BaSyx-UI Analyse & Usability-Konzept                        | Bestehendes BaSyx-GUI analysieren; Workflows für CD-Manager definieren (Listenansicht, Edit-Dialog).                                                                                | PM          |          |          |      |
| Repo-Struktur & Wiki anlegen                                | Root „TINF24F/TeamNr/ProjektName“; Wiki-Seiten für SRS/SAS; Issues pro Dokument + Team-Protokoll.                                                                                   | PL          |          |          |      |
| SRS (Wiki) finalisieren                                     | Spezifikation verfeinern (Black-Box), Schnittstellen definieren.                                                                                                                    | SA          |          |          |      |
| SAS (Architektur, EN) erstellen                             | White-Box-Sicht (Komponenten, APIs, Datenmodell IEC 61360).                                                                                                                         | SA          |          |          |      |
| Module ableiten & zuweisen                                  | CD-Repository, List-View, Edit-Dialog, IEC-Importer, Docs; Owner zuweisen.                                                                                                          | PL          |          |          |      |
| Prototyp v0.1 (UI)                                          | Listenansicht (sortierbar) + Edit-Dialog (Dummy-Daten).                                                                                                                             | Alle        |          |          |      |
| IEC-Importer PoC                                            | Web-Scrape IEC-CDD-Seite, Tabelle parsen, Mapping → Data Specification IEC 61360 (JSON).                                                                                            | Alle        |          |          |      |
| Projektplan & Reviews aktualisieren                         | GitHub Projects, Review-Issues je Dokument, Protokoll führen.                                                                                                                       | PL          |          |          |      |
| PPT vorbereiten (Sem.3)                                     |                                                                                                                                                                                     | TR          |          | 11/14/25 |      |
| Weihnachtspause                                             |                                                                                                                                                                                     |             |          |          |      |
| **MEILENSTEIN III**                                         |                                                                                                                                                                                     |             | 1/1/26   |          |      |
| CRUD & Repo-Integration                                     | CRUD für ConceptDescriptions + Anbindung an BaSyx-CD-Repository.                                                                                                                    | Alle        |          |          |      |
| Listenansicht: Sortierung/Filter/Pagination                 | Tabellen-UI produktionsreif (ähnlich Referenz-Screenshot).                                                                                                                          | Alle        |          |          |      |
| Edit-Dialog + Validierung                                   | Mapping auf Data Specification IEC 61360; Validierung & Fehlermeldungen.                                                                                                            | Alle        |          |          |      |
| IEC-Importer (vollständig)                                  | URL-Eingabe → Crawl/Parse IEC-CDD → Mapping & Persistenz; Duplikaterkennung.                                                                                                        | Alle        |          |          |      |
| Unit-/Integrationstests                                     | Modul- und End-to-End-Tests für UI/Backend/Importer.                                                                                                                                | TM          |          |          |      |
| STP erstellen (Repo)                                        | Systemtestplan im Repo anlegen.                                                                                                                                                     | TM          |          |          |      |
| Systemtest durchführen + STR                                | Systemtest ausführen; Ergebnisse als STR dokumentieren.                                                                                                                             | TM          |          |          |      |
| **MEILENSTEIN IV**                                          |                                                                                                                                                                                     |             |          |          |      |
| Überprüfung aller Dateien etc. auf Vollständigkeit          |                                                                                                                                                                                     | Alle        |          |          |      |
| Doublecheck aller abzugebenden Sachen auf Konvention und Anforderungen |                                                                                                                                                                          | Alle        |          |          |      |
| PPT vorbereiten (Sem.4)                                     |                                                                                                                                                                                     | TR          |          |          |      |
| Abgabe                                                      |                                                                                                                                                                                     | Alle        |          |          |      |






