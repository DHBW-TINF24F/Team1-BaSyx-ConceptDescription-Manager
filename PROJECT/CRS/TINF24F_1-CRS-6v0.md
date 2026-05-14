# Customer Requirement Specification (CRS) - BaSyx ConceptDescription-Plugin (CD-Manager)

## Versionskontrolle

| **Version** | **Datum**  | **Autor** | **Anmerkung**                                                   |
|-------------|------------|-----------|-----------------------------------------------------------------|
| 1.0         | 11.10.2025 | Anna      | Erste Version                                                   |
| 2.0         | 25.10.2025 | Anna      | Überarbeitung des Inhalts und  <br>Korrektur von Schreibfehlern |
| 3.0         | 02.11.2025 | Anna      | Gezielte Anpassung an die Formatanforderungen                   |
| 4.0         | 14.11.2025 | Anna      | Denglisch -> deutsch & Abgleich mit SRS                         |
| 5.0         | 18.11.2025 | Anna      | Nicht funktionale Anforderungen überarbeitet                    | 
| 6.0         | 09.05.2026 | Anna      | Überarbeitung des ganzen Dokuments                              |

## Inhaltsverzeichnis

<!-- TOC -->
* [Customer Requirement Specification (CRS) - BaSyx ConceptDescription-Plugin (CD-Manager)](#customer-requirement-specification-crs---basyx-conceptdescription-plugin-cd-manager)
  * [Versionskontrolle](#versionskontrolle)
  * [Inhaltsverzeichnis](#inhaltsverzeichnis)
  * [Begriffe und Abkürzungen](#begriffe-und-abkürzungen)
  * [Wichtige Dokumente](#wichtige-dokumente)
  * [Zweck des Dokuments](#zweck-des-dokuments)
  * [Umfang und beschreibung des Softwareprodukts](#umfang-und-beschreibung-des-softwareprodukts)
  * [Nutzeranforderungen](#nutzeranforderungen)
    * [UC-1: CDs in Tabelle und Tabellenseiten auflisten und suchen/filtern](#uc-1-cds-in-tabelle-und-tabellenseiten-auflisten-und-suchenfiltern)
      * [Beschreibung](#beschreibung)
      * [Designvorstellung](#designvorstellung)
      * [Referenzen](#referenzen)
    * [UC-2: Tabellen-interaktionen auf einzelnen CDs](#uc-2-tabellen-interaktionen-auf-einzelnen-cds)
      * [Beschreibung](#beschreibung-1)
      * [Designvorstellung](#designvorstellung-1)
      * [Referenzen](#referenzen-1)
    * [UC-3: Detail-Ansicht für CDs](#uc-3-detail-ansicht-für-cds)
      * [Beschreibung](#beschreibung-2)
      * [Designvorstellung](#designvorstellung-2)
      * [Referenzen](#referenzen-2)
    * [UC-4: Editor-Ansicht für CDs](#uc-4-editor-ansicht-für-cds)
      * [Beschreibung](#beschreibung-3)
      * [Designvorstellung](#designvorstellung-3)
      * [Referenzen](#referenzen-3)
    * [UC-5: Import Funktion für CD über AASX](#uc-5-import-funktion-für-cd-über-aasx)
      * [Beschreibung](#beschreibung-4)
      * [Designvorstellung](#designvorstellung-4)
      * [Referenzen](#referenzen-4)
    * [UC-6: Import Funktion für IECs als CD](#uc-6-import-funktion-für-iecs-als-cd)
      * [Beschreibung](#beschreibung-5)
      * [Designvorstellung](#designvorstellung-5)
      * [Referenzen](#referenzen-5)
    * [UC-7: Export Funktion für CDs](#uc-7-export-funktion-für-cds)
      * [Beschreibung](#beschreibung-6)
      * [Designvorstellung](#designvorstellung-6)
      * [Referenzen](#referenzen-6)
    * [UC-8: Referenzierung von CDs in Submodellen](#uc-8-referenzierung-von-cds-in-submodellen)
      * [Beschreibung](#beschreibung-7)
      * [Designvorstellung](#designvorstellung-7)
      * [Referenzen](#referenzen-7)
    * [UC-9: Löschen einzelner CDs aus dem CD repository](#uc-9-löschen-einzelner-cds-aus-dem-cd-repository)
      * [Beschreibung](#beschreibung-8)
      * [Designvorstellung](#designvorstellung-8)
      * [Referenzen](#referenzen-8)
    * [UC-10: CDs über JSON Datei importieren](#uc-10-cds-über-json-datei-importieren)
      * [Beschreibung](#beschreibung-9)
      * [Designvorstellung](#designvorstellung-9)
      * [Referenzen](#referenzen-9)
  * [Funktionale Anforderungen (MoSCoW)](#funktionale-anforderungen-moscow)
    * [Muss (M)](#muss-m)
    * [Soll (S)](#soll-s)
    * [Kann (K)](#kann-k)
  * [Nicht funktionale Anforderungen (NFA)](#nicht-funktionale-anforderungen-nfa)
  * [Akzeptanzkriterien](#akzeptanzkriterien)
<!-- TOC -->

## Begriffe und Abkürzungen

| Begriff/Abkürzung         | Beschreibung                                      |
|---------------------------|---------------------------------------------------|
| BaSyx                     | Eclipse-Projekt für AAS-Referenzimplementierungen |
| AASX                      | Asset Administration Shell Dateiformat            |
| CD                        | Concept Description                               |
| CRUD                      | Create, Read, Update, Delete                      |
| DataSpecificationIEC61360 | Template/ Schemadefinition für CDs                |
| CDD                       | Common Data Dictionary                            |
| IEC                       | International Electrotechnical Commission         |
| IRDI                      | International Registration Data Identifier        |
| tbd.                      | to be determined                                  |

## Wichtige Dokumente

- [Business Case: BC](../CRS/TINF24F_1-CRS-6v0.md)
- [Software Requirement Specification: SRS](../SRS/TINF24F_1-SRS-2v0.md)
- [Software Architecture Specification: SAS](../SAS/TINF24F_1-SAS-3v0.md)
- [System Test Plan: STP](../STP/TINF24F_1-STP-0v3.md)

## Zweck des Dokuments

Dokument legt den **Zweck, Umfang und die Anforderungen** für ein BaSyx-GUI-Plugin „CD-Manager" fest - also ein
webbasiertes Tool zur zentralen Pflege von Concept Descriptions inklusive. IEC-CDD-Import. Es definiert, **warum** das
Plugin gebaut wird und **was** es genau leisten muss, sodass Entwicklung, Test und Abnahme eine klare gemeinsame
Referenz haben.

## Umfang und beschreibung des Softwareprodukts

Der abgesteckte Umfang umfasst ein webbasiertes BaSyx-GUI-Plugin („CD-Manager") zur Verwaltung von Concept Descriptions
nach DataSpecificationIEC61360 mit komfortabler Listenansicht (Suche, Sortierung, Filter) und einem
Detail-/Editor-Dialog inklusive Feld-Validierungen sowie vollständigem CRUD gegen das BaSyx-CD-Repository (REST). Hinzu
kommt ein IEC-CDD-Importer: Per URL wird die CDD-Seite geladen, relevante Inhalte werden geparst, auf 61360-Felder
gemappt und als CD neu angelegt bzw. anhand eines eindeutigen Identifiers (z. B. IRDI) dedupliziert/aktualisiert; Fehler
werden verständlich behandelt und Aktionen geloggt, ein Export ausgewählter CDs als BaSyx-kompatibles JSON ist
vorgesehen. Das Ganze läuft in der lokalen BaSyx-Dev-Umgebung und prüft vor dem Speichern Pflichtfelder, Datentypen,
zulässige Werte sowie Identifier- und Versionskonventionen.

Erweiterungen sind konfigurierbare Tabellenspalten (nutzerbezogen gespeichert), Internationalisierung der UI (de/en) und
rollenbasierter Zugriff; als optionale Features sind Massenimport mit Ergebnisübersicht, Tagging/Klassifikation und eine
Vergleichsansicht zwischen zwei CDs vorgesehen. Nicht-funktional definiert der Umfang Ziele zu Performance (z. B.
Einzelimport < 5 s; Batch 50 URLs < 5 min), Sicherheit (Auth/Rollen, Eingabevalidierung, SSRF-Schutz), Wartbarkeit (
modulare Architektur, Logging, Feature-Flags) und Portabilität (Docker, CI-Build via GitHub Actions) sowie
i18n/Locale-Support. Für die Abnahme müssen UI-Elemente und Funktionen sichtbar wirksam sein, der Importer korrekt
befüllen/aktualisieren (inkl. identifier, preferredName/definition de/en, dataType, unit, version/revision), und es sind
README/Benutzerhandbuch sowie ein Community-Beitrag (PR/MR) gefordert.

## Nutzeranforderungen

Vorbemerkung: Die Begriffe "kann", "soll" und "muss" sowie deren Varianten dienen ausschließlich der Beschreibung der
Anforderungen und lassen keinen Rückschluss auf die tatsächliche Notwendigkeit des jeweiligen Use Cases zu.

### UC-1: CDs in Tabelle und Tabellenseiten auflisten und suchen/filtern

#### Beschreibung

CDs sollen in einer neuen Tabellenansicht dargestellt werden.  
Die Tabelle soll über eine Seitenaufteilung (Pagination) verfügen.  
Zusätzlich sollen Nutzer die Möglichkeit haben, gezielt nach bestimmten CDs zu suchen und die Ergebnisse zu filtern.

#### Designvorstellung

![cd-table-view.png](images/cd-table-view.png)

#### Referenzen

**Direkt Verwandt:**
- [FR-1](../SRS/TINF24F_1-SRS-2v0.md#fr-1---cds-in-tabelle-anzeigen-sowie-suchen-und-filtern): CDs in Tabelle anzeigen sowie suchen und filtern****
- [MOD-1](../SAS/TINF24F_1-SAS-3v0.md#mod-1---cd-table): CD Table
- [NFR-1](../SRS/TINF24F_1-SRS-2v0.md#nfr-1---nutzerfreundlichkeit): Nutzerfreundlichkeit

  **Indirekt Verwandt**
- [NFR-2](../SRS/TINF24F_1-SRS-2v0.md#nfr-2---responsive-design): Responsive Design
- [MOD-2](../SAS/TINF24F_1-SAS-3v0.md#mod-2---cd-store): CD Store
- [MOD-3](../SAS/TINF24F_1-SAS-3v0.md#mod-3---interaction-menu): Interaction menu

---

### UC-2: Tabellen-interaktionen auf einzelnen CDs

#### Beschreibung

Findet ein Nutzer eine CD über die Tabellen-Ansicht,
soll durch ein drei Punkte Menü die Interaktion mit einer gewählten CD möglich sein.
Das Interaktionsmenü soll dem Nutzer erlauben mit dem CD auf folgende weise zu interagieren:

- in Detail-Ansicht öffnen
- in Editor-Ansicht öffnen
- im submodell de-/referenzieren (je nachdem ob eine Referenz schon existiert)
- als JSON exportieren
- CD löschen

#### Designvorstellung

![cd-interaction-menu.png](images/cd-interaction-menu.png)

#### Referenzen

**Direkt Verwandt:**
- [FR-2](../SRS/TINF24F_1-SRS-2v0.md#fr-2---interaktionen-auf-einzelnen-cds-über-ineraktions-menü): Interaktionen auf einzelnen CDs über Ineraktions-Menü
- [NFR-1](../SRS/TINF24F_1-SRS-2v0.md#nfr-1---nutzerfreundlichkeit): Nutzerfreundlichkeit
- [MOD-1](../SAS/TINF24F_1-SAS-3v0.md#mod-1---cd-table): CD Table
- [MOD-3](../SAS/TINF24F_1-SAS-3v0.md#mod-3---interaction-menu): Interaction menu

  **Indirekt Verwandt**
- [FR-3](../SRS/TINF24F_1-SRS-2v0.md#fr-3---detail-ansicht-für-cds): Detail-Ansicht für CDs
- [FR-4](../SRS/TINF24F_1-SRS-2v0.md#fr-4---editor-ansicht-für-cds): Editor-Ansicht für CDs
- [FR-7](../SRS/TINF24F_1-SRS-2v0.md#fr-7---export-funktion-für-cds): Export Funktion für CDs
- [FR-8](../SRS/TINF24F_1-SRS-2v0.md#fr-8---referenzierung-von-cds-in-sms): Referenzierung von CDs in SMs
- [FR-10](../SRS/TINF24F_1-SRS-2v0.md#fr-10---löschen-einzelner-cds-aus-dem-cd-repository): Löschen einzelner CDs aus dem CD repository
- [NFR-2](../SRS/TINF24F_1-SRS-2v0.md#nfr-2---responsive-design): Responsive Design
- [MOD-2](../SAS/TINF24F_1-SAS-3v0.md#mod-2---cd-store): CD Store
- [MOD-4](../SAS/TINF24F_1-SAS-3v0.md#mod-4---cd-detail-view): CD Detail View
- [MOD-5](../SAS/TINF24F_1-SAS-3v0.md#mod-5---cd-editor): CD Editor
- [MOD-6](../SAS/TINF24F_1-SAS-3v0.md#mod-6---reference-module): Reference Module
- [MOD-7](../SAS/TINF24F_1-SAS-3v0.md#mod-7---cd-json-exporter): CD JSON exporter
- [MOD-8](../SAS/TINF24F_1-SAS-3v0.md#mod-8---cd-delete-view): CD Delete View

---

### UC-3: Detail-Ansicht für CDs

#### Beschreibung

Ein Nutzer muss die Möglichkeit haben, eine CD detailliert einzusehen.
Dadurch soll die eindeutige Identifikation der richtigen CD erleichtert werden, insbesondere bei CDs mit ähnlicher
Bedeutung oder Bezeichnung.

#### Designvorstellung

![cd-detail-view.png](images/cd-detail-view.png)

#### Referenzen

**Direkt Verwandt:**
- [FR-3](../SRS/TINF24F_1-SRS-2v0.md#fr-3---detail-ansicht-für-cds): Detail-Ansicht für CDs
- [FR-9](../SRS/TINF24F_1-SRS-2v0.md#fr-9---differenz--und-detailansicht-für-aasx-cds-und-iec-import): Differenz- und Detailansicht für AASX CDs und IEC Import
- [NFR-1](../SRS/TINF24F_1-SRS-2v0.md#nfr-1---nutzerfreundlichkeit): Nutzerfreundlichkeit
- [MOD-4](../SAS/TINF24F_1-SAS-3v0.md#mod-4---cd-detail-view): CD Detail View

  **Indirekt Verwandt**
- [FR-1](../SRS/TINF24F_1-SRS-2v0.md#fr-1---cds-in-tabelle-anzeigen-sowie-suchen-und-filtern): CDs in Tabelle anzeigen sowie suchen und filtern
- [FR-2](../SRS/TINF24F_1-SRS-2v0.md#fr-2---interaktionen-auf-einzelnen-cds-über-ineraktions-menü) : Interaktionen auf einzelnen CDs über Ineraktions-Menü
- [FR-5](../SRS/TINF24F_1-SRS-2v0.md#fr-5---import-funktion-für-cds-über-aasx): Import Funktion für CDs über AASX
- [FR-6](../SRS/TINF24F_1-SRS-2v0.md#fr-6---import-funktion-für-iecs-als-cd): Import Funktion für IECs als CD
- [MOD-1](../SAS/TINF24F_1-SAS-3v0.md#mod-1---cd-table): CD Table
- [MOD-2](../SAS/TINF24F_1-SAS-3v0.md#mod-2---cd-store): CD Store
- [MOD-3](../SAS/TINF24F_1-SAS-3v0.md#mod-3---interaction-menu): Interaction menu
- [MOD-10](../SAS/TINF24F_1-SAS-3v0.md#mod-10---aasx-cd-importer): AASX CD Importer
- [MOD-11](../SAS/TINF24F_1-SAS-3v0.md#mod-11---iec-cdd-importer): IEC CDD Importer

---

### UC-4: Editor-Ansicht für CDs

#### Beschreibung

Ein Nutzer muss die Möglichkeit haben, eine CD über die Editor-Ansicht zu öffnen und zu bearbeiten.
Dabei soll sichergestellt werden, dass eine CD nicht in einen ungültigen Zustand versetzt oder gespeichert werden kann.

#### Designvorstellung

![cd-edit-dialog.png](images/cd-edit-dialog.png)

#### Referenzen

**Direkt Verwandt:**
- [FR-4](../SRS/TINF24F_1-SRS-2v0.md#fr-4---editor-ansicht-für-cds): Editor-Ansicht für CDs
- [NFR-1](../SRS/TINF24F_1-SRS-2v0.md#nfr-1---nutzerfreundlichkeit): Nutzerfreundlichkeit
- [MOD-5](../SAS/TINF24F_1-SAS-3v0.md#mod-5---cd-editor): CD Editor

  **Indirekt Verwandt**
- [FR-2](../SRS/TINF24F_1-SRS-2v0.md#fr-2---interaktionen-auf-einzelnen-cds-über-ineraktions-menü) : Interaktionen auf einzelnen CDs über Ineraktions-Menü
- [MOD-1](../SAS/TINF24F_1-SAS-3v0.md#mod-1---cd-table): CD Table
- [MOD-2](../SAS/TINF24F_1-SAS-3v0.md#mod-2---cd-store): CD Store
- [MOD-3](../SAS/TINF24F_1-SAS-3v0.md#mod-3---interaction-menu): Interaction menu

---

### UC-5: Import Funktion für CD über AASX

#### Beschreibung

Ein Nutzer soll in der Lage sein eine eigene AASX Datei hochzuladen und nur die CDs zu extrahieren.

#### Designvorstellung

![cd-aasx-import.png](images/cd-aasx-import.png)

#### Referenzen

  **Direkt Verwandt:**
- [FR-5](../SRS/TINF24F_1-SRS-2v0.md#fr-5---import-funktion-für-cds-über-aasx): Import Funktion für CDs über AASX
- [FR-9](../SRS/TINF24F_1-SRS-2v0.md#fr-9---differenz--und-detailansicht-für-aasx-cds-und-iec-import): Differenz- und Detailansicht für AASX CDs und IEC Import
- [NFR-1](../SRS/TINF24F_1-SRS-2v0.md#nfr-1---nutzerfreundlichkeit): Nutzerfreundlichkeit
- [MOD-10](../SAS/TINF24F_1-SAS-3v0.md#mod-10---aasx-cd-importer): AASX CD Importer

  **Indirekt Verwandt**
- [FR-3](../SRS/TINF24F_1-SRS-2v0.md#fr-3---detail-ansicht-für-cds): Detail-Ansicht für CDs
- [MOD-4](../SAS/TINF24F_1-SAS-3v0.md#mod-4---cd-detail-view): CD Detail View

---

### UC-6: Import Funktion für IECs als CD

#### Beschreibung

IEC-Datensätze aus dem Common Data Dictionary stellen eine zusätzliche Quelle semantischer Informationen dar.
Ein Nutzer soll die Möglichkeit haben, IEC-Datensätze hochzuladen und als CD zu importieren.

#### Designvorstellung

![cd-iec-import.png](images/cd-iec-import.png)

#### Referenzen

  **Direkt Verwandt:**
- [FR-6](../SRS/TINF24F_1-SRS-2v0.md#fr-6---import-funktion-für-iecs-als-cd): Import Funktion für IECs als CD
- [FR-9](../SRS/TINF24F_1-SRS-2v0.md#fr-9---differenz--und-detailansicht-für-aasx-cds-und-iec-import): Differenz- und Detailansicht für AASX CDs und IEC Import
- [NFR-1](../SRS/TINF24F_1-SRS-2v0.md#nfr-1---nutzerfreundlichkeit): Nutzerfreundlichkeit
- [MOD-11](../SAS/TINF24F_1-SAS-3v0.md#mod-11---iec-cdd-importer): IEC CDD Importer

  **Indirekt Verwandt**
- [FR-3](../SRS/TINF24F_1-SRS-2v0.md#fr-3---detail-ansicht-für-cds): Detail-Ansicht für CDs
- [MOD-4](../SAS/TINF24F_1-SAS-3v0.md#mod-4---cd-detail-view): CD Detail View

---

### UC-7: Export Funktion für CDs

#### Beschreibung

Ein Nutzer soll die Möglichkeit haben, CDs als JSON Datei zu exportieren.

#### Designvorstellung

**ACHTUNG:** Das Design ist für den Standard download Dialog auf Ubuntu, dieser wird auf anderen Betriebssystemen
entsprechend anders aussehen.
![cd-export-as-json.png](images/cd-export-as-json.png)

#### Referenzen

**Direkt Verwandt:**
- [FR-2](../SRS/TINF24F_1-SRS-2v0.md#fr-2---interaktionen-auf-einzelnen-cds-über-ineraktions-menü) : Interaktionen auf einzelnen CDs über Ineraktions-Menü
- [FR-7](../SRS/TINF24F_1-SRS-2v0.md#fr-7---export-funktion-für-cds): Export Funktion für CDs
- [NFR-1](../SRS/TINF24F_1-SRS-2v0.md#nfr-1---nutzerfreundlichkeit): Nutzerfreundlichkeit
- [MOD-3](../SAS/TINF24F_1-SAS-3v0.md#mod-3---interaction-menu): Interaction menu
- [MOD-7](../SAS/TINF24F_1-SAS-3v0.md#mod-7---cd-json-exporter): CD JSON exporter

  **Indirekt Verwandt**
- [FR-1](../SRS/TINF24F_1-SRS-2v0.md#fr-1---cds-in-tabelle-anzeigen-sowie-suchen-und-filtern): CDs in Tabelle anzeigen sowie suchen und filtern
- [FR-11](../SRS/TINF24F_1-SRS-2v0.md#fr-11---cds-über-json-datei-importieren): CDs über JSON Datei importieren
- [MOD-1](../SAS/TINF24F_1-SAS-3v0.md#mod-1---cd-table): CD Table
- [MOD-12](../SAS/TINF24F_1-SAS-3v0.md#mod-12---json-importer): JSON Importer

---

### UC-8: Referenzierung von CDs in Submodellen

#### Beschreibung

Ein Nutzer soll die Möglichkeit haben, CDs in einem Submodell zu referenzieren uns bestehende Referenzen zu entfernen.

#### Designvorstellung

![cd-reference-dialog.png](images/cd-reference-dialog.png)

#### Referenzen

**Direkt Verwandt:**
- [FR-2](../SRS/TINF24F_1-SRS-2v0.md#fr-2---interaktionen-auf-einzelnen-cds-über-ineraktions-menü) : Interaktionen auf einzelnen CDs über Ineraktions-Menü
- [MOD-3](../SAS/TINF24F_1-SAS-3v0.md#mod-3---interaction-menu): Interaction menu
- [FR-8](../SRS/TINF24F_1-SRS-2v0.md#fr-8---referenzierung-von-cds-in-sms): Referenzierung von CDs in SMs
- [NFR-1](../SRS/TINF24F_1-SRS-2v0.md#nfr-1---nutzerfreundlichkeit): Nutzerfreundlichkeit
- [MOD-2](../SAS/TINF24F_1-SAS-3v0.md#mod-2---cd-store): CD Store
- [MOD-6](../SAS/TINF24F_1-SAS-3v0.md#mod-6---reference-module): Reference Module
- [MOD-9](../SAS/TINF24F_1-SAS-3v0.md#mod-9---reference-checker): Reference Checker

  **Indirekt Verwandt**
- [FR-1](../SRS/TINF24F_1-SRS-2v0.md#fr-1---cds-in-tabelle-anzeigen-sowie-suchen-und-filtern): CDs in Tabelle anzeigen sowie suchen und filtern
- [MOD-1](../SAS/TINF24F_1-SAS-3v0.md#mod-1---cd-table): CD Table

---

### UC-9: Löschen einzelner CDs aus dem CD repository

#### Beschreibung

Ein Nutzer soll die Möglichkeit haben, CDs aus dem CD repository zu löschen.
Sollte die CD noch in Submodellen referenziert wird, soll eine Sicherung eingebaut werden, sodass die CD nicht gelöscht werden kann, solange die Referenzen bestehen.
Zudem soll der Nutzer eine Warnung erhalten, damit klar ersichtlich ist, warum die CD nicht gelöscht werden kann.

#### Designvorstellung

![cd-delete-dialog.png](images/cd-delete-dialog.png)

#### Referenzen

**Direkt Verwandt:**
- [FR-2](../SRS/TINF24F_1-SRS-2v0.md#fr-2---interaktionen-auf-einzelnen-cds-über-ineraktions-menü) : Interaktionen auf einzelnen CDs über Ineraktions-Menü
- [FR-10](../SRS/TINF24F_1-SRS-2v0.md#fr-10---löschen-einzelner-cds-aus-dem-cd-repository): Löschen einzelner CDs aus dem CD repository
- [NFR-1](../SRS/TINF24F_1-SRS-2v0.md#nfr-1---nutzerfreundlichkeit): Nutzerfreundlichkeit
- [MOD-3](../SAS/TINF24F_1-SAS-3v0.md#mod-3---interaction-menu): Interaction menu
- [MOD-8](../SAS/TINF24F_1-SAS-3v0.md#mod-8---cd-delete-view): CD Delete View

  **Indirekt Verwandt**
- [FR-1](../SRS/TINF24F_1-SRS-2v0.md#fr-1---cds-in-tabelle-anzeigen-sowie-suchen-und-filtern): CDs in Tabelle anzeigen sowie suchen und filtern
- [MOD-1](../SAS/TINF24F_1-SAS-3v0.md#mod-1---cd-table): CD Table
- [MOD-2](../SAS/TINF24F_1-SAS-3v0.md#mod-2---cd-store): CD Store
- [MOD-9](../SAS/TINF24F_1-SAS-3v0.md#mod-9---reference-checker): Reference Checker

---

### UC-10: CDs über JSON Datei importieren

#### Beschreibung

Ein Nutzer soll die Möglichkeit haben, exportierte CDs nochmals zu importieren.
Hierbei ist zu beachten, dass das exportierte CD in einem validen Format vorliegen muss.

#### Designvorstellung

![cd-json-import.png](images/cd-json-import.png)

#### Referenzen

**Direkt Verwandt:**
- [FR-11](../SRS/TINF24F_1-SRS-2v0.md#fr-11---cds-über-json-datei-importieren): CDs über JSON Datei importieren
- [NFR-1](../SRS/TINF24F_1-SRS-2v0.md#nfr-1---nutzerfreundlichkeit): Nutzerfreundlichkeit
- [MOD-12](../SAS/TINF24F_1-SAS-3v0.md#mod-12---json-importer): JSON Importer

  **Indirekt Verwandt**
- [FR-7](../SRS/TINF24F_1-SRS-2v0.md#fr-7---export-funktion-für-cds): Export Funktion für CDs
- [MOD-7](../SAS/TINF24F_1-SAS-3v0.md#mod-7---cd-json-exporter): CD JSON exporter

---

## Funktionale Anforderungen (MoSCoW)

### Muss (M)

- Tabellenansicht mit Sortierung, Textsuche und Pagination
- Detail/ Editor Dialog für DataSpecificationIEC61360 mit Feldvalidierung (Pflichtfelder, Typen, Längen, Pattern).
- CRUD auf dem BaSyx CD Repository via REST.
- IEC CDD Importer
- AASX Importer explizit für enthaltene CDs
- Fehlerbehandlung mit verständlichen Meldungen (z. B. 404/Timeout/ Parsingfehler oder Konflikte).
- Deduplizierung/ Update+Insert anhand eindeutiger Kennung (Identifier/IRDI o. ä.).
- Build /Run fähig in lokaler BaSyx Dev Umgebung.
- Vor dem Speichern prüft das System: Pflichtfelder, Datentypen, zulässige Werte und Identifier Format.
- Export ausgewählter CDs als JSON.
- Rollenbasierter Zugriff(lesen/schreiben/löschen) über vorhandenes BaSyx Auth Konzept.

### Soll (S)

- Massenimport (mehrere URLs/JSON Dateien) mit Ergebnisübersicht.
- Vergleichsansicht zwischen zwei CDs beim importieren.

### Kann (K)

- Internationalisierung (mind. Deutsch/ Englisch) für UI Texte.
- Konfigurierbare Spalten in der Tabellenansicht, Persistenz je Nutzer (Local Storage).
- Tagging/Klassifikation zur besseren Gruppierung von CDs.

## Nicht funktionale Anforderungen (NFA)

- Performance: Import Funktionalität in angemessener Geschwindigkeit analog zu den existierenden import Funktionalitäten
- Sicherheit: Role Based Access Control (RBAC) via Keycloak
- Wartbarkeit: Modulare Architektur, Code Dokumentation.
- Portabilität: Containerisierbar (Docker), Möglichkeit für CI Build auf GitHub Actions.

## Akzeptanzkriterien

- UI: Sortierbare Tabelle, Filter, Suchfeld, Editor Dialog mit Validierung, konsistentes Styling.
- Funktion: CRUD Operationen wirken im Live System (lokal & Demo) sichtbar.
- Importer: Bei Eingabe einer gültigen IEC CDD Datei wird ein valides CD Objekt mit korrekten Feldern
  angelegt/aktualisiert (inkl. Identifier, preferredName, definition, dataType, unit, version/revision).
- Doku: README mit Build/Run, Benutzerhandbuch online verlinkt im BaSyx Repo.
- Community: Mind. ein PR/MR im BaSyx Projekt angenommen oder Review ohne Blocker.