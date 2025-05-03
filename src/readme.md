# ESP32 MQTT OTA

Un système modulaire pour la mise à jour à distance (OTA - Over The Air) d'ESP32 via le protocole MQTT.

## Aperçu

Ce projet permet de mettre à jour le firmware d'un ESP32 à distance, sans connexion physique, en utilisant le protocole MQTT. La conception modulaire du code facilite sa réutilisation et son adaptation à différents projets.

## Fonctionnalités

- ✅ Connexion WiFi configurable et gestion multiple de réseaux
- ✅ Communication bidirectionnelle via MQTT
- ✅ Vérification des mises à jour disponibles
- ✅ Téléchargement et installation automatique du firmware
- ✅ Stockage persistant des configurations dans la mémoire non volatile
- ✅ Interface web de contrôle
- ✅ Architecture modulaire pour une meilleure maintenance

## Structure du projet
ESP32_MQTT_OTA/
├── src/                    # Code source
│   ├── main.ino            # Fichier principal
│   ├── config.h            # Configuration
│   ├── wifi_manager.h/cpp  # Module de gestion WiFi
│   ├── mqtt_client.h/cpp   # Module client MQTT
│   ├── ota_updater.h/cpp   # Module de mise à jour OTA
│   └── storage.h/cpp       # Module de stockage
├── web_interface/          # Interface web de contrôle
│   └── mqtt_ota_control_interface.html
├── firmware/               # Fichiers de mise à jour
│   ├── firmware.bin        # Firmware compilé
│   └── version.json        # Informations de version
└── examples/               # Exemples d'utilisation
└── without_ota/        # Version sans fonctionnalité OTA

## Prérequis

### Matériel
- ESP32 (testé sur ESP32-WROOM-32)

### Logiciels
- Arduino IDE (1.8.x ou plus récent)
- PlatformIO (alternative recommandée)

### Bibliothèques
- WiFi.h (incluse dans l'ESP32 Core)
- PubSubClient (pour MQTT)
- ArduinoJson
- HTTPClient & HTTPUpdate (incluses dans l'ESP32 Core)
- Preferences (incluse dans l'ESP32 Core)

## Installation

1. Clonez ce dépôt : git clone https://github.com/votre-nom-utilisateur/ESP32_MQTT_OTA.git
   2. Ouvrez le dossier `src` dans l'Arduino IDE ou importez le projet dans PlatformIO.

3. Modifiez le fichier `config.h` selon vos besoins :
- Informations de connexion WiFi
- Configuration du broker MQTT
- URLs pour les mises à jour

4. Compilez et téléversez le code sur votre ESP32.

## Utilisation

### Commandes MQTT

Le système répond aux commandes suivantes envoyées au topic `esp32/[esp32_id]/command` :

| Commande | Description |
|----------|-------------|
| `check_update` | Vérifie si une mise à jour est disponible |
| `force_update:[URL_VERSION]:[URL_FIRMWARE]` | Force une mise à jour avec les URL spécifiées |
| `restart` | Redémarre l'ESP32 |
| `add_wifi:[SSID]:[PASSWORD]` | Ajoute ou met à jour un réseau WiFi |
| `list_wifi` | Liste les réseaux WiFi enregistrés |
| `clear_wifi` | Efface tous les réseaux WiFi enregistrés |

### Interface web

Une interface web HTML est fournie pour contrôler facilement l'ESP32 à distance :

1. Ouvrez le fichier `web_interface/mqtt_ota_control_interface.html` dans un navigateur.
2. Configurez les paramètres de connexion MQTT.
3. Utilisez les différents onglets pour :
- Vérifier et forcer les mises à jour
- Configurer les URLs GitHub
- Gérer les réseaux WiFi

## Fonctionnement des mises à jour OTA

1. Le système vérifie périodiquement ou sur demande un fichier JSON contenant les informations de version.
2. Si une nouvelle version est disponible, il peut télécharger le firmware correspondant.
3. Le firmware est installé et l'ESP32 redémarre automatiquement.

Exemple de fichier `version.json` :
```json
{
"version": "1.0.1",
"firmware_url": "https://raw.githubusercontent.com/votre-nom-utilisateur/ESP32_MQTT_OTA/main/firmware/firmware.bin"
}
