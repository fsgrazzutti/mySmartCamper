🔀 Gateway MySensors

Il Gateway MySensors è un componente software che agisce come interfaccia bidirezionale tra i microservizi MQTT e il controller domotico MyController, installato sul PC di bordo. Non utilizza moduli radio né comunica con nodi fisici: la sua funzione è tradurre i messaggi MQTT in formato binario MySensors e viceversa, permettendo l’integrazione tra il mondo dei microservizi e il sistema domotico.

🧩 Caratteristiche
Caratteristica	Descrizione
🔁 Gateway logico	Ponte tra microservizi MQTT e MyController
🧭 Dati in ingresso	Riceve da microservizi (es. GPS, meteo, sensori virtuali)
🧠 Dati in uscita	Inoltra a MyController per regole e automazioni
📤 Traduzione protocollo	Converte MQTT → MySensors binario e viceversa
🔌 Connessione seriale/TCP	Comunica con MyController via porta seriale o socket
🧪 Simulazione nodi virtuali	Emula nodi MySensors per compatibilità con il controller
🧱 Nessuna radio fisica	Non usa moduli NRF/RFM, tutto è software
🔧 Configurazione flessibile	Mapping tra topic MQTT e ID MySensors configurabile
📋 Logging e diagnostica	Log dettagliati per debug e tracciabilità
🔐 Accesso remoto	Monitorabile via SSH o interfaccia web del controller

🧠 Architettura funzionale
+------------------------+
|   Microservizio GPS    |
|   (MQTT Publisher)     |
+------------------------+
           ↓
        MQTT Broker
           ↓
+------------------------+
|   Gateway MySensors    |
| - MQTT Subscriber      |
| - Traduzione binaria   |
| - Emulazione nodi      |
+------------------------+
           ↓
+------------------------+
|   MyController (PC)    |
| - Regole automazione   |
| - Storage InfluxDB     |
| - GUI domotica         |
+------------------------+

🧱 Sintesi
🔀 Il Gateway MySensors è un ponte software che permette ai microservizi MQTT di interagire con MyController, simulando nodi virtuali e traducendo i messaggi nel protocollo binario MySensors. 🧭 In questo modo, dati come GPS, meteo, stato camper possono essere usati nelle regole di automazione del sistema domotico.