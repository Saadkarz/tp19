# 🚀 TP19 - Microservices Architecture avec Spring Cloud

![Spring Boot](https://img.shields.io/badge/Spring%20Boot-3.5.8-brightgreen)
![Spring Cloud](https://img.shields.io/badge/Spring%20Cloud-2025.0.0-blue)
![Java](https://img.shields.io/badge/Java-17-orange)
![Netflix Eureka](https://img.shields.io/badge/Netflix-Eureka-red)
![OpenFeign](https://img.shields.io/badge/OpenFeign-4.3.0-yellow)

## 📋 Table des Matières

- [Vue d'ensemble](#-vue-densemble)
- [Architecture](#-architecture)
- [Technologies Utilisées](#-technologies-utilisées)
- [Structure du Projet](#-structure-du-projet)
- [Prérequis](#-prérequis)
- [Installation et Démarrage](#-installation-et-démarrage)
- [Démonstration avec Screenshots](#-démonstration-avec-screenshots)
- [Endpoints API](#-endpoints-api)
- [Configuration](#-configuration)
- [Fonctionnalités](#-fonctionnalités)
- [Auteur](#-auteur)

---

## 🎯 Vue d'ensemble

Ce projet implémente une **architecture microservices complète** utilisant Spring Cloud. Il démontre les concepts clés des microservices modernes :

- **Service Discovery** avec Netflix Eureka
- **API Gateway** avec Spring Cloud Gateway MVC
- **Communication inter-services** avec OpenFeign
- **Load Balancing** automatique
- **Base de données H2** en mémoire pour chaque service

L'application gère deux domaines métier : **Clients** et **Voitures**, avec une relation entre eux via communication REST.

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                         Client Browser                           │
└────────────────────────────┬────────────────────────────────────┘
                             │
                             ▼
                    ┌─────────────────┐
                    │   API Gateway   │
                    │   Port: 8888    │
                    └────────┬────────┘
                             │
              ┌──────────────┼──────────────┐
              │              │              │
              ▼              ▼              ▼
      ┌──────────────┐ ┌──────────────┐ ┌──────────────┐
      │    Eureka    │ │   SERVICE-   │ │   SERVICE-   │
      │    Server    │ │    CLIENT    │ │   VOITURE    │
      │  Port: 8761  │ │  Port: 8088  │ │  Port: 8089  │
      └──────────────┘ └──────┬───────┘ └───────┬──────┘
                              │                  │
                              │    OpenFeign     │
                              │  Communication   │
                              └──────────────────┘
                              │                  │
                              ▼                  ▼
                         ┌─────────┐       ┌─────────┐
                         │   H2    │       │   H2    │
                         │Database │       │Database │
                         └─────────┘       └─────────┘
```

### Composants

| Service | Port | Rôle | Technologies |
|---------|------|------|-------------|
| **Eureka Server** | 8761 | Service Registry & Discovery | Netflix Eureka Server |
| **SERVICE-CLIENT** | 8088 | Gestion des clients | Spring Data JPA, H2, Eureka Client |
| **SERVICE-VOITURE** | 8089 | Gestion des voitures | Spring Data JPA, H2, OpenFeign, Eureka Client |
| **Gateway** | 8888 | API Gateway / Reverse Proxy | Spring Cloud Gateway MVC |

---

## 🛠️ Technologies Utilisées

### Backend
- **Spring Boot 3.5.8** - Framework principal
- **Spring Cloud 2025.0.0** - Infrastructure microservices
- **Netflix Eureka** - Service discovery
- **Spring Cloud Gateway MVC** - API Gateway
- **Spring Cloud OpenFeign** - Client HTTP déclaratif
- **Spring Data JPA** - Couche de persistance
- **H2 Database** - Base de données en mémoire
- **Lombok** - Réduction du code boilerplate
- **Java 17** - Version Java

### Build & Tools
- **Maven 3.9+** - Gestion des dépendances
- **Spring Boot DevTools** - Rechargement automatique

---

## 📁 Structure du Projet

```
tp19/
├── eureka-server/              # Service de découverte Eureka
│   ├── src/main/java/com/example/demo/
│   │   └── EurekaServerApplication.java
│   ├── src/main/resources/
│   │   └── application.properties
│   └── pom.xml
│
├── service-client/             # Microservice de gestion des clients
│   ├── src/main/java/com/example/demo/
│   │   ├── DemoApplication.java
│   │   ├── entities/Client.java
│   │   ├── repositories/ClientRepository.java
│   │   └── controllers/ClientController.java
│   ├── src/main/resources/
│   │   └── application.properties
│   └── pom.xml
│
├── service-voiture/            # Microservice de gestion des voitures
│   ├── src/main/java/com/example/voiture/
│   │   ├── VoitureApplication.java
│   │   ├── entities/Voiture.java
│   │   ├── models/Client.java
│   │   ├── repositories/VoitureRepository.java
│   │   ├── services/VoitureService.java
│   │   └── controllers/VoitureController.java
│   ├── src/main/resources/
│   │   └── application.properties
│   └── pom.xml
│
├── gateway/                    # API Gateway
│   ├── src/main/java/com/example/demo/
│   │   └── DemoApplication.java
│   ├── src/main/resources/
│   │   ├── application.properties
│   │   └── application.yml
│   └── pom.xml
│
├── Screenshots/                # Captures d'écran de démonstration
├── GATEWAY_CONFIGURATION_GUIDE.md
├── start-all-services.ps1     # Script de démarrage
└── README.md
```

---

## ⚙️ Prérequis

Avant de commencer, assurez-vous d'avoir installé :

- ☑️ **Java Development Kit (JDK) 17** ou supérieur
- ☑️ **Maven 3.9+** (ou utilisez les wrappers mvnw inclus)
- ☑️ **Git** pour cloner le repository
- ☑️ Un navigateur web moderne
- ☑️ **PowerShell** (pour Windows) ou **Bash** (pour Linux/Mac)

### Vérification de l'installation

```powershell
# Vérifier Java
java -version

# Vérifier Maven (optionnel si vous utilisez mvnw)
mvn -version
```

---

## 🚀 Installation et Démarrage

### 1️⃣ Cloner le Repository

```bash
git clone https://github.com/Saadkarz/tp19.git
cd tp19
```

### 2️⃣ Démarrage des Services

**Important** : Les services doivent être démarrés dans l'ordre suivant pour assurer un fonctionnement correct.

#### Option A : Démarrage Manuel (Recommandé pour la première fois)

Ouvrez **4 terminaux PowerShell** séparés et exécutez les commandes suivantes :

**Terminal 1 - Eureka Server** (attendre qu'il démarre complètement avant de passer au suivant)
```powershell
$env:JAVA_HOME="C:\Program Files\Java\jdk-17"
cd eureka-server
.\mvnw spring-boot:run
```

**Terminal 2 - SERVICE-CLIENT**
```powershell
$env:JAVA_HOME="C:\Program Files\Java\jdk-17"
cd service-client
.\mvnw spring-boot:run
```

**Terminal 3 - SERVICE-VOITURE**
```powershell
$env:JAVA_HOME="C:\Program Files\Java\jdk-17"
cd service-voiture
.\mvnw spring-boot:run
```

**Terminal 4 - Gateway**
```powershell
$env:JAVA_HOME="C:\Program Files\Java\jdk-17"
cd gateway
.\mvnw spring-boot:run
```

#### Option B : Script Automatisé

```powershell
.\start-all-services.ps1
```

### 3️⃣ Vérification du Démarrage

Une fois tous les services démarrés, vérifiez :

✅ **Eureka Dashboard** : http://localhost:8761/  
✅ **SERVICE-CLIENT** : http://localhost:8088/clients  
✅ **SERVICE-VOITURE** : http://localhost:8089/voitures  
✅ **Gateway** : http://localhost:8888/clients  

---

## 📸 Démonstration avec Screenshots

### 1. Eureka Dashboard - Service Registry

![Eureka Dashboard](Screenshots/Screenshot%202025-12-03%20143017.png)

**Description** : Le tableau de bord Eureka montre tous les microservices enregistrés :
- ✅ **SERVICE-CLIENT** enregistré avec 1 instance sur le port 8088
- ✅ **SERVICE-VOITURE** enregistré avec 1 instance sur le port 8089
- ✅ **GATEWAY** enregistré (si discovery.enabled=true)

Eureka permet la **découverte automatique des services** et le **load balancing** entre instances multiples.

---

### 2. Liste des Clients (SERVICE-CLIENT)

![Liste Clients](Screenshots/Screenshot%202025-12-03%20143035.png)

**Endpoint** : `GET http://localhost:8088/clients`

**Description** : Ce screenshot montre la réponse JSON du service CLIENT avec 3 clients pré-initialisés dans la base H2 :

```json
[
  {
    "id": 1,
    "nom": "Rabab SELIMANI",
    "age": 23.0
  },
  {
    "id": 2,
    "nom": "Amal RAMI",
    "age": 22.0
  },
  {
    "id": 3,
    "nom": "Samir SAFI",
    "age": 22.0
  }
]
```

Les données sont initialisées automatiquement au démarrage via `CommandLineRunner` dans `DemoApplication.java`.

---

### 3. Détail d'un Client Spécifique

![Client Détail](Screenshots/Screenshot%202025-12-03%20143047.png)

**Endpoint** : `GET http://localhost:8088/client/1`

**Description** : Récupération des informations d'un client spécifique par son ID. Le service retourne :

```json
{
  "id": 1,
  "nom": "Rabab SELIMANI",
  "age": 23.0
}
```

Ce endpoint utilise `findById()` de Spring Data JPA avec gestion d'erreur via `ResponseEntity`.

---

### 4. Liste des Voitures avec Clients (OpenFeign en action)

![Liste Voitures](Screenshots/Screenshot%202025-12-03%20143108.png)

**Endpoint** : `GET http://localhost:8089/voitures`

**Description** : Ce screenshot démontre la **communication inter-services via OpenFeign**. Chaque voiture contient les informations complètes du client propriétaire :

```json
[
  {
    "id": 1,
    "marque": "Toyota",
    "matricule": "A 25 333",
    "model": "Corolla",
    "id_client": 1,
    "client": {
      "id": 1,
      "nom": "Rabab SELIMANI",
      "age": 23.0
    }
  },
  {
    "id": 2,
    "marque": "Renault",
    "matricule": "B 6 3456",
    "model": "Megane",
    "id_client": 1,
    "client": {
      "id": 1,
      "nom": "Rabab SELIMANI",
      "age": 23.0
    }
  },
  {
    "id": 3,
    "marque": "Peugeot",
    "matricule": "A 55 4444",
    "model": "301",
    "id_client": 2,
    "client": {
      "id": 2,
      "nom": "Amal RAMI",
      "age": 22.0
    }
  }
]
```

**💡 Point clé** : SERVICE-VOITURE appelle automatiquement SERVICE-CLIENT via Feign pour enrichir les données.

---

### 5. Voitures d'un Client Spécifique

![Voitures par Client](Screenshots/Screenshot%202025-12-03%20143118.png)

**Endpoint** : `GET http://localhost:8089/voitures/client/1`

**Description** : Filtrage des voitures appartenant au client avec l'ID 1 (Rabab SELIMANI). Cette requête utilise une **requête JPQL personnalisée** :

```java
@Query("SELECT v FROM Voiture v WHERE v.id_client = :clientId")
List<Voiture> findByIdClient(@Param("clientId") Long clientId);
```

Résultat : 2 voitures (Toyota Corolla et Renault Megane) appartiennent à Rabab SELIMANI.

---

### 6. Accès via Gateway (Routage API)

![Gateway Routing](Screenshots/Screenshot%202025-12-03%20143129.png)

**Endpoint** : `GET http://localhost:8888/voitures`

**Description** : Le **Gateway** agit comme point d'entrée unique. Les requêtes vers `/voitures` sont automatiquement **routées** vers SERVICE-VOITURE (port 8089).

**Configuration du routing** (dans `application.yml`) :

```yaml
spring:
  cloud:
    gateway:
      mvc:
        routes:
          - id: r1
            uri: http://localhost:8088
            predicates:
              - Path=/clients/**
          - id: r2
            uri: http://localhost:8088
            predicates:
              - Path=/client/**
          - id: r3
            uri: http://localhost:8089
            predicates:
              - Path=/voitures/**
```

**Avantages du Gateway** :
- ✅ Point d'entrée unique pour tous les services
- ✅ Sécurité centralisée
- ✅ Rate limiting et throttling possibles
- ✅ Load balancing automatique

---

## 🔗 Endpoints API

### 📋 SERVICE-CLIENT (Port 8088)

| Méthode | Endpoint | Description | Exemple de Réponse |
|---------|----------|-------------|-------------------|
| `GET` | `/clients` | Récupérer tous les clients | Liste JSON de clients |
| `GET` | `/client/{id}` | Récupérer un client par ID | Client JSON ou 404 |

### 🚗 SERVICE-VOITURE (Port 8089)

| Méthode | Endpoint | Description | Détails |
|---------|----------|-------------|---------|
| `GET` | `/voitures` | Récupérer toutes les voitures | Avec client enrichi via Feign |
| `GET` | `/voitures/{id}` | Récupérer une voiture par ID | Voiture avec détails client |
| `GET` | `/voitures/client/{id}` | Voitures d'un client | Filtre par id_client |
| `POST` | `/voitures/{clientId}` | Créer une nouvelle voiture | Body: `{"marque":"...","matricule":"...","model":"..."}` |
| `PUT` | `/voitures/{id}` | Mettre à jour une voiture | Body: `{"matricule":"...","marque":"...","model":"..."}` |

### 🌐 Via Gateway (Port 8888)

Tous les endpoints ci-dessus sont accessibles via le Gateway en remplaçant les ports par `8888` :

```
http://localhost:8888/clients
http://localhost:8888/client/1
http://localhost:8888/voitures
http://localhost:8888/voitures/1
http://localhost:8888/voitures/client/1
```

---

## ⚙️ Configuration

### Eureka Server Configuration

```properties
# eureka-server/src/main/resources/application.properties
server.port=8761
eureka.client.register-with-eureka=false
eureka.client.fetch-registry=false
```

### SERVICE-CLIENT Configuration

```properties
# service-client/src/main/resources/application.properties
server.port=8088
spring.application.name=SERVICE-CLIENT
eureka.client.service-url.defaultZone=http://localhost:8761/eureka/
spring.datasource.url=jdbc:h2:mem:testdb
spring.h2.console.enabled=true
```

### SERVICE-VOITURE Configuration

```properties
# service-voiture/src/main/resources/application.properties
server.port=8089
spring.application.name=SERVICE-VOITURE
eureka.client.service-url.defaultZone=http://localhost:8761/eureka/
spring.datasource.url=jdbc:h2:mem:testdb
spring.h2.console.enabled=true
```

### Gateway Configuration

```yaml
# gateway/src/main/resources/application.yml
spring:
  application:
    name: Gateway
  cloud:
    discovery:
      enabled: false  # Static routing mode
    gateway:
      mvc:
        routes:
          - id: r1
            uri: http://localhost:8088
            predicates:
              - Path=/clients/**
          - id: r2
            uri: http://localhost:8088
            predicates:
              - Path=/client/**
          - id: r3
            uri: http://localhost:8089
            predicates:
              - Path=/voitures/**
```

---

## ✨ Fonctionnalités

### 🔹 Service Discovery avec Eureka
- Enregistrement automatique des services
- Health check et heartbeat
- Load balancing client-side
- Découverte dynamique des services

### 🔹 Communication Inter-Services avec OpenFeign
- Clients HTTP déclaratifs
- Sérialisation/Désérialisation automatique
- Gestion des erreurs et retry
- Load balancing intégré

### 🔹 API Gateway
- Point d'entrée unique
- Routage dynamique basé sur les chemins
- Possibilité d'ajouter des filtres (authentification, logging, etc.)
- Support du routage via Eureka (mode dynamique)

### 🔹 Gestion des Données
- H2 Database en mémoire pour dev/test
- Spring Data JPA avec repositories
- Initialisation automatique des données de test
- Console H2 accessible pour debug

### 🔹 Architecture RESTful
- Endpoints REST standardisés
- Codes HTTP appropriés (200, 404, 500, etc.)
- Gestion des erreurs avec ResponseEntity
- JSON comme format d'échange

---

## 🧪 Tests

### Test Manuel avec Navigateur

1. Démarrer tous les services
2. Ouvrir http://localhost:8761/ pour vérifier l'enregistrement
3. Tester les endpoints listés dans la section [Endpoints API](#-endpoints-api)

### Test avec Postman

Importer la collection Postman (à créer) avec tous les endpoints configurés.

### Test avec cURL

```bash
# Récupérer tous les clients
curl http://localhost:8088/clients

# Récupérer toutes les voitures via Gateway
curl http://localhost:8888/voitures

# Créer une nouvelle voiture
curl -X POST http://localhost:8089/voitures/1 \
  -H "Content-Type: application/json" \
  -d '{"marque":"BMW","matricule":"C 123 456","model":"X5"}'
```

---

## 🐛 Dépannage

### Les services ne démarrent pas

**Problème** : Port déjà utilisé  
**Solution** : 
```powershell
# Vérifier les ports utilisés
netstat -ano | findstr "8761"
netstat -ano | findstr "8088"
netstat -ano | findstr "8089"
netstat -ano | findstr "8888"

# Tuer le processus si nécessaire
Stop-Process -Id <PID> -Force
```

### Erreur "release version 17 not supported"

**Solution** : Définir JAVA_HOME avant de lancer Maven
```powershell
$env:JAVA_HOME="C:\Program Files\Java\jdk-17"
```

### SERVICE-VOITURE ne trouve pas SERVICE-CLIENT

**Solution** : Vérifier que :
1. Eureka Server est démarré
2. SERVICE-CLIENT est enregistré dans Eureka
3. Attendre 30 secondes pour la synchronisation

---

## 📚 Ressources

- [Spring Cloud Documentation](https://spring.io/projects/spring-cloud)
- [Netflix Eureka](https://github.com/Netflix/eureka)
- [Spring Cloud Gateway](https://spring.io/projects/spring-cloud-gateway)
- [OpenFeign](https://github.com/OpenFeign/feign)

---

## 👨‍💻 Auteur

**Saad Karzouz**

- GitHub: [@Saadkarz](https://github.com/Saadkarz)
- Repository: [tp19](https://github.com/Saadkarz/tp19)

---


<div align="center">

**⭐ Si vous trouvez ce projet utile, n'hésitez pas à lui donner une étoile sur GitHub ! ⭐**

Made with ❤️ using Spring Boot & Spring Cloud

</div>
