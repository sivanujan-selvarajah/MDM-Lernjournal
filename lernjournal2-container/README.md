# Lernjournal 2 Container

## Docker Web-Applikation

### Verwendete Docker Images

| | Bitte ausfüllen |
| -------- | ------- |
| Image 1 | nextcloud |
| Image 1 | [https://hub.docker.com/_/nextcloud](https://hub.docker.com/_/nextcloud) |
| Image 2 | mariadb |
| Image 2 | [https://hub.docker.com/_/mariadb](https://hub.docker.com/_/mariadb)  |
| ... | ... |
| Docker Compose |  |

---

## Inhaltsverzeichnis

  - [Docker Web-Applikation](#docker-web-applikation)
    - [Verwendete Docker Images](#verwendete-docker-images)
    - [Dokumentation manuelles Deployment](#dokumentation-manuelles-deployment)
      - [Screenshots – Manuelles Deployment (Nextcloud + MariaDB)](#screenshots--manuelles-deployment-nextcloud--mariadb)
    - [Dokumentation Docker-Compose Deployment](#dokumentation-docker-compose-deployment)
  - [Deployment ML-App](#deployment-ml-app)
    - [Variante und Repository](#variante-und-repository)
    - [Dokumentation lokales Deployment](#dokumentation-lokales-deployment)
      - [Screenshots – ONNX Image Classification](#screenshots--onnx-image-classification)
    - [Dokumentation Push nach Docker Hub](#dokumentation-push-nach-docker-hub)
      - [Screenshots – Dokumentation Push nach Docker Hub](#screenshots--dokumentation-push-nach-docker-hub)
    - [Dokumentation Deployment Azure Web App](#dokumentation-deployment-azure-web-app)
      - [Vorgehen (Azure CLI)](#vorgehen-azure-cli)
      - [Einstellungen](#einstellungen)
      - [Screenshots](#screenshots)
      - [Learnings & Herausforderungen](#learnings--herausforderungen)
  - [Reflexion](#reflexion)
  - [Quellenverzeichnis](#quellenverzeichnis)
  - [Abbildungsverzeichnis](#abbildungsverzeichnis)

---

### Dokumentation manuelles Deployment

- Docker Images `nextcloud` und `mariadb` wurden einzeln mit `docker pull` heruntergeladen
- Netzwerk erstellt: `docker network create nextcloud-net`
- Datenbank-Container gestartet:
  ```bash
  docker run -d --name mariadb --network nextcloud-net \
    -e MYSQL_ROOT_PASSWORD=secret \
    -e MYSQL_PASSWORD=nextcloud \
    -e MYSQL_DATABASE=nextcloud \
    -e MYSQL_USER=nextcloud \
    mariadb

- Danach Nextcloud-Container gestartet:

  ```bash
  docker run -d --name nextcloud --network nextcloud-net \
    -p 8080:80 nextcloud

- Im Browser erreichbar unter: `http://localhost:8080`
- Setup durch Web-UI abgeschlossen (Admin-User, Datenbankverbindung)
- Beide Container waren aktiv und sichtbar im Docker Dashboard und über `docker ps`

Im Rahmen dieses Lernjournals habe ich eine Web-Applikation manuell mit Docker deployed – bestehend aus Nextcloud als Anwendung und MariaDB als Datenbank. Ich habe mich bewusst für diese Kombination entschieden, da sie ein realistisches Beispiel für den Betrieb einer produktiven, datenbankgestützten Webanwendung darstellt. Besonders interessant war dabei das Zusammenspiel zweier Container, die über ein gemeinsam erstelltes Docker-Netzwerk kommunizieren.

Die Container wurden mit den offiziellen Images von Docker Hub gestartet. Zuerst habe ich MariaDB mit den notwendigen Umgebungsvariablen wie Benutzername, Passwort und Datenbankname eingerichtet. Anschließend wurde Nextcloud über den Parameter --network mit dem gleichen Netzwerk verbunden, wodurch die Applikation im Setup-Prozess auf die Datenbank zugreifen konnte.

In der Web-GUI von Nextcloud habe ich einen Admin-Account erstellt und die Verbindung zur MariaDB eingerichtet. Die Werte für Datenbank, Benutzername und Passwort entsprachen dabei den vorher gesetzten Umgebungsvariablen. Danach war die Anwendung unter http://localhost:8080 vollständig nutzbar.

Technisch war der herausforderndste Teil die Sicherstellung der funktionierenden Netzwerkverbindung zwischen den beiden Containern. Ohne ein benanntes Netzwerk konnte Nextcloud die Datenbank nicht erreichen – erst durch die Verwendung eines benutzerdefinierten Netzwerks (nextcloud-net) wurde das Problem gelöst. Dabei habe ich gelernt, wie wichtig DNS-Namensauflösung innerhalb von Docker-Netzwerken ist, und wie Container über Servicenamen ansprechbar werden.

Insgesamt war das manuelle Deployment eine wertvolle Übung, um die Zusammenhänge von Containern, Netzwerken, Ports und Umgebungsvariablen besser zu verstehen. Es hat deutlich gemacht, wie viel Konfigurationsaufwand hinter einem scheinbar einfachen Setup steckt und warum Automatisierung mit Docker Compose im nächsten Schritt sinnvoll ist.

#### Screenshots – Manuelles Deployment (Nextcloud + MariaDB)

![Docker Pull – Nextcloud](images/pull-nextcloud.png)  
**Abb. 1:** Docker Pull für das Nextcloud-Image

![Docker Pull – MariaDB](images/run-mariadb.png)  
**Abb. 2:** Startbefehl für MariaDB mit Umgebungsvariablen

![Docker Network & Verbindung](images/docker-network-connect.png)  
**Abb. 3:** Erstellen eines Docker-Netzwerks und Verbinden beider Container

![Nextcloud Setup GUI](images/nextcloud-setup-gui.png)  
**Abb. 4:** Web-basierte Einrichtung von Nextcloud

![Docker Dashboard – laufende Container](images/docker-containers-dashboard.png)  
**Abb. 5:** Übersicht laufender Container: Nextcloud & MariaDB

![Nextcloud Loginmaske](images/nextcloud-login.png)  
**Abb. 6:** Login-Bildschirm von Nextcloud nach erfolgreichem Setup

![Docker PS – laufende Container CLI](images/docker-ps.png)  
**Abb. 7:** Ausgabe von `docker ps` mit Containerstatus und Port-Mapping

---

### Dokumentation Docker-Compose Deployment

- `docker-compose.yml` erstellt mit den Diensten `db` und `app`
- Volumes zur Datenpersistenz definiert
- Kommandos:
  ```bash
  docker-compose up -d


- Funktionierte identisch wie manuelles Deployment
- Web-Oberfläche über `http://localhost:8080` geöffnet
- Screenshots vom Container-Start und Web-App im Browser vorhanden 

![docker-compose.yml – Konfiguration Nextcloud + MariaDB](images/docker-compose.png)  
**Abb. 8:** Inhalt der `docker-compose.yml` mit Definition der Services `db` und `app`


Nachdem ich die Web-Applikation (Nextcloud + MariaDB) manuell deployed hatte, habe ich das Setup mithilfe von Docker Compose automatisiert. Ziel war es, beide Container über eine zentrale Konfigurationsdatei (docker-compose.yml) zu starten, anstatt jeden Dienst einzeln per CLI zu verwalten.

Die Konfiguration der Services in der Compose-Datei umfasste das Definieren von Umgebungsvariablen, Volumes zur Datenpersistenz und ein benanntes Netzwerk zur Kommunikation zwischen den Containern. Zusätzlich habe ich mit depends_on sichergestellt, dass die Datenbank beim Start von Nextcloud bereits bereitsteht. Über den Befehl docker-compose up -d konnte ich die gesamte Applikation mit einem einzigen Schritt starten.

Im Browser war Nextcloud anschließend wie zuvor unter http://localhost:8080 erreichbar, und die Daten wurden erfolgreich gespeichert. Besonders praktisch war, dass nach einem Neustart des Systems oder einem docker-compose down das Setup vollständig rekonstruierbar blieb – inklusive aller Daten im Volume.

Der Unterschied zum manuellen Deployment war deutlich spürbar: Die Konfiguration war zentralisiert, die Wiederholbarkeit höher und die Fehleranfälligkeit geringer. Ich habe dadurch verstanden, wie Docker Compose in realen Projekten nicht nur Zeit spart, sondern auch die Wartbarkeit verbessert – vor allem bei mehreren voneinander abhängigen Services.

---

## Deployment ML-App

### Variante und Repository

| Gewähltes Beispiel | Bitte ausfüllen |
| -------- | ------- |
| onnx-sentiment-analysis | Nein |
| onnx-image-classification | Ja |
| Repo URL Fork | https://github.com/sivanujan-selvarajah/onnx-image-classification |
| Docker Hub URL | https://hub.docker.com/repository/docker/sivanujan26/onnx-image-classification/general |

---

### Dokumentation lokales Deployment

- Image lokal mit Dockerfile gebaut
  ```bash
  docker build -t sivanujan/onnx-image-classification .

- Container gestartet:
  ```bash
  docker run --name onnx-image-classification -p 9000:5000 -d sivanujan/onnx-image-classification 

  
- Web-App erfolgreich unter `http://localhost:5000` geöffnet


#### Screenshots – ONNX Image Classification

![Docker Build – ONNX](images/onnx-docker-build.png)  
**Abb. 9:** Build-Prozess der ONNX Image Classification App mit Dockerfile

![Docker Run – ONNX](images/onnx-docker-run.png)  
**Abb. 10:** Starten der ONNX-App lokal auf Port 5000

#### Dokumentation Push nach Docker Hub

- Nach erfolgreichem lokalen Test wurde ein Repository bei Docker Hub erstellt.
- Das Repository trägt den Namen `sivanujan26/onnx-image-classification`.
- Das lokal gebaute Image wurde mit folgendem Befehl hochgeladen:

  ```bash
  docker push sivanujan26/onnx-image-classification:latest
  ```
- Der Upload umfasst mehrere Layer, wovon viele aus dem Python-Basisimage wiederverwendet wurden.
- Das Image ist nun öffentlich auf Docker Hub verfügbar und kann mit folgendem Befehl gezogen werden:

  ```bash
  docker pull sivanujan26/onnx-image-classification:latest
  ```
  Nach erfolgreichem Testen des lokal gebauten Images habe ich ein öffentliches Repository auf Docker Hub erstellt und das Image hochgeladen. So war es für spätere Deployments über Azure bereit.

  
#### Screenshots – Dokumentation Push nach Docker Hub

![Erstellung des öffentlichen Repositories auf Docker Hub](images/dockerhub-create-repo.png)  
**Abb. 10a:** Erstellung des öffentlichen Repositories auf Docker Hub

![Upload des Docker-Images zu Docker Hub mit `docker push`](images/docker-push-onnx.png)  
**Abb. 11:** Upload des Docker-Images zu Docker Hub mit `docker push`

Durch das Erstellen eines eigenen Docker-Images mit einem vorgegebenen ONNX-Modell konnte ich das Zusammenspiel von Machine Learning und Containerisierung besser nachvollziehen. Besonders hilfreich war die Erfahrung, wie ein Buildprozess in Docker funktioniert, angefangen bei der Dockerfile-Struktur über das lokale Testen bis hin zur Veröffentlichung auf Docker Hub.

Ich habe außerdem verstanden, warum es wichtig ist, Images wiederverwendbar zu machen – insbesondere durch klare Namensgebung, Dokumentation und öffentliches Hosting. Damit ist die Grundlage gelegt, um das Image im nächsten Schritt über Azure-Services zu deployen.

---

### Dokumentation Deployment Azure Web App


Nach erfolgreichem lokalem Aufbau und dem Push des ONNX-Docker-Images zu Docker Hub habe ich das Modell auf Azure als Web-App bereitgestellt. Verwendet wurde der Dienst **Azure Web App for Containers**, mit direkter Anbindung an das veröffentlichte Container-Image.

####  Vorgehen (Azure CLI)

Die Bereitstellung erfolgte über das Azure CLI. Zunächst wurde eine neue Web-App mit dem gewünschten Container-Image erstellt:

```bash
az webapp create \
  --resource-group mdm-appservice \
  --plan onnx-image-classification \
  --name onnx-image-app \
  --deployment-container-image-name sivanujan26/onnx-image-classification:latest
```
  Im Anschluss wurde die Container-Konfiguration zusätzlich über folgenden Befehl festgelegt:
```bash
  az webapp config container set \
  --name onnx-image-app \
  --resource-group mdm-appservice \
  --docker-custom-image-name sivanujan26/onnx-image-classification:latest
```
####  Einstellungen

- **Resource Group**: `mdm-appservice`  
- **App Name**: `onnx-image-app`  
- **Image**: `sivanujan26/onnx-image-classification:latest`  
- **Plan**: `onnx-image-classification` (Standard F1 Plan, Region: West Europe)  
- **Port**: Standardmäßig Port 80 (durch Azure-Web-Apps vorgegeben)  
- **URL**:  
  🔗 [https://onnx-image-app.azurewebsites.net](https://onnx-image-app.azurewebsites.net)


#### Screenshots

- **Abb. 12:** Übersicht der Ressourcengruppe im Azure Portal  
  ![Abb. 12 – Ressourcengruppe](images/azure-resourcegroup.png)

- **Abb. 13:** Terminalausgabe des erfolgreichen Deployments  
  ![Abb. 13 – Terminal Deployment](images/azure-terminal-output.png)

- **Abb. 14:** Konfiguration der App mit Docker-Image  
  ![Abb. 14 – Container Config](images/azure-container-config.png)

- **Abb. 15:** Live-Anwendung mit getesteter Bildklassifikation  
  ![Abb. 15 – Laufende App](images/azure-live-app.png)


#### Learnings & Herausforderungen

Beim ersten Versuch kam es zu einem bekannten Fehler beim Deployment (`exec format error`).  
Grund dafür war die **Plattform-Inkompatibilität** des ursprünglichen Docker-Images.

**Lösung:**  
Ein **Neu-Build** des Images mit dem Parameter `--platform linux/amd64` stellte die Kompatibilität mit Azure sicher:

```bash
docker buildx build --platform linux/amd64 -t sivanujan26/onnx-image-classification:latest --push .
```
Zudem mussten die **Container-Parameter** über  
```bash
`az webapp config container set` nachjustiert werden,  
da der ursprünglich genutzte Befehl `--deployment-container-image-name` **veraltet** ist.
```

---


## Reflexion

Durch die Umsetzung dieses Lernjournals konnte ich meine Kenntnisse im Umgang mit Containern deutlich vertiefen. Besonders hilfreich war die praktische Erfahrung mit Docker Compose, da ich dadurch gelernt habe, wie mehrere Services automatisiert und effizient miteinander verbunden werden können. 

Auch der Prozess, ein eigenes Docker-Image zu erstellen, zu testen und auf Docker Hub zu veröffentlichen, hat mir gezeigt, wie wichtig Versionierung und Wiederverwendbarkeit in der Container-Welt sind. Der Einsatz eines einfachen ONNX-Modells in einer Web-App war zudem eine gute Übung, um zu verstehen, wie Machine Learning in Container-Anwendungen integriert werden kann.

Insgesamt hat mir dieses Projekt geholfen, das Zusammenspiel von lokalem Deployment, Repository-Verwaltung und Vorbereitung auf Cloud-Deployments wie Azure besser zu verstehen. Ich fühle mich nun sicherer im Umgang mit Docker und Containerisierung im Allgemeinen.

---

## Quellenverzeichnis

- [Docker Dokumentation](https://docs.docker.com)
- [Azure CLI Dokumentation](https://learn.microsoft.com/en-us/cli/azure/)
- [ONNX Projekt](https://onnx.ai)
- [Docker Hub – onnx-image-classification (eigenes Image)](https://hub.docker.com/repository/docker/sivanujan26/onnx-image-classification/general)
- [GitHub Repository – onnx-image-classification (Fork)](https://github.com/sivanujan-selvarajah/onnx-image-classification)


---

## Abbildungsverzeichnis

| Nr.  | Beschreibung                                                             | Dateiname                             |
|------|--------------------------------------------------------------------------|----------------------------------------|
| 1    | Docker Pull für das Nextcloud-Image                                     | images/pull-nextcloud.png              |
| 2    | Startbefehl für MariaDB mit Umgebungsvariablen                          | images/run-mariadb.png                 |
| 3    | Erstellen eines Docker-Netzwerks und Verbinden beider Container         | images/docker-network-connect.png      |
| 4    | Web-basierte Einrichtung von Nextcloud                                  | images/nextcloud-setup-gui.png         |
| 5    | Übersicht laufender Container: Nextcloud & MariaDB                      | images/docker-containers-dashboard.png |
| 6    | Login-Bildschirm von Nextcloud nach erfolgreichem Setup                 | images/nextcloud-login.png             |
| 7    | Ausgabe von `docker ps` mit Containerstatus und Port-Mapping            | images/docker-ps.png                   |
| 8    | Inhalt der `docker-compose.yml` mit Definition der Services             | images/docker-compose.png              |
| 9    | Build-Prozess der ONNX Image Classification App mit Dockerfile          | images/onnx-docker-build.png           |
| 10   | Starten der ONNX-App lokal auf Port 5000                                | images/onnx-docker-run.png             |
| 10a  | Erstellung des öffentlichen Repositories auf Docker Hub                 | images/dockerhub-create-repo.png       |
| 11   | Upload des Docker-Images zu Docker Hub mit `docker push`               | images/docker-push-onnx.png            |
| 12   | Übersicht der Ressourcengruppe im Azure Portal                          | images/azure-resourcegroup.png         |
| 13   | Terminalausgabe des erfolgreichen Deployments                           | images/azure-terminal-output.png       |
| 14   | Konfiguration der App mit Docker-Image                                  | images/azure-container-config.png      |
| 15   | Laufende Web-App mit ONNX-Bildklassifikation auf Azure                  | images/azure-live-app.png              |