# 🏠 Ma Domotique ESPHome

![ESPHome Validation](https://github.com/dguihal/esphome-conf/actions/workflows/esphome-validation.yml/badge.svg)

Ce dépôt centralise toutes les configurations de mes modules ESPHome.

## 🛠 Structure du Repo

- `/common/` : Fragments de code partagés (Wi-Fi, API, OTA).
- `/fonts/` : Fichiers de police TrueType (.ttf) utilisés pour les écrans.
- `*.yaml` : Fichiers de configuration par appareil.
- `secrets.yaml.example` : Modèle pour les données sensibles (le vrai fichier est ignoré par Git).

---

## 🏗 Projets Actuels

### 1. Volet Somfy RTS + Ventilateur plafonnier Mantra

Fichier: [bt-proxy-rts.yaml](bt-proxy-rts.yaml)

**Matériel :**

- Carte : `ESP32-C6-LCD-1.47` [Waveshare Wiki](https://www.waveshare.com/wiki/ESP32-C6-LCD-1.47)
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
| **GDO2** | GPIO 13 | Data Receive |


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
