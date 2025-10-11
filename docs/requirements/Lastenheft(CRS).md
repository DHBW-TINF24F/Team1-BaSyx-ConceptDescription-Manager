# Lastenheft – BaSyx ConceptDescription-Plugin(CD-Manager)

## Version Control
| Version |    Date    | Author |        Comment        |
|   ---   |     ---    |  ---   |          ---          |
|   1.0   | 11.10.2025 |  Anna  |     Erste Version     |

## 1. Zweck & Zielsetzung

#### Ziel: 
Ein webbasiertes Plubin("CD-Manager") für die BaSyx-GUI zur Berwaltung von Concept Descriiptions(CD) nach AAS/ DataSpecificationIEC61360, inklusieve komfortabler Listen-/Detail-Bearbeitung und einem IEC-CDD-Importer, der Merkmale aus der IEC-CDD per URL in das CD-Rapository übernimmt.

#### Finaler Nutzen:
 - Zentrale, nutzerfreundliche Pflege von Concept Descriptions für AAS-Projekte.
 - Auromatisertes Einlesen von IEC-CDD-Ddaten(Zeitgewinn, geringere Fehlerrate, höhere Konsistenz).
 - Beitragsfähige Open-Source-Erweiterung für Eclipse BaSyc inklusive Dokumentation und Demo-Hosting.

#### Nicht-Ziel:
 - Entwicklung eines vollständigen AAS-Backends(BaSyc stellt Backend)
 - generische Web-Crawler-Funktionalität außerhalb der IEC-CDD
 - tiefgreifende Datenqualitätsprüfung, abseits des IEC61360-Schemas.


