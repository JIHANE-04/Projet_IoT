# 🌡️ Système IoT de Surveillance de la Chaîne du Froid

![Python](https://img.shields.io/badge/Python-3.x-blue?logo=python)
![Django](https://img.shields.io/badge/Django-Framework-green?logo=django)
![ESP8266](https://img.shields.io/badge/Hardware-ESP8266-orange?logo=espressif)
![DHT](https://img.shields.io/badge/Capteur-DHT11%2F22-red)
![MQTT](https://img.shields.io/badge/Protocol-MQTT-purple)
![SQLite](https://img.shields.io/badge/Database-SQLite-lightgrey?logo=sqlite)
![IoT](https://img.shields.io/badge/IoT-Embarqué-blueviolet)

> Système IoT embarqué de surveillance en temps réel de la température et de l'humidité dans un laboratoire d'analyses médicales — avec détection automatique d'anomalies, gestion des incidents et dashboard web sécurisé.

---

## 📌 Contexte & Problématique

Ce projet a été réalisé dans le cadre d'un **laboratoire d'analyses médicales** disposant de réfrigérateurs contenant des **échantillons biologiques et chimiques** très sensibles aux variations de température.

Ces échantillons nécessitent une conservation stricte entre **2°C et 8°C**. Toute rupture de la chaîne du froid peut :
- ❌ Fausser les résultats d'analyses
- 💸 Entraîner des pertes financières importantes
- ⚠️ Mettre en danger la qualité des soins

La **surveillance manuelle** présente des limites critiques : elle n'est pas continue et dépend de l'humain — que se passe-t-il le week-end ou la nuit ?

**Notre solution** : un système IoT automatisé, disponible 24h/24, capable de détecter et signaler instantanément toute anomalie.

---

## ✨ Fonctionnalités

| Fonctionnalité | Description |
|---|---|
| 📡 **Acquisition en temps réel** | Lecture DHT via ESP8266, envoi MQTT → API REST Django |
| 🌡️ **Dashboard web** | Visualisation live de la température et humidité |
| 🚨 **Détection d'anomalies** | Alerte automatique si T° < 2°C ou T° > 8°C |
| 📋 **Gestion des incidents** | Création automatique d'incidents avec niveau d'alerte |
| 🗃️ **Historique complet** | Traçabilité totale des mesures (SQLite) |
| 🔐 **Accès sécurisé** | Authentification multi-utilisateurs (opérateurs + admin) |

---

## 🏗️ Architecture du système

```
[ESP8266 + DHT]  →  MQTT Publish  →  [Broker MQTT]  →  MQTT Subscribe  →  [Django Backend]
                                                                                    │
                                                                          ┌─────────┴─────────┐
                                                                          │                   │
                                                                     [SQLite DB]      [Vérification seuils]
                                                                          │                   │
                                                                    [Dashboard Web]    [Création Incident]
                                                                    API REST (GET)     + Alerte
```

---

## 🗂️ Structure du projet

```
📦 cold-chain-iot/
├── 📁 Code/              # Code embarqué ESP8266 (C++ / Arduino Framework)
│   └── main.ino          # Lecture DHT → Formatage JSON → Envoi MQTT
├── 📁 DHT/               # Librairies capteur DHT
├── 📁 incident/          # App Django : détection & gestion des incidents
├── 📁 projet/            # Configuration principale Django
├── 📁 static/js/         # Scripts JavaScript (dashboard temps réel)
├── 📁 templates/         # Templates HTML (interface web)
├── 📄 manage.py          # Point d'entrée Django
├── 📄 requirements.txt   # Dépendances Python
└── 📄 db.sqlite3         # Base de données SQLite
```

---

## 🗃️ Structure des données

### Table `Mesure`
| Champ | Type | Description |
|-------|------|-------------|
| `sensor_id` | String | Identifiant du capteur |
| `temperature` | Float | Température mesurée (°C) |
| `humidity` | Float | Humidité relative (%) |
| `timestamp` | DateTime | Horodatage de la mesure |

### Table `Incident`
| Champ | Type | Description |
|-------|------|-------------|
| `sensor_id` | String | Capteur ayant déclenché l'alerte |
| `valeur_mesurée` | Float | Valeur hors seuil |
| `date` | DateTime | Date de l'incident |
| `statut` | String | Ouvert / Résolu |
| `niveau_alerte` | String | Bas / Moyen / Critique |

> 🚨 Un incident est créé automatiquement si T° < 2°C ou T° > 8°C

---

## 🔧 Technologies utilisées

### Hardware
- **ESP8266** (NodeMCU) — microcontrôleur WiFi
- **Capteur DHT11 / DHT22** — température & humidité

### Protocole de communication
- **MQTT** — protocole léger et rapide pour l'IoT
- **API REST** — communication ESP8266 ↔ serveur Django

### Software
- **C++ (Arduino Framework)** — programmation embarquée ESP8266
- **Python 3.x + Django** — backend, API REST, gestion BDD
- **SQLite** — stockage et traçabilité des données
- **HTML / CSS / JavaScript** — interface web dashboard

### Conception
- **SysML** — Diagramme d'exigences + Diagramme de cas d'utilisation

---

## 🚀 Installation & Lancement

### Prérequis
- Python 3.x installé
- Arduino IDE (pour flasher l'ESP8266)
- Broker MQTT (ex: Mosquitto en local ou broker cloud)

### 1. Cloner le dépôt
```bash
git clone https://github.com/ton-compte/cold-chain-iot.git
cd cold-chain-iot
```

### 2. Installer les dépendances Python
```bash
pip install -r requirements.txt
```

### 3. Appliquer les migrations
```bash
python manage.py migrate
```

### 4. Lancer le serveur Django
```bash
python manage.py runserver
```

### 5. Flasher l'ESP8266
- Ouvre `Code/main.ino` avec **Arduino IDE**
- Configure l'IP du broker MQTT et du serveur Django dans le code
- Téléverse sur l'ESP8266

### 6. Accéder au dashboard
Ouvre ton navigateur sur : `http://127.0.0.1:8000`

---

## 🎓 Contexte académique

| | |
|---|---|
| **Établissement** | École Nationale des Sciences Appliquées d'Oujda (ENSAO) |
| **Filière** | GSEIR-4 (Génie des Systèmes Électroniques, Informatiques et Réseaux) |
| **Encadrant** | Mr. Moussati Ali |
| **Année** | 2024 – 2025 |

---

## 👩‍💻 Auteures

**Jihane Bouras**  
🔗 [LinkedIn](https://linkedin.com/in/jihane-bouras-74896427a) • 📧 jihane.brs123@gmail.com

**El Azimani Chaimae**

---

## 🔮 Perspectives d'évolution

- 📲 Alertes par **SMS** pour notification immédiate des techniciens
- 📱 **Application mobile** pour accès nomade
- 🔋 **Batterie de secours** pour continuité en cas de coupure
- ☁️ Déploiement **Cloud** pour accès à distance

---

## 📄 Licence

Ce projet est réalisé dans un cadre académique — ENSAO 2024/2025.
