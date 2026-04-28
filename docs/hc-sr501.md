# Spécifications Techniques : Capteur de Mouvement HC-SR501

Module de détection de mouvement à infrarouge passif (PIR) réglable.

!Image du capteur

## 🛠 Caractéristiques Électriques

| Attribut | Valeur |
| :--- | :--- |
| **Tension de fonctionnement** | 5V ~ 20V |
| **Tension de sortie (Logique)** | 3.3V (Haut) / 0V (Bas) |
| **Courant statique** | < 50 µA |
| **Temps de blocage** | 2.5 secondes (par défaut) |
| **Angle de détection** | < 120° (cône) |

## 📉 Réglages Physiques

- **Potentiomètre Distance :** Sensibilité réglable de 3m à 7m.
- **Potentiomètre Temps :** Durée du signal haut après détection (5s à 200s).
- **Jumper Mode :**
  - **L** : Mode non répétable (déclenche une fois).
  - **H** : Mode répétable (réinitialise le délai à chaque mouvement détecté - Recommandé).

## 🔌 Brochage (Pinout)

| Pin | Fonction | Description |
| :--- | :--- | :--- |
| **VCC** | Alimentation | Entrée 5V impérative |
| **OUT** | Signal | Sortie logique 3.3V (Compatible ESP32) |
| **GND** | Masse | Référence 0V |

## 💡 Notes de Configuration ESPHome

- Bien que le signal soit en 3.3V, le capteur a besoin de 5V sur `VCC` pour fonctionner correctement.
- Un filtre `delayed_on` de 200ms est conseillé pour éviter les faux positifs liés aux parasites électriques.
