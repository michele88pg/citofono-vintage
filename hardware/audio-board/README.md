# Scheda audio ES8311 (rev 0.1)

> **Stato:** solo schema. Il PCB non è ancora disegnato e la scheda non è ancora stata provata.

La scheda principale rev 0.5 apre il portone e rileva le chiamate, ma l'audio del citofono non passa dall'ESP32. Questa schedina si collega ai connettori già previsti **J8/J9/J10** e aggiunge due funzioni:

- **Ascolto e registrazione.** La voce che arriva dal posto esterno (linea 1) entra nel codec e l'ESP32 la riceve come un microfono.
- **Risposta da remoto.** La voce dal telefono esce dal codec e viene mandata sulla linea 2 al posto esterno. La cornetta resta esclusa.

Lo schema è **completo e verificato rete per rete** (netlist esportata e controllata). Il PCB conviene disegnarlo dopo il collaudo della scheda principale: vedi "Prima di ordinare".

## File

| File | Contenuto |
|---|---|
| `CitofonoAudio.kicad_sch` | Schema (formato KiCad 7, si apre in KiCad 10 che lo aggiorna) |
| `CitofonoAudio.kicad_sym` + `sym-lib-table` | Libreria di progetto: simboli ES8311 e SM-LP-5001 |
| `CitofonoAudio_schematic.pdf` | Schema stampabile |
| `BOM_AudioBoard.csv` | Distinta con i codici LCSC dei componenti principali; i campi vuoti vanno completati prima dell'ordine |

## Come funziona

```
LINEA URMET                          |  LATO LOGICA (GND scheda)
                                     |
linea 1 --C11--R4--[T1 600:600]------|--C12/C13--> MIC1P/N  ES8311 --I2S--> ESP32
          (presa ad alta impedenza)  |
linea 2 --K1 NC--> microfono cornetta|
        --K1 NA--C16--R8--[T2]-------|--R6/R7--C14/C15-- OUTP/N ES8311 <--I2S-- ESP32
linea 1 --K1 NC--> capsula cornetta  |
                                     |  TCA9554 (I2C 0x20) P0 -> Q1 -> bobina K1
```

- **Relè K1 a riposo, quindi scheda spenta o firmware in avvio:** linea e cornetta sono collegate come prima, il citofono funziona in analogico. È una scelta fail-safe.
- **Relè K1 eccitato ("Rispondi da remoto"):** la linea 2 riceve la voce dal codec e la cornetta viene esclusa. L'ascolto (T1) è sempre collegato, quindi si può registrare anche quando qualcuno usa la cornetta.
- **Isolamento:** trasformatori 600:600 da 2000 Vrms e contatti del relè. La massa della schedina è la stessa della logica della scheda principale, già isolata dalla linea.
- **Perché il TCA9554:** J8 non ha GPIO liberi. Il relè si comanda sullo stesso bus I2C del codec.

## Collegamento alla scheda principale

1. **Taglia JP1 e JP2** sulla scheda principale (il ponticello di stagno tra i due pad, con un cutter). Da quel momento è K1 della schedina a collegare linea e cornetta.
   Se togli la schedina devi richiudere JP1/JP2 con una goccia di stagno, altrimenti la cornetta resta muta.
2. Collega con cavetti Dupont femmina-femmina, **il più corti possibile (max 15 cm)**, perché MCLK è un clock a 4 MHz:
   - J1 → J8, pin 1 su pin 1 (stesso ordine)
   - J2 → J9
   - J3 → J10

## Prima di ordinare (in quest'ordine)

1. **Collaudo della scheda principale**, in particolare la prova 9 del [collaudo](../../docs/collaudo.md) (audio passante). Durante una chiamata misura con il multimetro in AC e in DC:
   - tra **1 e 6**, per sapere quanto segnale arriva (serve a confermare R4);
   - tra **2 e 6**, per sapere se c'è una tensione continua sul microfono. Se c'è, probabilmente serve montare **R9**, vedi nota nello schema.
2. **Simbolo e footprint del trasformatore.** KiCad non ha il footprint del Bourns SM-LP-5001. Sul tuo computer:
   ```
   pip install easyeda2kicad
   easyeda2kicad --full --lcsc_id=C7503474
   ```
   Poi assegna il footprint a T1 e T2. Il mio simbolo assume il **primario sui pin 1-3 e il secondario sui pin 4-6, con 2 e 5 non collegati**. Non ho potuto leggere il datasheet Bourns da qui (sito bloccato), quindi confrontalo con lo schema del datasheet o con il simbolo scaricato da easyeda2kicad. Se i numeri sono diversi, cambia solo i numeri dei pin nel simbolo: le reti restano le stesse.
3. **PCB.** Non ha vincoli di forma: va dentro il guscio del S62, dove c'è spazio. Uno spunto è 50×40 mm a 2 strati, con queste regole:
   - Condensatori di disaccoppiamento del codec attaccati ai pin. Il pad centrale (21) va a GND con 4-5 vias.
   - **Barriera di isolamento** come sulla scheda principale, larga almeno 2 mm, tra il lato linea e il lato logica:
     - lato linea: J2, J3, C11, R4, C16, R8, R9, i contatti di K1, i primari di T1 e i secondari di T2;
     - lato logica: tutto il resto;
     - T1, T2 e K1 stanno a cavallo della barriera.
   - Piano GND solo sul lato logica.
   - J1 vicino al bordo, per cavetti corti.

## Firmware

Nella cartella [`firmware`](../../firmware) c'è [`audio-es8311.yaml`](../../firmware/audio-es8311.yaml), un pacchetto ESPHome **validato con ESPHome 2026.6**. Si attiva aggiungendo tre righe a `citofono-vintage.yaml`:

```yaml
packages:
  audio: !include audio-es8311.yaml
```

Aggiunge queste entità:
- **"Rispondi da remoto (audio)"**: attiva K2 (sgancio simulato) e K1 audio, e si spegne da sola dopo 2 minuti;
- **"Volume voce verso la strada"**;
- il microfono `citofono_mic` e l'altoparlante `citofono_spk`.

**Cosa manca:** il "ponte" verso il telefono, cioè il pezzo che porta `citofono_mic`/`citofono_spk` in una chiamata sul cellulare. Il candidato è il componente della community *esphome-intercom* (SIP/RTP, integrato con Home Assistant). Lo configuriamo quando la schedina esiste, perché va provato sull'hardware vero.

## Punti da verificare al collaudo

- **Livello di ascolto:** `mic_gain` in `audio-es8311.yaml` (parte da 24 dB).
- **Livello di parlato:** prima il volume da Home Assistant. Se non basta, cambia R6/R7 (470 Ω) o R8 (1 kΩ).
- **Presenza di tensione continua su linea 2**, che decide se montare R9.
- **Pinout del trasformatore** (vedi sopra).
