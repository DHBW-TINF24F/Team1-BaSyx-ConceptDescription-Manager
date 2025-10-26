# Lastenheft – BaSyx ConceptDescription-Plugin(CD-Manager)

## Version Control
| Version |    Date    | Author |        Comment        |
|   ---   |     ---    |  ---   |          ---          |
|   1.0   | 11.10.2025 |  Anna  |     Erste Version     |
|   2.0   | 25.10.2025 |  Anna  |Überarbeitung des Inhalts und <br> Korrektur von Schreibfehlern|

## 1. Zweck & Zielsetzung

#### Ziel: 
Ein webbasiertes Plugin("CD-Manager") für die BaSyx-GUI zur Verwaltung von Concept Descriptions(CD) nach AAS/ DataSpecificationIEC61360, inklusive komfortabler Listen-/Detail-Bearbeitung und einem IEC-CDD-Importer, der Merkmale aus der IEC-CDD per URL in das CD-Repository übernimmt.<br>
Finaler Nutzen:
     - Zentrale, nutzerfreundliche Pflege von Concept Descriptions für AAS-Projekte.<br>
     - Automatisiertes Einlesen von IEC-CDD-Daten (Zeitgewinn, geringere Fehlerrate).<br>
     - Beitragsfähige Open-Source-Erweiterung für Eclipse BaSyx inklusive Dokumentation und Demo-Hosting.

