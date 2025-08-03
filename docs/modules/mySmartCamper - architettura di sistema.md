🚐 mySmartCamper — Architettura del sistema

🧠 Sistema centrale (PC di bordo)
Dispositivo: Beelink Slim T4pro Ruolo: Centro di controllo e visualizzazione

Componente	Funzione
🧭 MyController v1	Automazione domotica via protocollo MySensors
📊 InfluxDB 1.8	Database per dati camper e meta
📈 Grafana	Visualizzazione dati e dashboard
🔁 Node-RED	Automazioni grafiche (rimpiazzabile da microservizi)

📡 Gateways fisici
Dispositivo	Funzione
🌐 ESP32 + nRF24L01+	Gateway radio Wi-Fi per nodi MySensors
🔌 Gateway Software (su Pi)	Gateway virtuale MQTT → MyController via MySensors binario
🔧 Nodi e sensori
Hardware	Funzione
RF-NANO / Arduino Pro Micro + nRF24L01+	Sensori fisici MySensors
ESP32	Sensori Wi-Fi o nodi intelligenti

🍓 Raspberry Pi — MetaBridge
Ruolo: Nodo complementare e orchestratore

Componente	Funzione
🧠 MetaScout	Orchestratore dei microservizi e configurazioni
🔀 Gateway Software	Traduzione MQTT → MySensors binario
📦 MetaData	Storage locale per Wikipedia offline, feed RSS, hotspot Wi-Fi, diario JSON
📡 MQTT Broker	Comunicazione tra microservizi
📖 MetaDiario	Generazione del diario di bordo
🌦️ MeteoScout	Microservizio meteo
📡 StarlinkScout	Monitoraggio connessione Starlink
📶 MikroScout	Monitoraggio access point Mikrotik
📡 RutScout	Monitoraggio router Teltonika RUT241
📊 MetaInflux	Accesso ai database InfluxDB ("meta" e "camper")

📡 Rete e comunicazione

Dispositivo	Funzione
🌐 Teltonika RUT241	Router WAN principale
📡 Mikrotik SXTsq Lite2	Access Point WAN esterno
🔐 ZeroTier	Accesso remoto sicuro tra PC e Pi

📷 Sicurezza e visione
Dispositivo	Funzione
🎥 Blink Mini 2	Telecamera interna
🎥 Blink Outdoor 3	Telecamera esterna
🔁 Blink Sync	Hub per telecamere Blink

🧠 Nodo AI/ML
Dispositivo	Funzione
🍓 Raspberry Pi MetaAiMl	Elaborazioni AI/ML (futuro modulo intelligente)

🧱 Sintesi concettuale
mySmartCamper è un sistema distribuito e modulare, dove ogni componente ha una responsabilità chiara. Il PC di bordo gestisce la domotica e la visualizzazione, mentre il Raspberry Pi MetaBridge aggrega, orchestra e arricchisce il sistema con microservizi e contenuti. La rete è gestita da dispositivi professionali (Teltonika, Mikrotik), e la sicurezza è garantita da telecamere Blink e accesso remoto via ZeroTier.