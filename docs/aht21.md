# Spécifications Techniques : Capteur Température & Humidité AHT21

Version améliorée du AHT10/AHT20, offrant une meilleure stabilité et précision.

!Image du capteur

## 🛠 Caractéristiques Électriques

| Attribut | Valeur |
| :--- | :--- |
| **Tension de fonctionnement** | 2.2V ~ 5.5V |
| **Courant en mesure** | 980 µA |
| **Interface de commande** | Bus I2C |
| **Adresse I2C** | 0x38 |
| **Temps de mesure** | 80 ms |

## 📈 Performances de Mesure

- **Précision Température :** ±0.3°C.
- **Plage Température :** -40°C à +80°C.
- **Précision Humidité :** ±2% RH.
- **Plage Humidité :** 0% à 100% RH.
- **Stabilité long terme :** < 0.1°C / an.

## 🔌 Brochage (Pinout)

| Pin | Fonction | Description |
| :--- | :--- | :--- |
| **VDD** | Alimentation | 3.3V (Recommandé) |
| **GND** | Masse | Référence 0V |
| **SCL** | I2C Clock | Horloge du bus I2C |
| **SDA** | I2C Data | Données du bus I2C |

## 💡 Notes de Configuration ESPHome

- Utilisé ici via la plateforme `aht10` avec le variant `AHT20`.
