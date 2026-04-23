# 🏠 Ma Domotique ESPHome

![ESPHome Validation](https://github.com/dguihal/esphome-conf/actions/workflows/esphome-validation.yml/badge.svg)

Ce dépôt centralise toutes les configurations de mes modules ESPHome. 

## 🛠 Structure du Repo
- `/common/` : Fragments de code partagés (Wi-Fi, API, OTA).
- `*.yaml` : Fichiers de configuration par appareil.
- `secrets.yaml.example` : Modèle pour les données sensibles (le vrai fichier est ignoré par Git).

---

## 🏗 Projets Actuels

### 1. Volet Somfy RTS + Ventilateur plafonnier Mantra

Fichier: [bt-proxy-rts.yaml](bt-proxy-rts.yaml)

**Matériel :**
- Carte : `ESP32-C6-LCD-1.47` [Waveshare Wiki](https://www.waveshare.com/wiki/ESP32-C6-LCD-1.47)
- Radio : `CC1101` (433MHz)
- Composants:
  - [leonardpitzu/somfy_cover](https://github.com/leonardpitzu/somfy_cover)
  - [NicoIIT/esphome-ble_adv_proxy](https://github.com/NicoIIT/esphome-ble_adv_proxy)

**Câblage (SPI) :**

| CC1101 Pin | ESP32-C6 GPIO | Fonction |
| :--- | :--- | :--- |
| **VCC** | 3.3V | Alimentation |
| **GND** | GND | Masse |
| **SCK** | GPIO 20 | SPI Clock |
| **MISO** | GPIO 19 | SPI MISO |
| **MOSI** | GPIO 18 | SPI MOSI |
| **CSN** | GPIO 9 | SPI Chip Select |
| **GDO0** | GPIO 1 | Interrupt / Data |


### 2. Watermeter

Fichier: [watermeter.yaml](watermeter.yaml)

**Matériel :**
- Carte : `AZ Delivery esp-32-dev-kit-c-v4` [AZ Delivery](https://www.az-delivery.de/fr/products/esp-32-dev-kit-c-v4)
- Capteur de débit d'eau liquide Effet Hall [Amazon](https://www.amazon.fr/SWAWIS-Pi%C3%A8ces-Capteur-D%C3%A9bitm%C3%A8tre-Commutateur/dp/B0C2GT6LHY)


**Câblage :**

| Capteur débit | ESP32 GPIO | Fonction |
| :--- | :--- | :--- |
| Fil Rouge | 5V | Alimentation |
| Fil Noir | GND | Masse |
| Fil Jaune | GPIO 26 | Impulsions de comptage |

