# Spécifications Techniques : Capteur CO2 SCD41

Capteur de CO2 miniature utilisant la technologie de détection photoacoustique (NDIR) pour une mesure précise du dioxyde de carbone.

![Image du capteur](./img/scd41.jpg)

## 🛠 Caractéristiques Électriques

| Attribut | Valeur |
| :--- | :--- |
| **Tension de fonctionnement** | 2.4V ~ 5.5V |
| **Consommation moyenne** | 11 mA (mode normal) / 0.5 mA (low power) |
| **Interface de commande** | Bus I2C |
| **Adresse I2C** | 0x62 |
| **Température de service** | -10°C à +60°C |

## 📈 Performances de Mesure

- **Plage de mesure CO2 :** 400 - 5 000 ppm (extensible à 40 000 ppm).
- **Précision CO2 :** ±(40 ppm + 5% de la lecture).
- **Temps de réponse :** 60 secondes.
- **Plage Température :** -10°C à 60°C (±0.8°C).
- **Plage Humidité :** 0% à 95% RH (±6%).

## 🔌 Brochage (Pinout)

| Pin | Fonction | Description |
| :--- | :--- | :--- |
| **VDD** | Alimentation | Entrée 3.3V ou 5V |
| **GND** | Masse | Référence 0V |
| **SCL** | I2C Clock | Horloge du bus I2C |
| **SDA** | I2C Data | Données du bus I2C |

## 💡 Notes de Configuration ESPHome

- **Calibration :** Le capteur supporte l'Automatic Self-Calibration (ASC).
- **Mode :** Utilisé ici en `low_power_periodic` pour limiter l'auto-échauffement.
