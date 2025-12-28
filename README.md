# TP - Microservices Spring Cloud (Eureka, Gateway, Services)

✅ **Description**

Ce TP illustre une architecture microservices avec un serveur Eureka (découverte), une passerelle (gateway) 

et deux services : **service-client** (gestion des clients) 
                  **service-voiture** (gestion des voitures). 
                  
Le service-voiture utilise Feign pour appeler le service-client.

---

## 📁 Structure du projet

- `eureka-server/` — Serveur Eureka (port 8761)
- `gateway/` — API Gateway (port 8888)
- `service-client/` — Service clients (port 8088)
- `service-voiture/` — Service voitures (port 8089)

Fichiers de configuration importants : `src/main/resources/application.properties` dans chaque module.

---

## 🛠️ Prérequis

- Java 11+ (ou version compatible avec Spring Boot du projet)
- Maven (ou utilisation du wrapper `mvnw` / `mvnw.cmd` fourni)

---

## ▶️ Démarrage 

1. Démarrer Eureka (nécessaire pour l'enregistrement des services) :

   - Windows :
     ```powershell
     cd eureka-server
     .\mvnw.cmd spring-boot:run
     ```
   - Unix/macOS :
     ```bash
     cd eureka-server
     ./mvnw spring-boot:run
     ```

2. Démarrer `service-client` (doit être disponible pour Feign) :

   - Windows :
     ```powershell
     cd ..\service-client
     .\mvnw.cmd spring-boot:run
     ```

3. Démarrer `service-voiture` :

   - Windows :
     ```powershell
     cd ..\service-voiture
     .\mvnw.cmd spring-boot:run
     ```

4. Optionnel : Démarrer la `gateway` si vous avez besoin de routage centralisé :

   - Windows :
     ```powershell
     cd ..\gateway
     .\mvnw.cmd spring-boot:run
     ```

> Remarque : les commandes ci-dessus utilisent le wrapper Maven fourni. Vous pouvez aussi construire chaque module avec `mvnw.cmd clean package` et exécuter le jar dans `target/`.

---

## 🔌 Ports et endpoints utiles

- Eureka : http://localhost:8761

- Gateway : http://localhost:8888

- service-client : http://localhost:8088

  - GET /clients — lister tous les clients
  - GET /client/{id} — récupérer un client par ID
  
- service-voiture : http://localhost:8089

  - GET /voitures — lister toutes les voitures
  - GET /voitures/{id} — récupérer une voiture (client injecté via Feign)
  
  - GET /voitures/client/{id} — voitures d'un client
  - POST /voitures/{clientId} — ajouter une voiture pour le client
  
  - PUT /voitures/{id} — mettre à jour une voiture

Exemples curl :

```bash
curl http://localhost:8088/clients
curl http://localhost:8089/voitures
curl -X POST -H "Content-Type: application/json" -d '{"marque":"Toyota","matricule":"X 123","model":"Corolla"}' http://localhost:8089/voitures/1
```

---

## ✅ Tests

Pour lancer les tests unitaires d'un module (Windows) :

```powershell
cd <module>
.\mvnw.cmd test
```

---

## Conseils et notes

- Démarrer **Eureka** avant les autres services pour qu'ils puissent s'enregistrer.

- Le service-voiture utilise Feign pour appeler 

`service-client`; assurez-vous que `service-client` 
est démarré lors de l'exécution du 
`CommandLineRunner` (il interroge des clients pour pré-remplir la base H2).

- Configuration des services : 

consultez `src/main/resources/application.properties` de chaque module pour modifier les ports ou l'URL Eureka.

---

---
