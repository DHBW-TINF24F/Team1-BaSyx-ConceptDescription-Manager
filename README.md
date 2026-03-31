# Einführung

Dieses Projekt basiert auf der [Eclipse BaSyx AAS Web UI](https://github.com/eclipse-basyx/basyx-aas-web-ui).<br>
Im Fokus steht die Entwicklung eines Plugins für die Web UI, welches die Nutzung und Verwaltung von ConceptDescriptions,
im
bestehenden [ConceptDescription repository](https://github.com/eclipse-basyx/basyx-java-server-sdk/tree/main/basyx.conceptdescriptionrepository),
über die Web UI ermöglicht.

# Inhaltsverzeichnis

<!-- TOC -->
* [Einführung](#einführung)
* [Inhaltsverzeichnis](#inhaltsverzeichnis)
  * [Ordnerstruktur](#ordnerstruktur)
* [Primäre Ziele](#primäre-ziele)
  * [Concept Descriptions verwalten](#concept-descriptions-verwalten)
  * [Concept Descriptions in Modulen referenzieren](#concept-descriptions-in-modulen-referenzieren)
<!-- TOC -->

## Ordnerstruktur

Alle wichtigen Projektdateien befinden sich im `PROJECT` Ordner, oder werden durch eine im `PROJECT` Ordner befindliche
Datei verlinkt.
Der `development` Ordner beinhaltet wichitge Informationen für Entwickler zur lokalen Entwicklungsumgebung und dem
Projekt setup.

# Primäre Ziele

## Concept Descriptions verwalten

Das Eclipse BaSyx Projekt hat ein vollständiges backend zum verwalten der Concept Descriptions, jedoch ist es nicht über die UI erreichbar.<br>
Nutzer sollten die Standard Create, Read, Update und Delete (CRUD) Operationen über die UI durchführen können.

## Concept Descriptions in Modulen referenzieren

Der AAS Standard bedient sich an der Idee des Common Data Dictionary (CDD), welche durch Concept Descriptions ihren Weg in den Standard finden.  <br>
Sie beschreiben in der realen Welt existierende Konzepte und deren Bedeutung, wobei sie keine weitere Funktion darüber hinaus erfüllen.<br>
Concept Descriptions werden von Modulen referenziert, diese Module sind digitale Representationen eines realen Objekts, welches Konzepten der Realität unterworfen ist z.B. Betriebsspannung.<br>
Nutzer sollen Concept descriptions in Modulen referenzieren, oder eine solche Referenz entfernen können.