# Pflichtenheft - BaSyx ConceptDescription-Plugin (CD-Manager)

## Version Control
| Version |    Date    |  Author  |        Comment        |
|   ---   |     ---    |  ---     |          ---          |
|   1.0   | 23.10.2025 |  Niklas  |     Erste Version     |

## Überblick

Es soll ein Plugin für die bestehende BaSyx-Web-UI erstellt werden mit dem Concept Descriptions angezeigt, durchsucht bearbeitet, importiert und gelöscht werden können. Hierfür stellt BaSyx bereits ein vollständiges Backend bereit, welches durch die bestehende UI jedoch nicht genutzt werden kann.

## Funktionale Anforderungen

### Wireframe

Die geplante UI-Erweiterung folgt grob diesem Layout:

![](images/wireframe.png)

### 1. Header-Navigation

Im oberen Bereich der Anwendung befindet sich ein Header, über den Benutzer:innen zwischen verschiedenen Ansichten der Anwendung wechseln können.

**Anforderungen:**
-  Über die bestehende Navigation im Header soll die Ansicht zur Verwaltung von Content Descriptions erreichbar sein.

### 2. Attribut-Selector

Die linke Seitenleiste dient zur Auswahl von Attributen und enthält verschiedene Steuerungselemente.

**Anforderungen:**

-  Die Seitenleiste kann ein- und ausgeklappt werden, um die verfügbare Fläche für die Tabelle zu vergrößern.

-  Oben in der Seitenleiste befinden sich Schaltflächen für folgende Aktionen:
	- Import von Daten
	- Cloning aus einem CD Repository
	- Einklappen der Seitenleiste

- Die Attribute von Concept Descriptions werden untereinander aufgelistet. Mithilfe von Checkboxen lässt sich auswählen, welche Attribute in der nebenstehenden Tabelle als Spalten angezeigt werden.

### 3. Datentabelle

Im Hauptbereich der Seite wird eine Tabelle angezeigt, die die ausgewählten Concept Descriptions mit ihren Attributen darstellt.

**Anforderungen:**

-  Auf Grundlage der vom Backend bereitgestellten Pagination, werden Concept Descriptions aufgelistet.
-  Es können maximal vier Attribute (Spalten) gleichzeitig angezeigt werden.
-  Alle Spalten der Tabelle können individuell gefiltert und sortiert werden.
-  In allen Spalten ist eine freie Textsuche möglich.
-  Es steht ein Punkt-Menü zur Verfügung, über das einzelne Concept Descriptions exportiert werden können.

### 4. Detailansicht

Die rechte Seitenleiste zeigt Details zur aktuell ausgewählten Concept Description an und ermöglicht die Bearbeitung.

**Anforderungen:**

-  Beim Auswählen eines Eintrags in der Tabelle wird die vollständige Concept Description in der Detailansicht angezeigt.

-  Über eine Schaltfläche kann in den Bearbeitungsmodus gewechselt werden.

-  Im Bearbeitungsmodus kann die Concept Description direkt innerhalb der Detailansicht editiert werden.
    
-  Eine Speichern-Schaltfläche wird eingeblendet, sobald Änderungen vorgenommen wurden.

-  Vor dem Speichern werden die Eingaben validiert. Dabei wird geprüft:
    - Ob alle Pflichtfelder ausgefüllt wurden
    - Ob doppelte Einträge vermieden werden (Duplikat-Erkennung)