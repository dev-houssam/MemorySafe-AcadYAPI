# MemorySafe-AcadYAPI

## MemorySafe — Academic Year API

> **Projet de systèmes d'information en architecture microservices — Master 1 ILIADE, UBO — 2024/2025**

**MemorySafe** est un projet réalisé en groupe dans le cadre de l'UE **Systèmes d'Information** du Master 1 Informatique ILIADE à l'Université de Bretagne Occidentale.

Le projet consiste en la conception d'un **gestionnaire de formation basé sur une architecture microservices**, avec plusieurs services spécialisés ainsi qu'une interface Web permettant d'interagir avec le système.

Ce dépôt correspond au microservice **Academic Year**, consacré à la gestion des années de formation, des groupes, des unités d'enseignement et des inscriptions étudiantes.

🔗 **Dépôt du projet MemorySafe :**
[GitLab — MemorySafe](https://gitlab.com/hbacar/memorysafe/-/tree/main?ref_type=heads)

---

## 🧩 Architecture du projet

MemorySafe repose sur une architecture **microservices**, chaque service étant chargé d'un domaine fonctionnel spécifique.

Le système comprend notamment :

* une **API Academic Year** pour la gestion des formations ;
* une **API d'authentification** ;
* une **API de messagerie** ;
* une **application frontend** permettant aux utilisateurs d'interagir avec les différents services.

Schématiquement :

```text
                    ┌──────────────────────┐
                    │      Frontend        │
                    │     Vue.js / Vite    │
                    └──────────┬───────────┘
                               │
              ┌────────────────┼────────────────┐
              │                │                │
              ▼                ▼                ▼
       ┌─────────────┐  ┌─────────────┐  ┌─────────────┐
       │ Academic    │  │ Authentif.   │  │ Messagerie  │
       │ Year API    │  │ API          │  │ API         │
       └──────┬──────┘  └─────────────┘  └─────────────┘
              │
              ▼
       ┌─────────────┐
       │   MariaDB   │
       └─────────────┘
```

L'architecture permet de séparer les responsabilités fonctionnelles entre plusieurs services indépendants.

---

# 🎓 Academic Year API

L'API **Academic Year** est le microservice dédié à la gestion des données liées aux formations universitaires.

Elle permet notamment de gérer :

* les années/formations ;
* les groupes ;
* les étudiants ;
* les unités d'enseignement ;
* les demandes d'inscription ;
* les inscriptions aux formations et aux UE.

L'API expose ces fonctionnalités sous la forme d'une **API REST développée avec Spring Boot**.

---

## 🚀 Fonctionnalités

### Formations

```text
GET    /academicyears
POST   /academicyears
PUT    /academicyears/{id}
DELETE /academicyears/{id}
```

Gestion des formations et consultation des étudiants associés :

```text
GET  /academicyears/{id}/students

POST /academicyears/{id}/register/{studentId}
POST /academicyears/{id}/accept/{studentId}
POST /academicyears/{id}/reject/{studentId}
```

### Groupes

```text
GET    /groups
POST   /groups
PUT    /groups/{id}
DELETE /groups/{id}
```

### Unités d'enseignement

```text
GET    /teachingunits
POST   /teachingunits
PUT    /teachingunits/{id}
DELETE /teachingunits/{id}
```

Gestion des inscriptions aux UE :

```text
POST /teachingunits/{id}/register/{studentId}
POST /teachingunits/{id}/unregister/{studentId}
```

### Demandes d'inscription

```text
GET /requests
```

---

# 🏗️ Architecture du microservice

Le code du microservice est organisé en plusieurs couches :

```text
src/main/java/com/

├── controllers/
│   ├── AcademicYearController
│   ├── GroupController
│   ├── RequestController
│   ├── StudentController
│   └── TeachingUnitController
│
├── dtos/
│   ├── AcademicYearDto
│   ├── GroupDto
│   ├── RequestDto
│   ├── StudentDto
│   └── TeachingUnitDto
│
├── entities/
│   ├── AcademicYear
│   ├── Group
│   ├── Request
│   ├── Student
│   ├── TeachingUnit
│   └── User
│
├── mappers/
│
├── repositories/
│
├── services/
│   └── impl/
│
└── exceptions/
    ├── GlobalExceptionHandler
    ├── ResourceNotFoundException
    └── TeachingUnitNotFoundException
```

Cette organisation sépare notamment :

* les **contrôleurs REST** ;
* les **DTOs** ;
* les **entités métier** ;
* les **services** ;
* les **repositories** ;
* les **mappers** ;
* la **gestion des exceptions**.

---

## 🔄 Flux général

Une requête provenant du frontend suit le principe :

```text
Vue.js
   │
   │ HTTP
   ▼
Controller
   │
   ▼
Service
   │
   ▼
Repository
   │
   ▼
MariaDB
```

La réponse remonte ensuite vers le frontend sous forme de données exposées par l'API REST.

---

# 🖥️ Frontend

Le projet MemorySafe comprend également une **interface Web frontend** permettant d'utiliser les différents services du système.

Technologies utilisées notamment :

* **Vue.js**
* **Vite**
* JavaScript

Le frontend communique avec les APIs du système afin de fournir une interface utilisateur aux fonctionnalités de gestion de formation.

> Le frontend fait partie du projet MemorySafe global et ne constitue pas le périmètre principal de ce dépôt Academic Year.

---

# 🗄️ Base de données

Le microservice Academic Year utilise **MariaDB** pour la persistance des données.

La base peut être exécutée dans un conteneur Docker afin de faciliter l'environnement de développement.

```bash
docker run --name mariadb \
  -e MYSQL_ROOT_PASSWORD=motdepasse++!!UltraRobuste%%%%%Absolument-Robuste \
  -e MYSQL_DATABASE=projet_api \
  -p 3306:3306 \
  -d mariadb
```

---

# 📖 Documentation de l'API

L'API est documentée avec **Swagger / OpenAPI 3.1**.

La documentation est générée à partir de la spécification OpenAPI et permet notamment de consulter les endpoints disponibles et d'effectuer des tests depuis Swagger UI.

```text
/swagger-ui.html
```

Une spécification OpenAPI est également présente dans les ressources du projet :

```text
src/main/resources/openapi.yaml
```

---

# 🧪 Tests

Les endpoints de l'API ont été **testés manuellement avec Postman en environnement local** pendant le développement.

La documentation Swagger permet également de tester les différentes routes exposées par le microservice.

---

# 🐳 Environnement de développement

Le projet utilise notamment :

* **Gradle** pour la construction du projet Java ;
* **Docker** pour la base de données ;
* **MariaDB** pour la persistance ;
* **Postman** pour les tests ;
* **Swagger / OpenAPI** pour la documentation de l'API.

---

## 🛠️ Technologies

### Backend

* Java 17
* Spring Boot
* Spring REST
* Gradle

### Frontend — projet global

* Vue.js
* Vite

### Données et infrastructure

* MariaDB
* SQL
* Docker

### API et tests

* REST
* OpenAPI 3.1
* Swagger
* Postman

### Organisation du projet

* Architecture microservices
* DTO
* Services / Repositories
* Mappers
* Gestion centralisée des exceptions

---

# 👥 Travail en groupe

**MemorySafe a été réalisé en groupe** dans le cadre du Master 1 ILIADE.

Le projet nécessitait notamment de travailler avec une architecture distribuée composée de plusieurs microservices et d'une interface frontend commune.

Le dépôt global permet de retrouver l'ensemble du projet :

[Projet MemorySafe sur GitLab](https://gitlab.com/hbacar/memorysafe/-/tree/main?ref_type=heads)

---

# 🎓 Contexte académique

**Master 1 Informatique — Parcours ILIADE**

**Université de Bretagne Occidentale (UBO)**

**UE : Systèmes d'Information**

**Année universitaire : 2024/2025**

**Projet de groupe : MemorySafe**

**Microservice : Academic Year**

## LICENCE MIT
