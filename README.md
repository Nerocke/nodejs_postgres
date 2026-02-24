# 🛡️ DevSecure API (Node.js)

Une API Node.js (Express + PostgreSQL) conçue avec une approche DevSecOps : sécurisée dès le développement, containerisée et livrée via une pipeline CI/CD automatisée.

![CI](https://img.shields.io/badge/CI-GitHub%20Actions-blue)
![Docker](https://img.shields.io/badge/Docker-Ready-2496ED)
![Security](https://img.shields.io/badge/Security-Trivy%20%7C%20Snyk%20%7C%20Gitleaks-success)

## 🔧 Fonctionnalités

- ✅ API Node.js légère (Express)
- 🐳 Dockerisation complète
- ⚙️ Pipeline CI/CD GitHub Actions
	- Build de l'image Docker
	- Scan de sécurité:
		- Trivy (vulnérabilités image)
		- Snyk (dépendances & container)
		- Gitleaks (détection de secrets)
	- Push automatique vers Docker Hub (branche `dev`)
- 🧾 Génération de rapports HTML de vulnérabilités

## 🚀 Lancer en local

### 1. Cloner le projet

```sh
git clone https://github.com/Daniween/nodejs_postgres.git
cd nodejs_postgres
```

### 2. Construire et lancer l'API en Docker

```sh
docker build -t nodejs-postgres .
docker run -p 3000:3000 -e DATABASE_URL=postgres://postgres:mysecretpassword@host.docker.internal:5432/mydb nodejs-postgres
```

### 3. Tester l'API

```sh
curl http://localhost:3000/api/
```

## 🔐 Sécurité & Scans

| Outil | Objectif |
|-------|----------|
| Trivy | Scan des vulnérabilités dans l'image Docker |
| Snyk | Analyse des dépendances Node.js et du container |
| Gitleaks | Détection de secrets (clés, tokens...) dans le code |

## 📦 Déploiement Docker Hub

Une fois la branche `dev` poussée :

- L'image est construite automatiquement
- Elle est scannée avec Trivy, Snyk et Gitleaks
- Puis poussée en `latest` sur Docker Hub :

```sh
docker pull <ton_user>/nodejs-postgres:latest
```

## 🔑 Secrets GitHub nécessaires

- `DOCKER_USERNAME`
- `DOCKER_PASSWORD`
- `SNYK_TOKEN` (optionnel, recommandé)

## 📁 Structure du projet

```
├── server.js
├── Dockerfile
├── docker-compose.yml
├── package.json
├── .github/
│   └── workflows/
│       ├── cicd.yml
│       └── cicdmain.yml
└── app/
		├── controllers/
		├── models/
		└── routes/
```

## 🗺️ Architecture CI/CD

- `dev` : build + scans sécurité + rapports + push Docker Hub
- `main` : build & release Docker Hub (`latest` + SHA)
