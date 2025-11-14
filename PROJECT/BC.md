Bussines Case

BaSyx-ConceptDescription-Manager

| Auftraggeber:    | M. Rentschler & A. Zielstorff |
|:-----------------|-------------------------------|
| Firmenstandort:  | Lerchenweg 1, 70178           |
| Hersteller Name: | Team 1           |

| Rolle                 | Name der Beteiligten |
|-----------------------|----------------------|
| Projektleiter         | Anna                 |
| Projektmanager        | Niklas               |
| Testmanager           | Priyanshu            | 
| Systemarchitekt       | Chris                | 
| Technischer Redalteur | Lütfi                |

| **Version** | **Datum**  | **Autor** | **Anmerkung** |
|-------------|------------|-----------|---------------|
| 1.0         | 14.11.2025 | Anna      | Erste Version |

<!-- TOC -->
## Inhaltsverzeichnis
  * [Umfang und beschreibung des Softwareprodukts](#umfang-und-beschreibung-des-softwareprodukts)
  * [Einführung](#einführung)
  * [Qualitative und quantitative Vorteile des Produkts](#qualitative-und-quantitative-vorteile-des-produkts)
  * [Zeitlicher Ablauf](#zeitlicher-ablauf)
  * [Kostenplan](#kostenplan)
    * [Fixkosten](#fixkosten)
    * [Variable Kosten](#variable-kosten)
  * [Abschließender vermerk](#abschließender-vermerk)
<!-- TOC -->

## Umfang und beschreibung des Softwareprodukts
Der abgesteckte Umfang umfasst ein webbasiertes BaSyx-GUI-Plugin („CD-Manager") zur Verwaltung von Concept Descriptions nach DataSpecificationIEC61360 mit komfortabler Listenansicht (Suche, Sortierung, Filter) und einem Detail-/Editor-Dialog inklusive Feld-Validierungen sowie vollständigem CRUD gegen das BaSyx-CD-Repository (REST). Hinzu kommt ein IEC-CDD-Importer: Per URL wird die CDD-Seite geladen, relevante Inhalte werden geparst, auf 61360-Felder gemappt und als CD neu angelegt bzw. anhand eines eindeutigen Identifiers (z. B. IRDI) dedupliziert/aktualisiert; Fehler werden verständlich behandelt und Aktionen geloggt, ein Export ausgewählter CDs als BaSyx-kompatibles JSON ist vorgesehen. Das Ganze läuft in der lokalen BaSyx-Dev-Umgebung und prüft vor dem Speichern Pflichtfelder, Datentypen, zulässige Werte sowie Identifier- und Versionskonventionen.

## Einführung
Im Rahmen des Eclipse-BaSyx-Ökosystems spielen Concept Descriptions (CDs) eine zentrale Rolle für die semantische Beschreibung von Merkmalen und Parametern innerhalb von Asset Administration Shells. In der aktuellen Praxis ist die Einsicht und Pflege dieser Concept Descriptions jedoch nur über technische Schnittstellen möglich und für viele Anwenderinnen und Anwender wenig zugänglich. Dies erschwert die korrekte Referenzierung von CDs in AAS-Modellen und erhöht das Risiko fehlerhafter oder inkonsistenter Konfigurationen. Mit dem geplanten BaSyx-GUI-Plugin „CD-Manager“ wird diese Lücke geschlossen: Die Erweiterung der bestehenden Web UI stellt eine benutzerfreundliche Oberfläche bereit, über die Concept Descriptions zentral eingesehen, verwaltet und referenziert werden können. Auf diese Weise soll die Arbeit mit dem Concept-Description-Repository deutlich vereinfacht, die Datenqualität gesteigert und die praktische Nutzbarkeit des BaSyx-Ökosystems insgesamt erhöht werden.

## Qualitative und quantitative Vorteile des Produkts
Der CD-Manager verbessert die Benutzerfreundlichkeit der BaSyx Web UI erheblich, da Concept Descriptions nicht mehr abstrakt „im Hintergrund“, sondern transparent und intuitiv zugänglich sind. Dies führt zu einer höheren Datenqualität, weniger Fehlkonfigurationen und einer insgesamt gesteigerten Akzeptanz des AAS-Ansatzes bei Anwenderinnen und Anwendern. Insbesondere lassen sich Zeitgewinne bei der Suche, Pflege und Referenzierung von Concept Descriptions sowie reduzierte Aufwände für Fehleranalyse und Nacharbeit erwarten. Schon konservativ angesetzte Einsparungen weniger Stunden pro Monat und Nutzergruppe können, über mehrere Projekte und Organisationen hinweg, die einmaligen Entwicklungskosten des Plugins deutlich übersteigen und so einen klaren wirtschaftlichen Mehrwert erzeugen.

## Zeitlicher Ablauf
Die Zeitliche Rahmen des ersten Projektteils geht vom 26.09.2025 bis zum 0.11.2025 statt. In den ersten zwei Wochen wurden die organisatorischen Themen, sowie die Projektplanung gemacht. Der rest der Zeit wurde für die technische Planung und die Entwicklung des Prototyps aufgewendet. Der gesamtaufwand pro Person dürfte bis Ende des vierten Semesters, wenn das Projekt vollständig abgesschlossen wird bei ca. 150 bis 200 Stunden pro Person liegen.

| Aktivität                      | geplante Dauer | tatsächliche Dauer  |  Fertigstellungsgrad |
|--------------------------------| -------------- | ------------------- |  ------------------- |
| Planung (CRS, SRS, BC, SAS, …) | 1 Woche        | 2 Wochen            |  80 %                |
| Entwicklung des Prototyps      | 7 Wochen       | 6 Wochen            |  80 %                |
| Umsetzung                      | 8 Wochen       |                     |  0 %                 |
| Tests                          | 3 Wochen       |                     |  0 %                 |
| Abschluss                      | 1 Woche        |                     |  0 %                 |


## Kostenplan
In diesem Projekt wird ausschließlich eine Softwarelösungen entwickelt. Die Projektkosten entstehen daher im Wesentlichen durch den Einsatz von Softwarewerkzeugen, der vorhandenen Büroarbeitsplätze, einer geeigneten Serverumgebung sowie den Arbeitsstunden des Projektteams. Der Großteil der eingesetzten Tools ist kostenfrei oder steht als Open-Source-Software zur Verfügung. Da alle Beteiligten remote arbeiten können und bereits über eine übliche Büroausstattung verfügen, fallen keine zusätzlichen Ausgaben für Büroräume oder Hardware an. Somit entstehen zusätzlich lediglich noch die laufenden Kosten für Energie und Internet.

### Fixkosten
| Fixkosten                 | Kosten pro Person | Anzahl | Kosten für das gesamte Team |
| ------------------------- |-------------------| ------ |-----------------------------|
| Energie & Internet        | 250 €             | 5      | 1.250 €                     |
| **Summe Fixkosten**       |                   |        | **1.250 €**                 |

### Variable Kosten
| Rolle                          | Teammitglied | Stundensatz | Kosten für 170 Stunden |
|--------------------------------|--------------|-------------|------------------------|
| Projektleiter                  | Anna         | 45 €        | 7.650 €                |
| Projektmanager                 | Niklas       | 30 €        | 5.100 €                |
| Testmanager                    | Priyanshu    | 25 €        | 4.250 €                |
| Systemarchitekt                | Chris        | 25 €        | 4.250 €                |
| Technischer Redalteur          | Lütfi        | 30 €        | 5.100 €                |
| **Geschätzte variable Kosten** |              |             | **24.650 €**           |


Die variablen Projektkosten ergeben sich aus den Stundenlöhnen der beteiligten Rollen und der geplanten Arbeitszeit. Die vorrausgegangene Tabelle zeigen die Verteilung der Kosten nach Mitarbeitenden.

## Abschließender vermerk
Obwohl für die Entwicklung Gesamtkosten in Höhe von 25.900 € anfallen, wird das Ergebnisprojekt unter als Erweiterung eines Open Source Projektes veröffentlicht und den Anwendern kostenfrei zur Verfügung gestellt. Eine direkte Monetarisierung über Lizenzgebühren ist nicht vorgesehen. Das Plugin dient vielmehr als Referenz- und Demonstrationsprojekt, mit dem die technische Kompetenz des Unternehmens sichtbar gemacht, die Verbreitung des BaSyx-Ökosystems unterstützt und mittelbar neue Beratungs- und Entwicklungsaufträge gewonnen werden sollen.