## 2. Produktkontext & Abgrenzung
 - Systemkontext: BaSyx-Ökosystem (GUI/UI-Layer) mit bestehendem CD-Repository (BaSyx backend/REST). Der CD-Manager ist ein UI-Plugin mit Backend-Adapter.
 - Abhängigkeiten:
     - BaSyx-GUI (Version tbd), BaSyx-Backend(tbd)
     - AASX-Explorer (für Einarbeitung/ Validierung)
     - IEC-CDD(https://cdd.iec.ch) als externe Datenquelle
     - Open-Source-Projekt AAS-Brain als Ideengeber/ Best Practice

## 3. Begriffe & Reverenzen
 - AAS - Asset Administration Shell (Industrie 4.0)
 - AASX-Explorer - Referenzwerkzeug zur AAS-Inspektion
 - BaSyx - Eclipse-Projekt für AAS-Referenzimplementierungen
 - CD - Concept Description
 - CRUD - Create, Read, Update, Delete
 - DataSpecificationIEC61360 - Template/ Schemadefinition für CDs
 - IEC-CDD - IED Common Data Dictionary
 - IRDI  -  International Registration Data Identifier
 - tbd. - to be determined

## 4. Stakeholder & deren Rollen
 - Product Owner/ Auftraggeber – priorisiert Anforderungen und Abnahme
 - Open Source Community (Eclipse BaSyx) – Review, Contribution und Akzeptanz
 - Entwickler – Implementierung, Tests und Doku
 - Fachanwender/ Ingenieure – Pflege/Import von CDs
 - Admin/ DevOps – Build/Deploy, Demo Server und Monitoring

## 5. Nutzeranforderungen (Use Cases)
#### UC-1: CD in Tabelle finden & ansehen
 - Für Anwender muss eine sortier-/ filterbare Tabelle existieren (Spalten konfigurierbar), um relevante Datensätze schnell zu finden.
#### UC-2: CD anlegen, bearbeiten & löschen (CRUD)
 - Neues CD Objekt gemäß DataSpecificationIEC61360 anlegen und Pflichtfelder validieren.
 - Bestehende CD in einem Dialog (Inline Form) ändern und Version/Revision optional fortschreiben.
 - CD löschen (mit Sicherheitsabfrage) und ggf. referenzierende Objekte prüfen (Warnung ausgeben).
#### UC-3: IEC-CDD-Import per URL
 - Eine IEC CDD URL eingeben, System lädt Seite, extrahiert relevante Inhalte, mappt auf DataSpecificationIEC61360 Felder und legt die CD an.
 - Deduplizierung, falls identische/ äquivalente Identifier vorhanden sind und update diese statt neue Anzulegen.
#### UC-4: Massenimport
 - Mehrere URLs (Tabelle) einfügen, d.h. System importiert batchweise und protokolliert Erfolg/Fehler je Eintrag.
 - Im Fall eines Fehlers soll auch die URL zum Eintrag protokolliert werden (zur einfachen Wiederverwendung).
#### UC-5: Nachvollziehbarkeit
 - Änderungshistorie/ Audit: Wer hat wann welche Felder geändert? (Nachlesbar im Detaildialog.)
#### UC-6: Export/Integration(optional)
 - Einzelne/mehrere CDs als JSON(BaSyx kompatibel) exportieren.
## 6. Funktionale Anforderungen (MoSCoW)
#### Muss (M)
1.	Tabellenansicht mit Sortierung, Textsuche und optionalen Filtern (z.B. preferredName, idShort, identifier).
2.	Detail/ Editor Dialog für DataSpecificationIEC61360 mit Feldvalidierung (Pflichtfelder, Typen, Längen, Pattern).
3.	CRUD auf dem BaSyx CD Repository via REST.
4.	IEC CDD Importer per URL: HTML abrufen → Inhalte parsen → Mapping → Persistenz.
5.	Fehlerbehandlung mit verständlichen Meldungen (z. B. 404/Timeout/ Parsingfehler oder Konflikte).
6.	Deduplizierung/ Update+Insert anhand eindeutiger Kennung (Identifier/IRDI o. ä.).
7.	Logging/Audit der Import  und Änderungsaktionen.
8.	Build /Run fähig in lokaler BaSyx Dev Umgebung.
9.	Vor dem Speichern prüft das System: Pflichtfelder, Datentypen, zulässige Werte, Identifier Format und Version/ Revision Konvention.
10.	Export ausgewählter CDs als JSON.
#### Soll(S)
11.	Konfigurierbare Spalten in der Tabellenansicht, Persistenz je Nutzer (Local Storage).
12.	Internationalisierung (mind. Deutsch/ Englisch) für UI Texte.
13.	Rollenbasierter Zugriff(lesen/schreiben/löschen) über vorhandenes BaSyx Auth Konzept.
#### Kann(K)
14.	Massenimport (mehrere URLs/JSON Dateien) mit Ergebnisübersicht.
15.	Tagging/Klassifikation zur besseren Gruppierung von CDs.
16.	Vergleichsansicht zwischen zwei CDs.
## 7. Nicht funktionale Anforderungen (NFA)
 - Performance: Einzel Import < 5 Sekunden bei erreichbarer CDD, Batch 50 URLs < 5 min (Netzabhängigkeit ausgenommen).
 - Sicherheit: Nur authentifizierte Nutzer, Rollen beachten, Eingaben server /clientseitig validieren, SSRF/HTML Parsing absichern (Whitelist).
 - Wartbarkeit: Modulare Architektur (Importer, Mapping, UI, API Client getrennt), Logging, Feature Flags für Importer.
 - Portabilität: Containerisierbar (Docker), CI Build auf GitHub Actions.
 - Internationalisierung: de/en, UTF8 und locale abhängige Formate.
## 8. Akzeptanzkriterien
1.	UI: Sortierbare Tabelle, Filter, Suchfeld, Editor Dialog mit Validierung, konsistentes Styling.
2.	Funktion: CRUD Operationen wirken im Live System (lokal & Demo) sichtbar.
3.	Importer: Bei Eingabe einer gültigen IEC CDD URL wird ein valides CD Objekt mit korrekten Feldern angelegt/aktualisiert (inkl. Identifier, preferredName[de/en], definition[de/en], dataType, unit, version/revision).
4.	Doku: README mit Build/Run, Benutzerhandbuch online verlinkt im BaSyx Repo.
5.	Community: Mind. ein PR/MR im BaSyx Projekt angenommen oder Review ohne Blocker.

