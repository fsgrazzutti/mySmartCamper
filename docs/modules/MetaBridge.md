🧭 MetaBridge

🔄 Ruolo di MetaBridge

MetaBridge è un componente complementare a MyController, progettato per:

🧩 Aggregare dati e servizi provenienti da microservizi MQTT e dal Gateway mySensors ospitati sul Raspberry Pi che ospita MetaBridge stesso

🔄 Tradurre e instradare i dati verso il gateway MySensors

📖 Generare contenuti narrativi attraverso il microservizio MetaDiario

È una entità interconnessa e indipendente dal PC di bordo, operante su un Raspberry Pi configurato per minimo impatto energetico, ideale per operatività continua e discreta.

⚡️ Caratteristiche hardware e architetturali
🧱 Basato su Raspberry Pi Ottimizzato per consumi ridotti e operatività autonoma.

🔌 Indipendente dal PC di bordo Funziona autonomamente, con accesso remoto, orchestrazione interna e storage locale.

🔄 Interconnesso via rete locale e ZeroTier Comunica con il PC e altri nodi, ma non dipende da essi per eseguire le sue funzioni.

🧩 Funzioni principali
Funzione	Descrizione
🔌 Client MQTT	Riceve dati da microservizi locali (diario, meteo, RSS, ecc.)
🧠 Orchestratore	Interpreta config.json per mappare i dati verso MySensors
🌐 Gateway MySensors Ethernet	Invia dati binari a MyController v1 sul PC
📡 Broker MQTT (Mosquitto)	Smista messaggi tra microservizi e orchestratore
🗃️ Storage locale	Salva dati selettivi su services_db per analisi o backup
🔐 Accesso remoto via ZeroTier	Permette manutenzione da remoto
🛠️ Ambiente di test e deploy	Riceve aggiornamenti via git pull dal PC di sviluppo
📖 Host del microservizio MetaDiario	Aggrega eventi, stati e luoghi visitati per generare il diario di bordo
📖 MetaDiario: il diario del sistema

MetaBridge è il naturale ospite del microservizio MetaDiario, che:

🧠 Integra eventi e stati del sistema mySmartCamper

📍 Raccoglie luoghi visitati e contesto ambientale

📝 Genera un diario di bordo ricco e coerente

🌐 Consente la pubblicazione su blog e social, o il salvataggio locale

🧱 Sintesi concettuale
🧩 MetaBridge è un nodo complementare, autonomo e a basso consumo, che estende le capacità di MyController. 🔄 Agisce come ponte logico tra microservizi MQTT e MySensors. 📖 È anche il narratore del sistema, grazie a MetaDiario, trasformando dati tecnici in contenuti umani e condivisibili.