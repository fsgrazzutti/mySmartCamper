# 🌐 Modello Semantico – I 4 Elementi Ambientali

Sistema concettuale che raggruppa sensori, attuatori e regole in **macro-dominii tematici**, per semplificare la gestione logica e la visualizzazione del sistema embedded **mySmartCamper**.

---

## 🔹 I 4 Elementi

| Simbolo | Elemento     | Descrizione                                          |
|---------|--------------|------------------------------------------------------|
| 💧      | **Acque**     | Flusso idrico, filtrazione, riscaldamento, scarico, serbatoi |
| 🔋      | **Batterie**  | Stato di carica, tensione, corrente, autonomia energetica |
| 🔥      | **Gas**       | Livello bombole, sicurezza, combustione, fornelli    |
| ⚡      | **Carichi / Sorgenti** | Energia elettrica, inverter, display, ventole, solare |

Ogni entità sensoriale o attuativa è mappata su almeno uno di questi elementi → utili per:
- Regole logiche e semantiche
- Visualizzazione embedded
- Analisi termica, consumi e automazioni intelligenti

---

## 🧩 Mappatura Entità per Elemento

### 💧 Acque
- Nodo `waterfilterboiler`
- Flusso ingresso / scarico elettrico
- Sensori livello acque grigie
- Filtri, lampada UV, miscelatori

### 🔋 Batterie
- Nodo monitoraggio tensione (disponibile/da installare)
- Stato inverter / solar / carica dinamica
- Viewer autonomia sistema

### 🔥 Gas
- Sonde Mopeka (11 kg e 6 kg)
- Bruciatori, fornelli cucina
- Sicurezza gas → fallback automatico

### ⚡ Carichi / Sorgenti
- Luci LED (dinette, esterni, soffitto)
- Ventole (bagno, frigo, controller)
- Display embedded ESP32
- Inverter, moduli di carico attivi
- Bilanciamento carichi + priorità fallback

---

## 🎯 Utilizzo nei Moduli del Sistema

| Modulo                  | Utilizzo Elementi                           |
|-------------------------|---------------------------------------------|
| `rule-engine.md`        | Attivazione regole per carico / consumi / comfort |
| `ui-display.md`         | Icone semantiche (💧 🔋 🔥 ⚡) → viewer intelligente |
| `entities-mapping.md`   | Categorizzazione nodi / childId / zona      |
| `scenari-logici.md`     | Fallback e gestione sicurezza per ciascun elemento |
| `metrics_visualization_strategy.json` | Colorazioni e soglie per stato risorse |

---

## ✅ Vantaggi Architetturali

- Facilita lettura del sistema anche da utenti non tecnici
- Permette aggregazione e sintesi nei viewer
- Supporta regole logiche scalabili
- Estendibile a nuovi elementi (es. aria, luce, movimento...)

---

✳️ Ultimo aggiornamento: `12 Luglio 2025`  
📁 Posizione consigliata: `📘 Docs/elements-model.md`
