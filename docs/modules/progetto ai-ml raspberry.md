# 🌙 Modalità Laboratorio AI Notturna – Camper Intelligente

## 🎯 Obiettivo

Attivare un Raspberry Pi dedicato all’AI/ML solo quando il camper è:
- Parcheggiato sotto casa
- Alimentato dalla rete elettrica
- In orario notturno (00:00–06:00)
- Non in uso (es. porte chiuse, nessun movimento)

Durante questa finestra, il sistema esegue:
- Training modelli AI/ML
- Aggiornamento moduli
- Consolidamento dati
- Backup su cloud
- Spegnimento sicuro al termine

---

## 🧩 Condizioni di attivazione

| Condizione         | Fonte              | Metodo di rilevamento |
|--------------------|--------------------|------------------------|
| Posizione GPS      | Domotica           | Geofencing “home”     |
| Alimentazione rete | Domotica / sensore | Smart plug / relè     |
| Orario notturno    | Raspberry / cron   | Finestra 00:00–06:00  |
| Stato camper       | Domotica           | Porte chiuse, no movimento |

---

## 🔁 Flusso operativo

```mermaid
graph TD
A[Domotica: GPS + Alimentazione + Orario] --> B[Trigger laboratorio AI]
B --> C[Accensione Raspberry AI]
C --> D[Avvio orchestrazione]
D --> E[Training + Backup + Deploy]
E --> F[Interruzione sicura + Log]
🧠 Attività eseguite
1. Training modelli AI/ML
Input: dati storici da InfluxDB

Output: modelli aggiornati (.pkl, .onnx, .tflite)

2. Aggiornamento moduli
Pull da repository Git

Ricostruzione container Docker

3. Consolidamento dati
Pulizia, aggregazione, compressione

4. Backup su cloud
Cloud: OneDrive / Google Drive

Tool: rclone

Destinazione: /CamperAIBackup

5. Interruzione sicura
Arresto container

Salvataggio log

Spegnimento Raspberry AI

🧰 Tecnologie consigliate
Componente	Tecnologia
ML Framework	scikit-learn, XGBoost, ONNX, TFLite
Container	Docker, docker-compose
Messaging	MQTT (Mosquitto)
Storage	InfluxDB
Backup	rclone → OneDrive / Google Drive
Logging	InfluxDB + Grafana
🛑 Interruzione sicura
Monitoraggio orario → stop alle 06:00

Arresto container Docker

Scrittura log su file o InfluxDB

Spegnimento Raspberry AI (opzionale)

☁️ Backup con rclone
Esempio comando:

bash
rclone sync /home/pi/models onedrive:/CamperAIBackup --log-file backup.log
📊 Notifiche
MQTT → ai/status/lab → “Training completato”, “Backup eseguito”

Log visibile in Grafana

Opzionale: invio email / Telegram

🧠 Vantaggi
Utilizzo intelligente dell’energia

Nessun impatto sul comfort o prestazioni

Modelli sempre aggiornati

Backup automatico e sicuro

📌 Note finali
Questa modalità può essere estesa in futuro per:

AutoML → tuning automatico dei modelli

Sincronizzazione con NAS locale

Riconoscimento automatico di scenari stagionali