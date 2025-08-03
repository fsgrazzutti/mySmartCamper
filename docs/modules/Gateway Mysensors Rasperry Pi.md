🧰 1. Preparazione della SD card
🔧 Usa Raspberry Pi Imager
Scarica Raspberry Pi Imager su PC/Mac.

⚙️ Configura in modalità avanzata
Premi Ctrl + Shift + X per aprire le opzioni avanzate:

✅ Abilita SSH (con password o chiave pubblica)

📶 Inserisci SSID e password del Wi-Fi

🌍 Imposta paese (es. IT per Italia)

👤 Imposta utente e password

🖥️ Disabilita interfaccia grafica scegliendo Raspberry Pi OS Lite

Scrivi l’immagine sulla SD e inseriscila nel Raspberry Pi.

🚀 2. Primo avvio e accesso via SSH
Collega l’alimentazione al Pi

Attendi 1–2 minuti per la connessione Wi-Fi

Trova l’IP del Pi (es. via router o ping raspberrypi.local)

Accedi via SSH:

bash
ssh pi@raspberrypi.local
🧹 3. Disabilita tutto tranne il Wi-Fi
🔌 Disabilita HDMI, audio, Bluetooth, LED, ecc.
Modifica /boot/config.txt:

bash
sudo nano /boot/config.txt
Aggiungi in fondo:

ini
# Disabilita HDMI
hdmi_blanking=2

# Disabilita audio
dtparam=audio=off

# Disabilita LED
dtparam=act_led_trigger=none
dtparam=act_led_activelow=on

# Disabilita Bluetooth
dtoverlay=disable-bt
🧠 Disabilita servizi inutili
bash
sudo systemctl disable hciuart
sudo systemctl disable triggerhappy
sudo systemctl disable dphys-swapfile
sudo systemctl disable avahi-daemon
🧼 Disabilita interfacce di rete non Wi-Fi
Modifica /etc/network/interfaces o usa nmcli se hai Raspberry Pi OS Bookworm:

bash
nmcli device set eth0 managed no
nmcli device disconnect eth0
🧪 4. Test finale
Riavvia:

bash
sudo reboot
Verifica che:

Il Pi si connetta solo via Wi-Fi

SSH funzioni

Nessun servizio inutile sia attivo

🔗 Riferimenti utili
Configurazione headless con Raspberry Pi Imager

Modifica Wi-Fi su SD card
---
script bash completo per:

✅ Disattivare servizi inutili ✅ Ridurre il consumo energetico ✅ Disabilitare interfacce non necessarie ✅ Ridurre la frequenza del clock della CPU ✅ Ottimizzare il Raspberry Pi 3B+ per uso headless Wi-Fi-only

🧾 Script: minimal_pi_setup.sh
bash
#!/bin/bash

echo "🔧 Configurazione minimale del Raspberry Pi..."

# 1. Disabilita servizi inutili
echo "🧹 Disabilitazione servizi..."
SERVICES=(
  hciuart
  triggerhappy
  dphys-swapfile
  avahi-daemon
  bluetooth
)

for service in "${SERVICES[@]}"; do
  sudo systemctl disable "$service" 2>/dev/null
  sudo systemctl stop "$service" 2>/dev/null
done

# 2. Disabilita HDMI, audio, LED, Bluetooth via config.txt
echo "📝 Modifica /boot/config.txt..."
sudo sed -i '/^# Minimal config start/,/^# Minimal config end/d' /boot/config.txt
sudo bash -c 'cat >> /boot/config.txt' <<EOF

# Minimal config start
# Disabilita HDMI
hdmi_blanking=2

# Disabilita audio
dtparam=audio=off

# Disabilita LED
dtparam=act_led_trigger=none
dtparam=act_led_activelow=on

# Disabilita Bluetooth
dtoverlay=disable-bt

# Riduci clock CPU (600 MHz min, 900 MHz max)
arm_freq=900
arm_freq_min=600
# Minimal config end
EOF

# 3. Disabilita Ethernet (se non usata)
echo "🌐 Disconnessione Ethernet..."
sudo nmcli device disconnect eth0 2>/dev/null || echo "nmcli non disponibile, salto..."

# 4. Disabilita swap
echo "🚫 Disabilitazione swap..."
sudo dphys-swapfile swapoff
sudo systemctl disable dphys-swapfile

# 5. Rimuovi pacchetti non necessari (opzionale)
echo "📦 Rimozione pacchetti opzionali..."
sudo apt purge -y triggerhappy wolfram-engine libreoffice* scratch* sonic-pi minecraft-pi || true
sudo apt autoremove -y

# 6. Riavvio finale
echo "✅ Configurazione completata. Riavvio in 5 secondi..."
sleep 5
sudo reboot
▶️ Come usarlo
Salva lo script:

