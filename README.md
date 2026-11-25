# 🏠 Ma Domotique ESPHome

![ESPHome Validation](https://github.com/dguihal/esphome-conf/actions/workflows/esphome-validation.yml/badge.svg)

Ce dépôt centralise toutes les configurations de mes modules ESPHome.

## Dépendances

Renovate surveille les composants déclarés avec `external_components` et ouvre
une pull request lorsqu'un nouveau commit apparaît sur la branche déclarée
du dépôt concerné. Les références `github://` restent épinglées sur un SHA :
aucune configuration ne suit `main` directement. Ces mises à jour ne sont pas
automatiquement fusionnées et sont validées par le workflow ESPHome avant une
revue manuelle. La branche suivie est indiquée par le commentaire
`# renovate: branch=...` qui précède chaque composant.

## 🛠 Structure du Repo

- `/common/` : Fragments de code partagés (Wi-Fi, API, OTA).
- `/fonts/` : Fichiers de police TrueType (.ttf) et icônes (MDI).
- `*.yaml` : Fichiers de configuration par appareil.
- `secrets.yaml.example` : Modèle pour les données sensibles (le vrai fichier est ignoré par Git).

---

## 🏗 Projets Actuels

### 1. Volet Somfy RTS + Ventilateur plafonnier Mantra

Fichier: [bt-proxy-rts.yaml](bt-proxy-rts.yaml)

**Matériel :**

- Carte : `ESP32-C6-LCD-1.47` [Waveshare Wiki](https://www.waveshare.com/wiki/ESP32-C6-LCD-1.47) / [Schématique](docs/ESP32-C6-LCD-1.47_schematics.pdf)
- Radio : `CC1101` (433MHz) [Doc produit](docs/cc1101.md)
- Composants:
  - [leonardpitzu/somfy_cover](https://github.com/leonardpitzu/somfy_cover)
  - [NicoIIT/esphome-ble_adv_proxy](https://github.com/NicoIIT/esphome-ble_adv_proxy)

**Câblage (SPI) :**

| CC1101 Pin | ESP32-C6 GPIO | Fonction |
| :--- | :--- | :--- |
| **VCC** | 3.3V | Alimentation |
| **GND** | GND | Masse |
| **SCK** | GPIO 18 | SPI Clock |
| **MOSI** | GPIO 19 | SPI MOSI |
| **MISO** | GPIO 20 | SPI MISO |
| **CSN** | GPIO 2 | SPI Chip Select |
| **GDO0** | GPIO 5 | Data Transmit |
| **GDO2** | GPIO 23 | Data Receive |


### 2. Watermeter

Fichier: [watermeter.yaml](watermeter.yaml)

**Matériel :**

- Carte : `AZ Delivery esp-32-dev-kit-c-v4` [AZ Delivery](https://www.az-delivery.de/fr/products/esp-32-dev-kit-c-v4)
- Capteur de débit d'eau liquide Effet Hall [Doc produit](docs/debitmetre.md)


**Câblage :**

| Capteur débit | ESP32 GPIO | Fonction |
| :--- | :--- | :--- |
| Fil Rouge | 5V | Alimentation |
| Fil Noir | GND | Masse |
| Fil Jaune | GPIO 26 | Impulsions de comptage |

### 3. Environmental Sensor (Multi-Sensor + LCD)

Fichier: [environmental-sensor.yaml](environmental-sensor.yaml)

Station de monitoring de la qualité de l'air avec affichage cyclique sur écran TFT et indicateur visuel LED.

**Matériel :**

- Carte : `ESP32-C6-WROOM-1-N8` (8MB Flash)
- Écran : `LCD TFT 2" 240x320` (Contrôleur ST7789)
- Capteurs :
  - SCD41 : CO2 (NDIR véritable), Température, Humidité. [Doc produit](docs/scd41.md)
  - ENS160 : AQI, COV, eCO2. [Doc produit](docs/ens160.md)
  - BME680 : Pression atmosphérique, Gaz, Température, Humidité. [Doc produit](docs/bme680.md)
  - AHT21 : Température et Humidité de précision. [Doc produit](docs/aht21.md)
  - HC-SR501 : Détecteur de mouvement (PIR) pour gestion du rétroéclairage. [Doc produit](docs/hc-sr501.md)
  - LD2420 : Radar de présence mmWave (détection micro-mouvements). [Doc produit](docs/ld2420.md)

**Câblage :**

| Composant | Pin ESP32-C6 | Fonction |
| :--- | :--- | :--- |
| **Bus I2C** | GPIO 2 (SDA), GPIO 1 (SCL) | Tous les capteurs |
| **Bus SPI (LCD)** | GPIO 7 (CLK), GPIO 6 (MOSI) | Affichage |
| **LCD CS** | GPIO 14 | Chip Select |
| **LCD DC** | GPIO 3 | Data/Command |
| **LCD RST** | GPIO 18 | Reset |
| **LCD BL** | GPIO 22 | Backlight (PWM) |
| **LD2420 UART** | GPIO 20 (TX), GPIO 21 (RX) | Communication Radar |
| **Status LED** | GPIO 10 | LED RGB WS2812 |
| **PIR Sensor** | GPIO 0 | Détection de présence |

**Fonctionnalités :**

- Interface graphique **LVGL** avec rotation logicielle.
- Cycle d'affichage automatique des capteurs toutes les 5 secondes.
- Ajustement dynamique des couleurs (Vert -> Rouge) selon les seuils de CO2/COV.
- Gestion d'énergie : l'écran s'assombrit après X secondes sans mouvement.
- LED de statut synchronisée sur la qualité de l'air (clignote en rouge en cas d'alerte).

### 4. Pont Somfy IO sur Heltec Wireless Bridge

Fichier: [somfy-io-bridge.yaml](somfy-io-bridge.yaml)

**Matériel :**

- Carte 868 MHz : [Heltec Wireless Bridge](https://heltec.org/project/wireless-bridge/)
  avec radio Semtech SX1276 intégrée.
- Antenne 868 MHz adaptée, indispensable avant toute émission.
- Composant :
  [laberning/home_io_control](https://github.com/laberning/home_io_control).

La carte utilise le bus SPI intégré (`GPIO5/27/19`) et les signaux radio
(`CS18`, `RESET14`, `DIO0 26`) de la version 868 MHz. Aucune radio externe
n'est nécessaire.

**Fonctionnalités :**

- Contrôle Somfy IO 1W (`Ouvrir`, `Fermer`, `Stop`) avec un état de volet
  estimé à partir des temps de course.
- Enrôlement du pont comme télécommande 1W Somfy supplémentaire.
- Écoute des télécommandes Somfy IO 1W déjà autorisées. Les appuis sont
  décodés dans les journaux DEBUG et publiés dans Home Assistant sous
  l'événement `esphome.home_io_control_sender_event`.
- Préparation au 2W : boutons de découverte/appairage, récupération de clé et
  recherche des périphériques déjà appairés.

Les identifiants `somfy_io_hub_node_id` et `somfy_io_hub_system_key` doivent
rester inchangés après l'enrôlement. Pour identifier une télécommande, flashez
la configuration, appuyez sur ses boutons puis relevez l'identifiant affiché
après `rx 1W remote` dans les journaux. Ajoutez-le à
un secret dédié, par exemple `somfy_io_remote_salon_id`, puis référencez ce
secret dans `exposed_senders` et dans le `follows` de chaque périphérique
concerné. Cette organisation permet d'associer plusieurs télécommandes à
plusieurs périphériques sans faire suivre toutes les commandes à chaque
entité. Une map imbriquée dans `secrets.yaml` ne peut pas être utilisée
directement : ESPHome attend des listes plates dans ces deux champs.

Pour appairer le pont, mettez le moteur en mode apprentissage avec une
télécommande déjà enregistrée puis utilisez le bouton `Enroll 1W Controller`
créé dans Home Assistant. Une identité 1W doit être enrôlée séparément pour
chaque périphérique piloté indépendamment.

Le 1W ne fournit ni accusé de réception ni position réelle : l'entité Home
Assistant expose donc une position estimée. Le 2W nécessite une configuration
propre à chaque périphérique et reste disponible via les assistants
d'appairage, mais n'est pas activé tant qu'aucune entité 2W n'est déclarée.
