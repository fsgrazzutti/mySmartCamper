⚙️ Microservizio

Un microservizio è una unità software autonoma e specializzata, progettata per svolgere una funzione ben definita all’interno del sistema distribuito mySmartCamper. Opera su MetaBridge (Raspberry Pi) e comunica con il resto del sistema tramite MQTT, mentre i dati persistenti vengono salvati su un InfluxDB remoto installato sul PC di bordo.

🧩 Caratteristiche
Caratteristica	Descrizione
🔒 Isolamento funzionale	Ogni microservizio è indipendente e non condivide stato con altri
🧠 Responsabilità singola	Svolge una funzione specifica (es. meteo, diario, sensore, RSS)
🔄 Comunicazione asincrona	Usa MQTT per pubblicare e/o sottoscrivere messaggi
🛠️ Autonomia operativa	Può essere avviato, arrestato o aggiornato senza impattare il sistema
🧪 Testabilità locale	Può essere testato in isolamento, anche su PC
🔧 Configurazione modulare	Usa file di configurazione (es. config.json) per parametri e mapping
📦 Packaging leggero	Tipicamente implementato in Python (Flask, script) o altro linguaggio semplice
🌐 Interfaccia REST/HTML	Può offrire una GUI locale o API per interazione diretta
🗃️ Storage remoto su InfluxDB	Scrive su un database dedicato (services_db) installato sul PC di bordo
🔐 Accesso remoto	Può essere monitorato o aggiornato via SSH, Git, o interfaccia web

🧠 Architettura
+---------------------+
|  Microservizio X    |
|---------------------|
| - Funzione: Meteo   |
| - MQTT Pub/Sub      |
| - Config.json       |
| - Flask Web UI      |
| - InfluxDB (remoto) |
+---------------------+
        ↓
     MQTT Broker (Pi)
        ↓
   MetaBridge Orchestrator
        ↓
   Gateway MySensors → MyController
        ↓
   InfluxDB (PC di bordo)

🧱 Sintesi
🧩 I microservizi girano su MetaBridge, ma persistono i dati su InfluxDB remoto installato sul PC di bordo. 🔄 Questo consente una separazione pulita tra logica e storage, mantenendo basso il carico sul Raspberry Pi e centralizzando i dati.
🔄 Scalabilità modulare: puoi aggiungere o rimuovere microservizi senza modificare l’intero sistema
🧠 Manutenibilità: ogni servizio può essere aggiornato singolarmente
📖 Estensibilità narrativa: servizi come MetaDiario possono aggregare output da più microservizi
⚡️ Efficienza energetica: esecuzione su Raspberry Pi con basso consumo