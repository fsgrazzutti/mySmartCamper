# 🚐 mySmartCamper

**mySmartCamper** è un ecosistema modulare e distribuito per la gestione intelligente di un camper. Combina automazione, monitoraggio, archiviazione dati e accesso offline a contenuti, il tutto orchestrato da microservizi e dispositivi embedded.

---

## 🧩 Architettura del sistema

### 🖥️ PC di bordo (Beelink Slim T4pro)
- **MyController v1**: gestione rete MySensors
- **InfluxDB 1.8**: database per dati camper e meta
- **Grafana**: dashboard e visualizzazione
- **Node-RED**: automazioni grafiche (rimpiazzabile da microservizi)

### 🍓 Raspberry Pi — MetaBridge
- **MetaScout**: orchestratore microservizi
- **Gateway Software**: bridge MQTT → MySensors
- **MetaData**: storage locale (Wikipedia offline, feed RSS, hotspot Wi-Fi, diario JSON)
- **MQTT Broker**: comunicazione tra microservizi

### 📡 Microservizi attuali
- `RutScout`: monitor router Teltonika RUT241
- `MikroScout`: monitor access point Mikrotik SXTsq Lite2
- `StarlinkScout`: monitor connessione Starlink
- `MeteoScout`: meteo locale
- `MetaDiario`: diario di bordo JSON
- `MetaData`: storage contenuti
- `MetaInflux`: accesso ai DB Influx ("meta", "camper")

### 📶 Gateways
- ESP32 + nRF24L01+: gateway radio Wi-Fi
- Gateway Software su Pi: bridge MQTT → MySensors

### 🔧 Nodi e sensori
- RF-NANO / Arduino Pro Micro + nRF24L01+
- ESP32 (Wi-Fi o nodi intelligenti)

### 📷 Sicurezza e visione
- Blink Mini 2 (telecamera interna)
- Blink Outdoor 3 (telecamera esterna)
- Blink Sync (hub telecamere)

### 🌐 Rete e accesso remoto
- Teltonika RUT241 (router WAN)
- Mikrotik SXTsq Lite2 (access point WAN)
- ZeroTier (VPN per accesso remoto)

### 🧠 Nodo AI/ML
- Raspberry Pi MetaAiMl: elaborazioni AI/ML (in sviluppo)

---

## 📦 Requisiti software

- Linux (Debian-based consigliato)
- Docker (opzionale per microservizi)
- Node.js (per Node-RED e microservizi)
- Python (per microservizi e script)
- MQTT Broker (es. Mosquitto)
- InfluxDB 1.8
- Grafana
- MyController v1

---


# Accedi a Grafana
http://localhost:3000
