# Lastenheft - BaSyx ConceptDescription-Plugin(CD-Manager)

Team 1

## Version Control

| **Version** | **Datum** | **Autor** | **Anmerkung** |
| --- | --- | --- | --- |
| 1.0 | 11.10.2025 | Anna | Erste Version |
| 2.0 | 25.10.2025 | Anna | Überarbeitung des Inhalts und  <br>Korrektur von Schreibfehlern |
| 3.0 | 02.11.2025 | Anna | Gezielte Anpassung an die Formatanforderungen |
| 4.0 | 14.11.2025 | Anna | Denglisch -> deutsch & Abgleich mit SRS |

<!-- TOC -->
* [Lastenheft - BaSyx ConceptDescription-Plugin(CD-Manager)](#lastenheft---basyx-conceptdescription-plugincd-manager)
  * [Version Control](#version-control)
  * [Zweck des Dokuments](#zweck-des-dokuments)
  * [Begriffe und Abkürzungen](#begriffe-und-abkürzungen)
  * [Nutzeranforderungen (Use Cases)](#nutzeranforderungen-use-cases)
  * [Funktionale Anforderungen (MoSCoW)](#funktionale-anforderungen-moscow)
    * [Muss (M)](#muss-m)
    * [Soll (S)](#soll-s)
    * [Kann (K)](#kann-k)
  * [Nicht funktionale Anforderungen (NFA)](#nicht-funktionale-anforderungen-nfa)
  * [Akzeptanzkriterien](#akzeptanzkriterien)
<!-- TOC -->

## Zweck des Dokuments

Dokument legt den **Zweck, Umfang und die Anforderungen** für ein BaSyx-GUI-Plugin „CD-Manager" fest - also ein webbasiertes Tool zur zentralen Pflege von Concept Descriptions inklusive. IEC-CDD-Import. Es definiert, **warum** das Plugin gebaut wird und **was** es genau leisten muss, sodass Entwicklung, Test und Abnahme eine klare gemeinsame Referenz haben.

## Umfang und beschreibung des Softwareprodukts

Der abgesteckte Umfang umfasst ein webbasiertes BaSyx-GUI-Plugin („CD-Manager") zur Verwaltung von Concept Descriptions nach DataSpecificationIEC61360 mit komfortabler Listenansicht (Suche, Sortierung, Filter) und einem Detail-/Editor-Dialog inklusive Feld-Validierungen sowie vollständigem CRUD gegen das BaSyx-CD-Repository (REST). Hinzu kommt ein IEC-CDD-Importer: Per URL wird die CDD-Seite geladen, relevante Inhalte werden geparst, auf 61360-Felder gemappt und als CD neu angelegt bzw. anhand eines eindeutigen Identifiers (z. B. IRDI) dedupliziert/aktualisiert; Fehler werden verständlich behandelt und Aktionen geloggt, ein Export ausgewählter CDs als BaSyx-kompatibles JSON ist vorgesehen. Das Ganze läuft in der lokalen BaSyx-Dev-Umgebung und prüft vor dem Speichern Pflichtfelder, Datentypen, zulässige Werte sowie Identifier- und Versionskonventionen.

Erweiterungen sind konfigurierbare Tabellenspalten (nutzerbezogen gespeichert), Internationalisierung der UI (de/en) und rollenbasierter Zugriff; als optionale Features sind Massenimport mit Ergebnisübersicht, Tagging/Klassifikation und eine Vergleichsansicht zwischen zwei CDs vorgesehen. Nicht-funktional definiert der Umfang Ziele zu Performance (z. B. Einzelimport < 5 s; Batch 50 URLs < 5 min), Sicherheit (Auth/Rollen, Eingabevalidierung, SSRF-Schutz), Wartbarkeit (modulare Architektur, Logging, Feature-Flags) und Portabilität (Docker, CI-Build via GitHub Actions) sowie i18n/Locale-Support. Für die Abnahme müssen UI-Elemente und Funktionen sichtbar wirksam sein, der Importer korrekt befüllen/aktualisieren (inkl. identifier, preferredName/definition de/en, dataType, unit, version/revision), und es sind README/Benutzerhandbuch sowie ein Community-Beitrag (PR/MR) gefordert.

## Begriffe und Abkürzungen

- AAS - Asset Administration Shell (Industrie 4.0)
- AASX-Explorer - Referenzwerkzeug zur AAS-Inspektion
- BaSyx - Eclipse-Projekt für AAS-Referenzimplementierungen
- CD - Concept Description
- CRUD - Create, Read, Update, Delete
- DataSpecificationIEC61360 - Template/ Schemadefinition für CDs
- IEC-CDD - IED Common Data Dictionary
- IRDI - International Registration Data Identifier
- tbd. - to be determined

## Nutzeranforderungen (Use Cases)

**UC-1: CD in Tabelle finden & ansehen**

- Für Anwender muss eine sortier-/ filterbare Tabelle existieren (Spalten konfigurierbar), um relevante Datensätze schnell zu finden.

**UC-2: CD anlegen, bearbeiten & löschen (CRUD)**

- Neues CD Objekt gemäß DataSpecificationIEC61360 anlegen und Pflichtfelder validieren.
- Bestehende CD in einem Dialog (Inline Form) ändern und Version/Revision optional fortschreiben.
- CD löschen (mit Sicherheitsabfrage) und ggf. referenzierende Objekte prüfen (Warnung ausgeben).

**UC-3: IEC-CDD-Import per URL**

- Eine IEC CDD URL eingeben, System lädt Seite, extrahiert relevante Inhalte, mappt auf DataSpecificationIEC61360 Felder und legt die CD an.
- Deduplizierung, falls identische/ äquivalente Identifier vorhanden sind und update diese statt neue Anzulegen.

**UC-4: Massenimport**

- Mehrere URLs (Tabelle) einfügen, d.h. System importiert batchweise und protokolliert Erfolg/Fehler je Eintrag.
- Im Fall eines Fehlers soll auch die URL zum Eintrag protokolliert werden (zur einfachen Wiederverwendung).

**UC-5: Export/Integration(optional)**

- Einzelne/mehrere CDs als JSON(BaSyx kompatibel) exportieren.

## Funktionale Anforderungen (MoSCoW)

### Muss (M)

- Tabellenansicht mit Sortierung, Textsuche und optionalen Filtern (z.B. preferredName, idShort, identifier).
- Detail/ Editor Dialog für DataSpecificationIEC61360 mit Feldvalidierung (Pflichtfelder, Typen, Längen, Pattern).
- CRUD auf dem BaSyx CD Repository via REST.
- IEC CDD Importer per URL: HTML abrufen → Inhalte parsen → Mapping → Persistenz.
- Fehlerbehandlung mit verständlichen Meldungen (z. B. 404/Timeout/ Parsingfehler oder Konflikte).
- Deduplizierung/ Update+Insert anhand eindeutiger Kennung (Identifier/IRDI o. ä.).
- Logging/Audit der Import und Änderungsaktionen.
- Build /Run fähig in lokaler BaSyx Dev Umgebung.
- Vor dem Speichern prüft das System: Pflichtfelder, Datentypen, zulässige Werte, Identifier Format und Version/ Revision Konvention.
- Export ausgewählter CDs als JSON.

### Soll (S)

- Konfigurierbare Spalten in der Tabellenansicht, Persistenz je Nutzer (Local Storage).
- Internationalisierung (mind. Deutsch/ Englisch) für UI Texte.
- Rollenbasierter Zugriff(lesen/schreiben/löschen) über vorhandenes BaSyx Auth Konzept.

### Kann (K)

- Massenimport (mehrere URLs/JSON Dateien) mit Ergebnisübersicht.
- Tagging/Klassifikation zur besseren Gruppierung von CDs.
- Vergleichsansicht zwischen zwei CDs.

## Nicht funktionale Anforderungen (NFA)

- Performance: Einzel Import < 5 Sekunden bei erreichbarer CDD, Batch 50 URLs < 5 min (Netzabhängigkeit ausgenommen).
- Sicherheit: Nur authentifizierte Nutzer, Rollen beachten, Eingaben server /clientseitig validieren, SSRF/HTML Parsing absichern (Whitelist).
- Wartbarkeit: Modulare Architektur (Importer, Mapping, UI, API Client getrennt), Logging, Feature Flags für Importer.
- Portabilität: Containerisierbar (Docker), CI Build auf GitHub Actions.
- Internationalisierung: de/en, UTF8 und locale abhängige Formate.

## Akzeptanzkriterien

- UI: Sortierbare Tabelle, Filter, Suchfeld, Editor Dialog mit Validierung, konsistentes Styling.
- Funktion: CRUD Operationen wirken im Live System (lokal & Demo) sichtbar.
- Importer: Bei Eingabe einer gültigen IEC CDD URL wird ein valides CD Objekt mit korrekten Feldern angelegt/aktualisiert (inkl. Identifier, preferredName\[de/en\], definition\[de/en\], dataType, unit, version/revision).
- Doku: README mit Build/Run, Benutzerhandbuch online verlinkt im BaSyx Repo.
- Community: Mind. ein PR/MR im BaSyx Projekt angenommen oder Review ohne Blocker.