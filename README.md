# MERN APP

## 1. Vue d'ensemble

Ce projet est une application full-stack MERN composée d'un client React, d'un serveur Node.js avec Express et d'une base de données MongoDB.

Docker est utilisé pour la conteneurisation des différents services, tandis que Docker Compose permet de les orchestrer.

Ce projet intègre également des pipelines CI permettant d'automatiser la construction des images Docker, leur analyse de sécurité avec Trivy et leur publication sur Docker Hub.

## 2. Technologies utilisées

- **Frontend :** React
- **Backend :** Node.js, Express
- **Base de données :** MongoDB
- **Conteneurisation :** Docker, Docker Compose
- **Intégration continue :** Jenkins, GitHub Actions
- **Analyse de sécurité :** Trivy
- **Registry :** Docker Hub

## 3. Structure du projet

```text
mern-app/
│
├── client/
│   └── Dockerfile
│
├── server/
│   └── Dockerfile
│
├── .github/
│   └── workflows/
│       └── docker-ci.yml
│
├── Jenkinsfile
├── docker-compose.yml
└── README.md
```

## 4. Variables d'environnement

Les principales variables utilisées dans l'application sont :

- **REACT_APP_API_URL :** URL de base du serveur API utilisée par le client React.
- **MONGO_URI :** URI de connexion à la base de données MongoDB.

Les identifiants Docker Hub utilisés dans les pipelines sont configurés sous forme de secrets ou de credentials. Ils ne doivent pas être enregistrés directement dans le dépôt.

## 5. Configuration Docker

Le projet contient deux Dockerfiles :

### Client

Le Dockerfile du client utilise Node.js pour installer les dépendances et construire l'application React.

### Serveur

Le Dockerfile du serveur utilise Node.js pour installer les dépendances et démarrer l'application Express.

Les images de base utilisées sont basées sur `node:lts-alpine`.

## 6. Docker Compose

Docker Compose permet de gérer les différents services de l'application :

- Client React.
- Serveur Node.js.
- Base de données MongoDB.

Il configure les ports, les variables d'environnement et la communication entre les services.

## 7. Exécution du projet

### Prérequis

- Docker installé.
- Docker Compose disponible.

### Lancement

1. Cloner le dépôt :

```bash
git clone https://github.com/OumaimaMd/mern-app.git
```

2. Accéder au projet :

```bash
cd mern-app
```

3. Construire et démarrer les services :

```bash
docker compose up --build
```

4. Accéder à l'application :

- Client : http://localhost:3000

## 8. Intégration continue (CI)

L'intégration continue permet d'automatiser la construction, l'analyse et la publication des images Docker à chaque modification du projet.

### 8.1 Jenkins

Un pipeline Jenkins est défini dans le fichier `Jenkinsfile`.

Les principales étapes sont :

1. **Checkout :** récupération du code source depuis GitHub.
2. **Build :** construction des images Docker du serveur et du client.
3. **Scan :** analyse des images avec Trivy afin de détecter les vulnérabilités.
4. **Push :** publication des images sur Docker Hub.
5. **Cleanup :** suppression des images locales après l'exécution du pipeline.

Les rapports Trivy sont conservés comme artefacts Jenkins.

### 8.2 GitHub Actions

Le workflow GitHub Actions est défini dans :

`.github/workflows/docker-ci.yml`

Il permet d'automatiser les opérations suivantes :

1. Détection des fichiers modifiés.
2. Construction de l'image du composant concerné.
3. Analyse de sécurité avec Trivy.
4. Conservation du rapport comme artefact.
5. Publication de l'image sur Docker Hub.

Le pipeline est conçu pour construire uniquement le composant modifié (`server` ou `client`).

## 9. Images Docker publiées

Les deux images Docker sont publiées sur Docker Hub :

- **Backend :** https://hub.docker.com/r/oumaimamdaini/mern-server
- **Frontend :** https://hub.docker.com/r/oumaimamdaini/mern-client

Les images sont identifiées par des tags correspondant aux versions de build Jenkins ou aux identifiants des commits GitHub Actions.

## 10. Sécurité avec Trivy

Trivy est utilisé pour analyser les images Docker et identifier les vulnérabilités.

Les rapports permettent de classer les vulnérabilités selon leur niveau de gravité :

- CRITICAL
- HIGH
- MEDIUM
- LOW

Les rapports sont conservés dans les artefacts des pipelines pour faciliter leur consultation.