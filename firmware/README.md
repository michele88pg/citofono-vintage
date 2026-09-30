# Firmware (ESPHome)

Firmware per la scheda rev 0.5 (Siemens S62 su impianto URMET 4+N), scritto in ESPHome.
Configurazione **validata con ESPHome 2026.6** e codice C++ generato correttamente; la compilazione finale va fatta sul tuo computer (vedi sotto).

## Cosa fa

| Funzione | Come |
|---|---|
| **Apertura con la rotella** | Componi un numero qualsiasi → il portone si apre (come molti citofoni-telefono d'epoca). Opzionale: un **codice segreto** da comporre, impostabile da Home Assistant o dalla pagina web |
| **Apertura remota** | Pulsante "Apri portone" in Home Assistant o nella pagina web. Prima simula lo sgancio (K2) per abilitare la linea URMET, poi chiude K1 |
| **Chiamata** | Rileva quando suonano al posto esterno: squilla il buzzer, genera un evento "Campanello" in Home Assistant, aggiorna "Ultima chiamata" e il contatore |
| **Notifica sul telefono** | Con le automazioni in [`home-assistant/`](../home-assistant): notifica con il pulsante "Apri il portone" dentro |
| **Non disturbare** | Silenzia il buzzer; la notifica arriva comunque |
| **Apertura automatica** | Se attivata, apre da sola 2 s dopo la chiamata (feste, consegne attese). Si spegne da sola al riavvio |
| **Pagina web locale** | `http://citofono-vintage.local`: funziona anche senza Home Assistant |
| **Sicurezze** | Relè sempre aperti all'accensione; K1 non resta chiuso più di 5 s, K2 non più di 2 minuti |

Google Home, Apple Home e Alexa si collegano **tramite Home Assistant**: vedi [docs/home-assistant.md](../docs/home-assistant.md).

## Primo caricamento

1. Installa ESPHome sul computer: `pip install esphome` (oppure l'add-on ESPHome in Home Assistant).
2. Copia `secrets.yaml.example` come `secrets.yaml` e compila WiFi e password.
   Per la chiave API: `openssl rand -base64 32`.
3. Collega la scheda al computer con un cavo **USB-C dati** (non solo ricarica).
4. `esphome run citofono-vintage.yaml` → scegli la porta USB.
   Se la scheda non viene vista: tieni premuto **BOOT**, premi e rilascia **RESET**, poi rilascia BOOT.
5. Gli aggiornamenti successivi arrivano via WiFi: `esphome run citofono-vintage.yaml` e scegli l'indirizzo di rete.

Se il WiFi configurato non è raggiungibile, la scheda crea la rete **Citofono-Setup**: collegati dal telefono e scegli la rete di casa.

## Collaudo

Il piano di collaudo passo passo, prima da banco e poi sull'impianto, è in [docs/collaudo.md](../docs/collaudo.md).

## File

- `citofono-vintage.yaml` — il firmware
- `secrets.yaml.example` — modello delle password (il vero `secrets.yaml` non va mai pubblicato)
- [`home-assistant/automazioni-citofono.yaml`](../home-assistant/automazioni-citofono.yaml) — notifica con pulsante "Apri"
- `audio-es8311.yaml` — pacchetto opzionale per la scheda audio ES8311 (vedi [hardware/audio-board](../hardware/audio-board/README.md)); si attiva con `packages: audio: !include audio-es8311.yaml`
