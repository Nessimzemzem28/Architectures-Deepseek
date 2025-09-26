# 🧠 DeepSeek - Architecture Microservices

## Introduction
DeepSeek est une plateforme d'intelligence artificielle basée sur une architecture microservices moderne. Cette architecture est conçue pour garantir **scalabilité**, **résilience**, et **maintenabilité**. Chaque service a une responsabilité claire, et la communication entre services utilise des méthodes synchrones et asynchrones, assurant une performance optimale et une gestion efficace des pics de charge.

## Auteurs
- Ahmed Takieddine Ghrib
- Mariem Belhadj
- Yesmine Zhioua
- Nessim Zemzem

## 📋 Table des Matières
- [Aperçu de l'Architecture](#aperçu-de-larchitecture)
- [Composants Principaux](#composants-principaux)
- [Infrastructure de Support](#infrastructure-de-support)
- [Workflow de Traitement](#workflow-de-traitement)
- [Patterns Architecturaux](#patterns-architecturaux)
- [Considérations de Performance](#considérations-de-performance)
- [Notes Techniques](#notes-techniques)

## 🏗️ Aperçu de l'Architecture

L'architecture microservices de DeepSeek repose sur une **décomposition modulaire**, où chaque service a une fonction précise. Cela permet de faciliter l'évolution, la maintenance et la montée en charge des services.

### Diagramme d'Architecture

![Architecture Microservices DeepSeek](./images/deepseek-architecture.png)

## 🔧 Composants Principaux

| Service | Responsabilité | Technologie |
|---------|----------------|------------|
| **API Gateway** | Point d'entrée unique, routage des requêtes | NGINX, Spring Cloud Gateway |
| **Service Auth** | Authentification JWT, sécurité | Node.js/Python, JWT, OAuth2 |
| **Service Utilisateurs** | Gestion des profils et préférences utilisateurs | Python/Go, REST APIs |
| **Service Chat/Core AI** | Traitement des conversations, NLP | Python, TensorFlow/PyTorch |
| **Service Inférence** | Exécution des modèles IA | Python, GPU acceleration |
| **Service Modèles** | Gestion des modèles de langage | Python, Model Registry |
| **Service Entraînement** | Fine-tuning et entraînement des modèles | Python, Distributed Training |

## 🏢 Infrastructure de Support

| Composant | Rôle | Technologie |
|-----------|------|------------|
| Load Balancers | Répartition de charge | HAProxy, NGINX |
| Message Queue | Communication asynchrone | Apache Kafka |
| Cache | Optimisation des performances | Redis |
| Monitoring | Collecte des métriques et logs | Prometheus, Grafana |
| Bases de Données | Persistance des données | PostgreSQL, MongoDB |

## 🔄 Workflow de Traitement

### Séquence Complète d'une Requête Chat

```mermaid
sequenceDiagram
    participant C as Client
    participant G as API Gateway
    participant L as Load Balancer
    participant A as Service Auth
    participant U as Service Utilisateur
    participant CS as Service Chat
    participant I as Service Inférence
    participant MQ as Message Queue
    participant D as Base de Données

    C->>G: POST /chat (token JWT)
    G->>L: Routage de la requête
    L->>A: Vérification du token
    A->>D: Validation du token
    D->>A: Token valide
    A->>U: Récupération du profil utilisateur
    U->>D: Requête des données utilisateur
    D->>U: Retour des données
    U->>CS: Transmission au service Chat
    CS->>D: Sauvegarde de la conversation
    D->>CS: Confirmation
    CS->>I: Traitement du message
    I->>MQ: Mise en queue pour traitement asynchrone
    MQ->>I: Traitement de la tâche
    I->>CS: Réponse IA
    CS->>D: Sauvegarde de la réponse
    D->>CS: Confirmation
    CS->>U: Retour de la réponse
    U->>A: Transmission
    A->>L: Transmission
    L->>G: Transmission
    G->>C: Réponse finale
```