## 2. Produktkontext & Abgrenzung
 - Systemkontext: BaSyc-Ökosystem(GUI/UI-Layer) mit bestehendem CD-Repository(BaSyx Backend/REST). Der CD-Manager ist ein UI-Plugin mit Backend-Adapter.

 - Abhängigkeiten:
    - BaSyx-GUI(Version tbd), BaSyx-Backend(tbd)
    - AASC-Explorer(für Einarbeitung/ Validierung)
    - IEC-CDD(https://cdd.iec.ch) als externe Datenquelle
    - Open-Source-Projekt AAS-Brain als Ideengeber/ Best Practice

 - Abgrenzung: 
    - Keine eigenständige Identity- und Access-Management-Lösung
    - keine Garantie für permanente Erreichbarkeit der externen IEC-CDD-Seiten
    - keine Offline-Spiegelung der CDD


## 3. Begriffe & Reverenzen
 - AAS  -  Asset Administration Shell(Industrie 4.0)
 - CD  -  Concept Description
 - DataSpecificationIEC61360  -  Template/ Schemadefinition für CDs
 - IEC-CDD  -  IED Common Data Dictionary
 - CRUD  -  Create, Read, Update, Delete
 - AASX-Explorer  -  Referenzwerkzeug zur AAS-Instpektion
 - BaSyx  -  Exlopse-Projekt für AAS-Raferenzimplementierungen
 - tbd.  -  to be determined


## 4. Stakeholder & derren Rollen
 - Product Owner/ Auftraggeber  –  priorisiert Anforderungen und Abnahme
 - Open‑Source‑Community(Eclipse BaSyx)  –  Review, Contribution und Akzeptanz
 - Entwickler  –  Implementierung, Tests und Doku
 - Fachanwender/ Ingenieure – Pflege/Import von CDs
 - Admin/ DevOps  –  Build/Deploy, Demo‑Server und Monitoring


## 5. Nutzheranforderungen(Use Cases)

#### UC-1: CD in Liste finden & ansehen
 - Für Anwender muss eine sortier-/ filterbare Liste existieren(Spalten konfigurierbar), um relevante Datensätze schnell zu finden.

#### UC-2: CD anlegen, bearbeiten & löschen(CRUD)
 - Neues CD‑Objekt gemäß DataSpecificationIEC61360 anlegen und Pflichtfelder validieren.
 - Bestehende CD in einem Dialog(Inline‑Form) ändern und Version/Revision optional fortschreiben.
 - CD löschen(mit Sicherheitsabfrage) und ggf. referenzierende Objekte prüfen(Warnung ausgeben).

#### UC-3: IEC-CDD-Import per URL
 - Eine IEC‑CDD‑URL(z. B. PropertiesAllVersions/…) eingeben, System lädt Seite, extrahiert relevante Inhalte, mappt auf DataSpecificationIEC61360‑Felder und legt/aktualisiert die CD an.
 - Deduplizierung: Falls identische/ equivalente Identifier vorhanden sind und Update statt Neuanlage.

#### UC‑4: Massenimport
 - Mehrere URLs(Liste/CSV) einfügen, d.h. System importiert batchweise und protokolliert Erfolg/Fehler je Eintrag.

#### UC‑5: Validierung & Qualität
 - Vor dem Speichern prüft das System: Pflichtfelder, Datentypen, zulässige Werte, Identifier‑Format und Version/ Revision‑Konvention.

#### UC‑6: Nachvollziehbarkeit
 - Änderungshistorie/ Audit: Wer hat wann welche Felder geändert?(Nachlesbar im Detaildialog.)

#### UC‑7: Export/Integration(optional)
 - Einzelne/mehrere CDs als JSON(BaSyx‑kompatibel) exportieren.


## 6. Funktionale Anforderungen(MoSCoW)
#### Muss(M)
1. Listenansicht mit Sortierung, Textsuche und optionalen Filtern(z.B. preferredName, idShort, identifier).
2. Detail/ Editor‑Dialog für DataSpecificationIEC61360 mit Feldvalidierung(Pflichtfelder, Typen, Längen, Pattern).
3. CRUD auf dem BaSyx‑CD‑Repository via REST.
4. IEC‑CDD‑Importer per URL: HTML abrufen → Inhalte parsen → Mapping → Persistenz.
5. Fehlerbehandlung mit verständlichen Meldungen(z. B. 404/Timeout/Parsingfehler oder Konflikte).
6. Deduplizierung/ Update+Insert anhand eindeutiger Kennung(Identifier/IRDI o. ä.).
7. Logging/Audit der Import‑ und Änderungsaktionen.
8. Build‑/Run‑fähig in lokaler BaSyx‑Dev‑Umgebung.

#### Soll(S)
9. Massenimport(mehrere URLs/CSV‑Upload) mit Ergebnisübersicht.
10. Konfigurierbare Spalten in der Listenansicht, Persistenz je Nutzer(Local Storage).
11. Internationalisierung (mind. Deutsch/ Englisch) für UI‑Texte.
12. Rollenbasierter Zugriff(lesen/schreiben/löschen) über vorhandenes BaSyx‑Auth‑Konzept.

#### Kann(K)
13. Export ausgewählter CDs als JSON/CSV.
14. Tagging/Klassifikation zur besseren Gruppierung.
15. Vergleichsansicht(Diff) zwischen Versionen/Revisionen.


## 7. Datenmodell & Feldmapping(IEC61360⇄CD)
 - identifier(IRDI/IRI), idShort, preferredName(multi‑lang) und definition-(multi‑lang)
 - dataType(IEC61360‑Werteliste), unit und unitId
 - valueFormat/levelType(falls genutzt) und valueList(optional)
 - version, revision, sourceUrl(Importherkunft), lastImportedAt

#### Mapping‑Regeln:
 - Aus der IEC‑CDD‑Seite werden Bezeichnungen(en/de), Definitionen, Datentyp, Einheit, IRDI/Identifier und Versionsinfos maschinell extrahiert.
 - Priorität: strukturierte Informationen(Tabellen/Metadaten) statt Freitext.
 - Mehrsprachigkeit: falls verfügbar, in Sprachlisten übernehmen(de, en).

#### Validierung:
 - Pflichtfelder: identifier, preferredName, dataType, Syntaxprüfung für Identifier(Pattern) und Wertemengenprüfung für dataType.

## 8. Schnittstellen
 - BaSyx‑Backend(REST): Endpunkte für CD‑Repository(tbd: Pfade, Auth, Payloads und Statuscodes).
 - HTTP‑Client → IEC‑CDD: GET auf übergebene URL(s), User‑Agent setzen, Timeouts/Retry und Robots/ToS beachten.
 - AASX‑Explorer: kein Hard‑Interface, wird zu Einarbeitung und Cross‑Check genutzt.

## 9. Nicht‑funktionale Anforderungen(NFA)
 - Usability: Listenperformance bei ≥5.000 CDs, Tastatur‑Navigation, klare Fehlermeldungen, konsistentes UI(BaSyx‑Look&Feel).
 - Performance: Einzel‑Import < 5 s bei erreichbarer CDD, Batch 50 URLs < 5 min(Netzabhängigkeit ausgenommen). Client bleibt responsiv.
 - Sicherheit: Nur authentifizierte Nutzer, Rollen beachten, Eingaben server‑/clientseitig validieren, SSRF/HTML‑Parsing absichern(Whitelist Domain cdd.iec.ch, no arbitrary fetch).
 - Robustheit: Timeouts/Retry‑Backoff, Idempotenz beim Upsert, Transaktionssicherheit(entweder vollständiges Mapping oder Rollback).
 - Wartbarkeit: Moduläre Architektur(Importer, Mapping, UI, API‑Client getrennt), Logging, Feature‑Flags für Importer.
 - Portabilität: Containerisierbar(Docker), CI‑Build auf GitHub Actions.
 - Kompatibilität: Aktuelle Chrome/Firefox/Edge(2 letzte Major‑Versionen) und Responsive UI(≥1280px optimiert, ≥1024px nutzbar).
 - Internationalisierung: de/en, UTF‑8und locale‑abhängige Formate.
 - Barrierefreiheit(nice‑to‑have): ARIA‑Labels, Kontrast, Fokus‑States.


## 10. Architektur‑ & Technologiepräferenzen (ToDo: Muss noch mit SA(Chris) besprochen werden)
 - Frontend: Web‑Plugin im BaSyx‑GUI‑Stack(tbd: React/Angular gemäß BaSyx‑Vorgaben), TypeScript, UI‑Komponenten analog BaSyx.
 - Importer: Service/Worker mit HTML‑Parser(DOM‑basierend) und Mappings als deklarative Regeln(z. B. JSON‑Schema/Mapping‑Table).
 - Backend‑Adapter: REST‑Client mit Typsicherheit, Retry/Backoff, Token‑Handling gem. BaSyx.
 - Build: Node + BaSyx‑Buildchain, CI(GitHub Actions), Linting/Prettier, Unit‑/E2E‑Tests.


## 11. Qualitätssicherung & Tests (ToDo: Muss noch mit TM(Priyanshu) besprochen werden)
 - Unit‑Tests: Mapping/Validator(≥80% Coverage Importer‑Kern).
 - E2E‑Tests: CRUD‑Flows, Import‑Flow(Mock für IEC‑CDD) und Fehlerszenarien.
 - Cross‑Browser‑Smoke‑Tests(Chrome/Firefox/Edge).
 - Testdaten: Beispiel‑URLs(mind. 5) aus IEC‑CDD und künstliche Fehlerfälle.
 - Akzeptanzkriterien(Auszug):
    1. Liste bietet Sortierung, Textsuche, Filter, 5.000 Datensätze bleiben bedienbar.
    2. Editor erzwingt Pflichtfelder, valide Speicherung im BaSyx‑Repository.
    3. Import einer gültigen IEC‑CDD‑URL erzeugt korrekt gemapptes CD‑Objekt inkl. Identifier, preferredName, dataType und unit.
    4. Deduplizierung: erneuter Import gleicher URL aktualisiert denselben Datensatz(kein Duplikat).
    5. Batch‑Import zeigt pro Eintrag Status(OK/Fehler) und detaillierte Fehlermeldung.


## 12. Betrieb & Hosting(Demo)
 - Ziel: Öffentlicher Demo‑Server(HTTPS) für Community‑Tests.
 - Technik: Container‑Deployment(Docker/Compose), Reverse Proxy(nginx), TLS und Basic Auth für Schreibzugriffe(oder read‑only Demo + separater Schreib‑Sandbox).
 - Monitoring/Logs: Access/Error‑Logs, Import‑Logs und Health‑Checks.
 - Backup: Snapshot des CD‑Repos(täglich) und Restore‑Prozess dokumentiert.


## 13. Dokumentation & Onboarding
 - Benutzerdoku: Quick‑Start, Tour durch Liste/Editor, Import‑How‑To, Fehlersuche.
 - Entwicklerdoku: Architekturüberblick, Mapping‑Regeln, API‑Kontrakte, Lokale Dev‑Umgebung, Test/CI und Release.
 - Tutorial‑Ergänzungen: Identifizierte Lücken in bestehenden BaSyx‑Tutorials werden als PR/Issue dokumentiert.


## 14. Risiken & Gegenmaßnahmen
 - HTML‑Struktur CDD ändert sich → Parser bricht: Lösung: Selektoren zentral, Fallback‑Heuristiken, Monitor‑Tests.
 - Rechtliche/ToS‑Aspekte IEC‑CDD → Nur lesender Zugriff, Rate‑Limit/Robots beachten, Caching minimal.
 - Dateninkonsistenzen → Strenge Validierung, klare Fehlermeldungen, manuelle Nachpflege ermöglichen.
 - Community‑Akzeptanz → Frühe PR‑Entwürfe, Coding‑Standards einhalten und Maintainer früh einbinden.
 - Leistung bei großen Datenmengen → Virtuelle Listen, Pagination, selektive Felder und Backend‑Filter.


## 15. Offene Punkte & Annahmen
 - BaSyx‑Version & GUI‑Stack(React/Angular)  –  tbd.
 - Exakte REST‑Endpunkte und Auth‑Mechanik des CD‑Repos  –  tbd.
 - Vollständige Feldliste des DataSpecificationIEC61360 für Editor/Validierung  –  wird im Mapping‑Anhang konkretisiert.
 - Lizenz/Compliance für CDD‑Scraping  –  Prüfen und dokumentieren.
 - Demo‑Server‑Rollenmodell(read‑only vs. Schreibzugriff)  –  Entscheidung durch Team oder Dozent.


## 16. Akzeptanzkriterien
1. UI: Sortierbare Liste, Filter, Suchfeld, Editor‑Dialog mit Validierung, konsistentes Styling.
2. Funktion: CRUD‑Operationen wirken im Live‑System(lokal & Demo) sichtbar.
3. Importer: Bei Eingabe einer gültigen IEC‑CDD‑URL wird ein valides CD‑Objekt mit korrekten Feldern angelegt/aktualisiert(inkl. Identifier, preferredName[de/en], definition[de/en], dataType, unit, version/revision).
4. Doku: README mit Build/Run, Benutzerhandbuch online verlinkt im BaSyx‑Repo.
5. Community: Mind. ein PR/MR im BaSyx‑Projekt angenommen oder Review ohne Blocker.