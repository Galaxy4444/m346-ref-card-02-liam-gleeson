# Architecture Ref.Card 02 - React Application (serverless)

Link zur Übersicht<br/>
https://gitlab.com/bbwrl/m346-ref-card-overview

## Installation der benötigten Werkzeuge

Für das Bauen der App wird Node bzw. npm benötigt. Die Tools sind unter
der folgenden URL zu finden. Für die meisten Benutzer:innen empfiehlt sich
die LTS Version.<br/>
https://nodejs.org/en/download/

Node Version Manager<br/>
Für erfahren Benutzer:innen empfiehlt sich die Installation des
Node Version Manager nvm. Dieses Tool erlaubt das Installiert und das
Wechseln der Node Version über die Kommandozeile.<br/>
**Achtung: Node darf noch nicht auf dem Computer installiert sein.**<br/>
https://learn2torials.com/a/how-to-install-nvm

## Inbetriebnahme auf eigenem Computer

Projekt herunterladen<br/>
```git clone git@gitlab.com:bbwrl/m346-ref-card-02.git```
<br/>
```cd architecture-refcard-02```

### Projekt bauen und starten

Die Ausführung der Befehle erfolgt im Projektordner

Builden mit Node/npm<br/>
```$ npm install```

Das Projekt wird gebaut und die entsprechenden Dateien unter dem Ordner node_modules gespeichert.

Die App kann nun mit folgendem Befehl gestartet werden<br/>
```$ npm start```

Die App kann nun im Browser unter der URL http://localhost:3000 betrachtet werden.

### Inbetriebnahme mit Docker Container

1. **Create a `Dockerfile`:**

   Dockerfile

   ```
   FROM node:18-alpine
   WORKDIR /app
   COPY package*.json ./
   RUN npm install
   COPY . .
   EXPOSE 3000
   CMD ["npm", "start"]

   ```


2. **Build and push the Docker image:**

   Bash

   ```
   docker build -t  galaxy444/m346-ref-card-liam-gleeson .
   docker push  galaxy444/m346-ref-card-liam-gleeson

   ```

### **Configure GitHub Actions:**

1. **Create a `.github/workflows/ci.yml` file:**

   YAML

   ```
   name: CI

   on:
     push:
       branches: [ main ]

   jobs:
     build:
       runs-on: ubuntu-latest

       steps:
       - uses: actions/checkout@v3
       - name: Build the Docker image
         run: docker build -t galaxy444/m346-ref-card-liam-gleeson .
       - name: Log in to Docker Hub
         run: docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD
       - name: Push the Docker image
         run: docker push galaxy444/m346-ref-card-liam-gleeson

   ```


2. **Add Docker Hub credentials as secrets:**

    - Create secrets in your GitHub repository settings.
    - Image of dockerhub: ![image](/images/dockerhub.png)

### **Conclusion:**

I made the pipeline build and then update it on my docker account, for this to work I needed a personal access token (
POT), after I got this i set the repo secret to that value
