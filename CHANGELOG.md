# Changelog

## Scheda principale

### rev 0.5 (settembre 2026): primi prototipi
- Ordinati 5 prototipi assemblati da JLCPCB (29/09/2026). **Collaudo in corso.**
- Modulo ESP32-S3-WROOM-1-**N16R2** (PSRAM quad).
- K1: PhotoMOS DIP-6 a foro passante, TLP3546A(F) o AQV252G, da saldare a mano.
- Codici LCSC su tutti i componenti assemblati.
- Posizionamento dei componenti rivisto e sbroglio completo. DRC: 0 connessioni mancanti, 0 violazioni elettriche.

### rev 0.3 (settembre 2026)
- Schema completo: alimentazione con LM5164 (sostituisce l'LM5165, limitato a 150 mA), interfaccia URMET isolata (PC817, due PhotoMOS), interfaccia S62, ESP32-S3.
- Ordine dei pin di J4 cambiato per far passare la barriera di isolamento: `1=HS2 2=HS1 3=M/R 4=HK1 5=HK2 6=R 7=M`.
- Barriera di isolamento a strisce su entrambi gli strati; J8 spostato sul bordo sinistro.

## Firmware

### 0.5.0 (settembre 2026)
- Prima versione ESPHome, con queste funzioni:
  - apertura con il disco, con codice segreto opzionale;
  - apertura remota con sgancio simulato;
  - rilevamento della chiamata con squillo, evento campanello e storico;
  - "Non disturbare" e apertura automatica;
  - pagina web locale;
  - sicurezze sui relè.
- Pacchetto opzionale `audio-es8311.yaml` per la scheda audio.

## Scheda audio

### rev 0.1 (settembre 2026)
- Schema con codec ES8311, trasformatori di isolamento 600:600, relè fail-safe ed espansore I2C TCA9554. PCB non ancora disegnato.
