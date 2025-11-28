# Docker nginx Webservice

Dieses Projekt zeigt einen einfachen Webservice, der in einem Docker-Container läuft.  
Der Container verwendet **nginx** als Webserver und liefert eine statische HTML-Seite aus.

Das Projekt ist Teil meines Portfolios im Bereich **Systemintegration** und **Cloud-Engineering**.

---

## Zweck des Projekts

Ich wollte die Grundbausteine verstehen, die im IT-Betrieb und in Cloud-Umgebungen regelmäßig gebraucht werden:

- Wie man einen Dienst als Container bereitstellt  
- Wie Webserver wie nginx funktionieren  
- Wie Ports zwischen Host und Container zugeordnet werden  
- Wie man Fehler über Logs findet  
- Wie man reproduzierbare Umgebungen baut (Dockerfile)  

---

## Aufbau des Webservices

Der Container enthält:

- **nginx** als Webserver  
- eine eigene Konfiguration (`nginx.conf`)  
- eine statische HTML-Seite (`index.html`)  

nginx läuft im Container auf Port **80**.  
Auf dem Host ist der Service über einen frei gewählten Port erreichbar (z. B. 8080).

---

## Wichtige Dateien

| Datei              | Bedeutung |
|-------------------|-----------|
| `Dockerfile`       | Erstellt das Image. Kopiert HTML und Konfiguration in den Container. |
| `nginx.conf`       | Eigene Webserver-Konfiguration (Port, Root-Verzeichnis, Routing). |
| `html/index.html`  | Die Seite, die nginx ausliefert. |

---

# Container bauen und starten

### Image bauen

```
docker build -t docker-nginx-webservice .
```

### Container starten
```
docker run -d -p 8080:80 --name webservice docker-nginx-webservice
```

### Der Dienst ist erreichbar unter:
```
http://localhost:8080
```
# Port-Konflikte und Fehlerbehebung
Wenn der Dienst nicht erreichbar ist, gehe ich so vor:

### 1. Läuft der Container?
``` 
docker ps -a
```

### 2. Logs prüfen
``` 
docker logs webservice
 ```

### 3. Host-Port belegt?

Container mit anderem Port starten:

``` 
docker run -d -p 8081:80 --name webservice docker-nginx-webservice
 ```

### 4. Alten Container löschen (falls nötig)
```
docker stop webservice
docker rm webservice 
```
