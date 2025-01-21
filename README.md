# Structure NoSQL pour la Gestion de Stock Multi-Magasin

Ce document détaille la structure JSON proposée pour gérer un système de stock multi-magasin basé sur Firestore. Cette structure est conçue pour répondre aux besoins suivants :  
- Gestion des produits dans plusieurs magasins.  
- Suivi des quantités et des dates d’expiration des produits.  
- Gestion des réservations par client avant le retrait physique des produits.  

---

## 1. Structure Globale

La base de données est organisée en plusieurs collections et sous-collections principales :  
- **`stores` :** Contient les données relatives à chaque magasin.  
- **`products` :** Sous-collection de `stores`, gérant les informations sur les produits.  
- **`clients` :** Collection globale pour stocker les informations des clients.  
- **`reservations` :** Collection globale pour une vue d'ensemble des réservations.  

---

## 2. Détails des Collections et Champs

### **`stores` (Collection principale)**  
Chaque document dans la collection `stores` représente un magasin avec les informations suivantes :  
- **`name` :** Nom du magasin.  
- **`location` :** Adresse ou localisation du magasin.  
- **`products` :** Sous-collection pour gérer les produits spécifiques à ce magasin.

#### **Sous-collection `products`**  
Chaque produit est identifié par un `barcode` unique et contient :  
- **`name` :** Nom du produit.  
- **`quantity` :** Quantité actuelle en stock dans le magasin.  
- **`expiration_date` :** Date d’expiration du produit.  
- **`reservations` :** Sous-collection contenant les réservations liées au produit.  

#### **Sous-collection `reservations`**  
Chaque réservation liée à un produit est définie par :  
- **`client_id` :** Identifiant unique du client ayant effectué la réservation.  
- **`reserved_quantity` :** Quantité réservée par le client.  
- **`reservation_date` :** Date à laquelle la réservation a été effectuée.  
- **`status` :** État actuel de la réservation (`pending`, `confirmed`, etc.).

---

### **`clients` (Collection principale)**  
Cette collection stocke les informations des clients qui interagissent avec le système. Chaque document contient :  
- **`name` :** Nom du client.  
- **`contact` :** Coordonnées du client (par exemple, adresse email).  

---

### **`reservations` (Collection principale)**  
Cette collection offre une vue centralisée de toutes les réservations effectuées.  
Chaque document inclut :  
- **`store_id` :** Identifiant du magasin où la réservation a été faite.  
- **`barcode` :** Code-barres du produit réservé.  
- **`client_id` :** Identifiant du client.  
- **`reserved_quantity` :** Quantité réservée.  
- **`reservation_date` :** Date de la réservation.  
- **`status` :** État de la réservation.  

---

## 3. Exemple JSON

Voici un exemple concret de cette structure :

```json
{
  "stores": {
    "mapeau": {
      "name": "Mapeau",
      "location": "Adresse Magasin Mapeau 1",
      "products": {
        "barcode_1234567890": {
          "name": "Produit 1",
          "quantity": 50,
          "expiration_date": "2027-02-01",
          "reservations": {
            "reservation_1": {
              "client_id": "client_1",
              "reserved_quantity": 5,
              "reservation_date": "2025-01-21",
              "status": "pending"
            },
            "reservation_2": {
              "client_id": "client_2",
              "reserved_quantity": 3,
              "reservation_date": "2025-01-22",
              "status": "confirmed"
            }
          }
        },
        "barcode_0987654321": {
          "name": "Produit 2",
          "quantity": 30,
          "expiration_date": "2026-06-01",
          "reservations": {}
        }
      }
    }
  },
  "clients": {
    "client_1": {
      "name": "Client 1",
      "contact": "client1@example.com"
    },
    "client_2": {
      "name": "Client 2",
      "contact": "client2@example.com"
    }
  },
  "reservations": {
    "reservation_1": {
      "store_id": "mapeau",
      "barcode": "barcode_1234567890",
      "client_id": "client_1",
      "reserved_quantity": 5,
      "reservation_date": "2025-01-21",
      "status": "pending"
    },
    "reservation_2": {
      "store_id": "mapeau",
      "barcode": "barcode_1234567890",
      "client_id": "client_2",
      "reserved_quantity": 3,
      "reservation_date": "2025-01-22",
      "status": "confirmed"
    }
  }
}


## Contact

- **Email :** y.shimiyounes@gmail.com  
- **LinkedIn :** [linkedin.com/in/younes-shimi](https://www.linkedin.com/in/younes-shimi/)