bash
nano minimal_pi_setup.sh
Incolla il contenuto sopra, salva e chiudi (Ctrl+O, Ctrl+X)

Rendi eseguibile:

bash
chmod +x minimal_pi_setup.sh
Esegui:

bash
./minimal_pi_setup.sh
🧠 Note
Il clock ridotto (arm_freq_min=600, arm_freq=900) è stabile per la maggior parte degli usi headless.

Puoi abbassarlo ulteriormente se vuoi massimizzare il risparmio energetico (es. arm_freq=700).

Se usi Raspberry Pi OS Bookworm, il comando nmcli è disponibile per gestire le interfacce di rete.
---
🧾 Script: pi_minimal_optimize.sh
bash
#!/bin/bash

echo "🔧 Ottimizzazione Raspberry Pi 3B+ headless..."

# 1. Disabilita servizi inutili
echo "🧹 Disabilitazione servizi..."
SERVICES=(
  hciuart
  triggerhappy
  dphys-swapfile
  avahi-daemon
  bluetooth
)

for service in "${SERVICES[@]}"; do
  sudo systemctl disable "$service" 2>/dev/null
  sudo systemctl stop "$service" 2>/dev/null
done

# 2. Modifica /boot/config.txt per clock e termica
echo "📝 Modifica /boot/config.txt..."
sudo sed -i '/^# Minimal config start/,/^# Minimal config end/d' /boot/config.txt
sudo bash -c 'cat >> /boot/config.txt' <<EOF

# Minimal config start
# Disabilita HDMI
hdmi_blanking=2

# Disabilita audio
dtparam=audio=off

# Disabilita LED
dtparam=act_led_trigger=none
dtparam=act_led_activelow=on

# Disabilita Bluetooth
dtoverlay=disable-bt

# Riduci clock CPU (termico + energetico)
arm_freq=900
arm_freq_min=600
# Minimal config end
EOF

# 3. Disattiva power saving Wi-Fi via systemd
echo "📶 Creazione servizio Wi-Fi always-on..."
sudo bash -c 'cat > /etc/systemd/system/wifi-nosleep.service' <<EOF
[Unit]
Description=Disattiva power saving Wi-Fi
After=network.target

[Service]
ExecStart=/sbin/iwconfig wlan0 power off
Type=oneshot

[Install]
WantedBy=multi-user.target
EOF

sudo systemctl enable wifi-nosleep.service

# 4. Disconnessione Ethernet (se non usata)
echo "🌐 Disconnessione Ethernet..."
sudo nmcli device disconnect eth0 2>/dev/null || echo "nmcli non disponibile, salto..."

# 5. Rimozione pacchetti opzionali (opzionale)
echo "📦 Rimozione pacchetti inutili..."
sudo apt purge -y triggerhappy wolfram-engine libreoffice* scratch* sonic-pi minecraft-pi || true
sudo apt autoremove -y

# 6. Riavvio finale
echo "✅ Ottimizzazione completata. Riavvio in 5 secondi..."
sleep 5
sudo reboot
▶️ Come usarlo
Salva lo script:

bash
nano pi_minimal_optimize.sh
Incolla il contenuto sopra, salva e chiudi (Ctrl+O, Ctrl+X)

Rendi eseguibile:

bash
chmod +x pi_minimal_optimize.sh
Esegui:

bash
./pi_minimal_optimize.sh
---
#!/bin/bash

# Percorso dell'eseguibile
GATEWAY_PATH="/home/pi/MySensors/bin/mysgw"
SERVICE_PATH="/etc/systemd/system/mysgw.service"

# Verifica che il file esista
if [ ! -f "$GATEWAY_PATH" ]; then
  echo "❌ Errore: il file $GATEWAY_PATH non esiste. Assicurati di aver compilato il gateway."
  exit 1
fi

# Crea il file di servizio
echo "🛠️ Creazione del servizio systemd..."
sudo bash -c "cat > $SERVICE_PATH" <<EOF
[Unit]
Description=MySensors Gateway
After=network.target

[Service]
ExecStart=$GATEWAY_PATH
WorkingDirectory=/home/pi/MySensors
StandardOutput=inherit
StandardError=inherit
Restart=always
User=pi

[Install]
WantedBy=multi-user.target
EOF

# Ricarica systemd e abilita il servizio
echo "🔄 Ricarico systemd..."
sudo systemctl daemon-reexec
sudo systemctl daemon-reload

echo "✅ Abilitazione e avvio del servizio..."
sudo systemctl enable mysgw.service
sudo systemctl start mysgw.service

echo "🎉 Fatto! Il gateway è ora avviato automaticamente al boot."


---

🛠️ Requisiti
Raspberry Pi 3B+ con Raspbian (consigliato: Lite o Desktop)

Connessione internet

Accesso SSH o terminale

Nessun modulo radio (come richiesto)

