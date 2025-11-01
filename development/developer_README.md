# Einführung

Das Ziel dieses Projekts ist die Entwicklung eines Frontend-Plugins zur Verwaltung der Daten aus dem ConceptDescription-Repository(CD-Repo).
In diesem Dokument wird erklärt, wie die Umgebung für die lokale Entwicklung eingerichtet wird.

# Disclaimer

Um nicht unnötige Schritte zu machen, bitte erst die ganzen Sektionen durchlesen.<br>
Wichtige Anmerkungen kommen meist nach dem Link zu den Guides und sollten entsprechend vorher gelesen werden.

# Inhaltsverzeichnis

<!-- TOC -->
* [Einführung](#einführung)
* [Disclaimer](#disclaimer)
* [Inhaltsverzeichnis](#inhaltsverzeichnis)
  * [Technologies](#technologies)
    * [Windows Subsystem for Linux](#windows-subsystem-for-linux)
    * [Docker (WSL)](#docker-wsl)
      * [Container Starten](#container-starten)
    * [Node Version Manager](#node-version-manager)
      * [Frontend starten](#frontend-starten)
<!-- TOC -->

## Technologies

### Windows Subsystem for Linux

Um auf Windows das Frontend zu starten braucht man WSL.<br>
WSL unterstützt viele Linux Distributionen(distros), kommt aber standardmäßig mit `Ubuntu`, was für ein erstmaliges Linux und Docker setup zu empfehlen ist.
Einen Guide zum aufsetzen von WSL findet man [hier](https://learn.microsoft.com/de-de/windows/wsl/install).

Um mit VS Code und WSL zu arbeiten bitte [diesen guide](https://learn.microsoft.com/de-de/windows/wsl/tutorials/wsl-vscode) befolgen.

### Docker (WSL)

Das CD-Repo wird in einem Docker Container bereitgestellt, dafür braucht man mindestens den `docker daemon` und die `docker engine`.
Zudem wird `docker compose` genutzt um den Container schnell und einfach zu starten.

Für die installation einfach [diesem Kapitel](https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository) des installations guides folgen.

#### Container Starten

Wenn Docker Desktop läuft, kann man den CD-Repo Container starten, indem man im Terminal in den `development/setupFiles` Ordner geht und den folgenden Befehl ausführt:<br>
```cmd
docker compose up -d
```
um den Container wieder zu stoppen fürht man den folgenden Befehl im gleichen Ordner aus:
```cmd
docker compose down
```

### Node Version Manager

Um das Frontend zu starten braucht man Node.js und den Node Package Manager(NPM), damit die Pakete für das Frontend installiert werden können.<br>
Auf Linux gibt es einen `Node Version Manager`(NVM), welcher verwendet werden kann um sehr einfach node zu installieren (NPM wird sofort mit der Node Version installiert).<br>
Zur einfachen Installation, einfach dem [stackoverflow Kommentar](https://stackoverflow.com/a/39981888) folgen und die `"latest node version"` installieren.

**WICHTIG:** für dieses Projekt braucht es keine Ports unter `1024`, dieser Schritt muss also nicht ausgeführt werden.


#### Frontend starten

Als erstes muss man das projekt Klonen und in VS Code öffnen.<br>
Um das Frontend zu starten folgt man am besten dem Unterpunkt [How to develop on Linux](https://github.com/DHBW-TINF24F/Team1-basyx-aas-web-ui/tree/main?tab=readme-ov-file#how-to-develop-on-linux) in der `REAME.md` des [Frontend Repositories](https://github.com/DHBW-TINF24F/Team1-basyx-aas-web-ui/tree/main).<br>
Alle Befehle werden in dem VS Code Terminal ausgeführt.