⚙️ Installazione del gateway MySensors
1. 🔄 Aggiorna il sistema
bash
sudo apt update && sudo apt upgrade -y
2. 📦 Installa i pacchetti necessari
bash
sudo apt install git build-essential
3. 📥 Clona il repository MySensors
bash
git clone https://github.com/mysensors/MySensors.git --branch master
cd MySensors
Se vuoi usare la versione di sviluppo più aggiornata:

bash
git clone https://github.com/mysensors/MySensors.git --branch development
🧩 Configura il gateway senza radio
Puoi scegliere tra tre tipi di gateway software:

Ethernet (default, ascolta sulla porta 5003)

Seriale virtuale

MQTT

Esempio: gateway Ethernet
bash
./configure --my-gateway=ethernet --my-port=5003
Esempio: gateway MQTT (se usi Mosquitto o simili)
bash
./configure --my-gateway=mqtt \
  --my-controller-ip-address=127.0.0.1 \
  --my-mqtt-publish-topic-prefix=mysensors-out \
  --my-mqtt-subscribe-topic-prefix=mysensors-in \
  --my-mqtt-client-id=mygateway1
Esempio: gateway seriale virtuale
bash
./configure --my-gateway=serial \
  --my-serial-is-pty \
  --my-serial-port=/dev/ttyMySensorsGateway
🔨 Compila il gateway
bash
make
Se tutto va bene, verrà creato il file eseguibile:

bash
bin/mysgw
🚀 Avvio del gateway
Puoi avviarlo manualmente:

bash
sudo ./bin/mysgw
Oppure creare un servizio systemd per avviarlo automaticamente all'accensione.

📁 Configurazione avanzata
Puoi usare un file di configurazione personalizzato:

bash
./configure --my-config-file=/etc/mysensors.conf
🔗 Riferimenti utili
Guida ufficiale MySensors per Raspberry Pi

Forum di supporto MySensors

---
🧩 1. Verifica percorso del gateway
Assumiamo che il file eseguibile si trovi in:

bash
/home/pi/MySensors/bin/mysgw
Se è in un altro percorso, sostituiscilo nello script qui sotto.

📝 2. Crea il servizio systemd
Apri il file del servizio:

bash
sudo nano /etc/systemd/system/mysgw.service
Inserisci questo contenuto:

ini
[Unit]
Description=MySensors Gateway
After=network.target

[Service]
ExecStart=/home/pi/MySensors/bin/mysgw
WorkingDirectory=/home/pi/MySensors
StandardOutput=inherit
StandardError=inherit
Restart=always
User=pi

[Install]
WantedBy=multi-user.target
🔧 3. Abilita e avvia il servizio
bash
sudo systemctl daemon-reexec
sudo systemctl daemon-reload
sudo systemctl enable mysgw.service
sudo systemctl start mysgw.service
✅ 4. Verifica che funzioni
Controlla lo stato:

bash
systemctl status mysgw.service
Oppure visualizza i log:

bash
journalctl -u mysgw.service -f
🧼 5. (Opzionale) Nascondi output e log
Se vuoi un servizio più silenzioso, puoi modificare la sezione [Service] così:

ini
StandardOutput=null
StandardError=null
---
🧾 Script: setup_mysgw_service.sh
bash
#!/bin/bash

# Percorso dell'eseguibile
GATEWAY_PATH="/home/pi/MySensors/bin/mysgw"
SERVICE_PATH="/etc/systemd/system/mysgw.service"

# Verifica che il file esista
if [ ! -f "$GATEWAY_PATH" ]; then
  echo "❌ Errore: il file $GATEWAY_PATH non esiste. Assicurati di aver compilato il gateway."
  exit 1
fi

# Crea il file di servizio
echo "🛠️ Creazione del servizio systemd..."
sudo bash -c "cat > $SERVICE_PATH" <<EOF
[Unit]
Description=MySensors Gateway
After=network.target

[Service]
ExecStart=$GATEWAY_PATH
WorkingDirectory=/home/pi/MySensors
StandardOutput=inherit
StandardError=inherit
Restart=always
User=pi

[Install]
WantedBy=multi-user.target
EOF

# Ricarica systemd e abilita il servizio
echo "🔄 Ricarico systemd..."
sudo systemctl daemon-reexec
sudo systemctl daemon-reload

echo "✅ Abilitazione e avvio del servizio..."
sudo systemctl enable mysgw.service
sudo systemctl start mysgw.service

echo "🎉 Fatto! Il gateway è ora avviato automaticamente al boot."
▶️ Come usarlo
Salva lo script:

bash
nano setup_mysgw_service.sh
Incolla il contenuto sopra, salva e chiudi (Ctrl+O, Ctrl+X)

Rendi eseguibile:

bash
chmod +x setup_mysgw_service.sh
Esegui:

bash
./setup_mysgw_service.sh