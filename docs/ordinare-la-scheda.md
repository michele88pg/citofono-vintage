# Ordinare e montare la scheda

La scheda si ordina **già assemblata** da [JLCPCB](https://jlcpcb.com): arrivano PCB e componenti SMD saldati. A mano restano solo il relè K1 e le morsettiere, tutti a foro passante e facili da saldare.

I file pronti sono in [`hardware/mainboard/production`](../hardware/mainboard/production):

| File | Cosa contiene |
|---|---|
| `CitofonoVintage_GERBER_JLCPCB.zip` | Gerber e forature del PCB |
| `BOM_JLCPCB.csv` | Distinta componenti con i codici **LCSC Part#** |
| `CPL_JLCPCB.csv` | Posizione e rotazione dei componenti (coordinate del centro del corpo) |

## Ordine su JLCPCB, passo passo

1. **Carica il Gerber:** "Order now" e poi "Add gerber file" con lo zip. Le opzioni da scegliere:

   | Opzione | Valore |
   |---|---|
   | Base material | FR-4 |
   | Layers | 2 |
   | Dimensions | le rileva da solo, circa 81 × 85 mm |
   | PCB Qty | 5, il minimo |
   | PCB Thickness | 1,6 mm |
   | Surface finish | HASL lead-free oppure ENIG |
   | Mark on PCB | "Remove Mark", se disponibile |

   Il resto va lasciato di default.

2. **Attiva "PCB Assembly":**
   - lato **Top**;
   - **Standard** e non Economic: il modulo ESP32-S3 si può montare solo con l'assemblaggio Standard;
   - quantità: 2 oppure 5.

3. **BOM e CPL:** carica `BOM_JLCPCB.csv` e `CPL_JLCPCB.csv`.
   - Se il sito rifiuta i CSV ("File processing failed"), aprili e salvali in formato **XLSX**, poi ricaricali.
4. **Controlla la lista componenti.** Alcune voci risultano senza codice, ed è voluto: sono le parti da saldare a mano, elencate sotto. Deselezionale.
   - Se un componente è esaurito, cercane uno equivalente con **lo stesso footprint**.
5. **Controlla l'anteprima di posizionamento.** Ogni componente deve stare sulla sua impronta, con il pin 1 nel verso giusto.
   - Attenzione a U3 (ESP32), J2 (USB-C), OK1, BZ1, diodi e condensatori elettrolitici.
   - Se un pezzo è ruotato o spostato, correggilo nell'anteprima: JLCPCB fa comunque una verifica manuale (DFM) prima di produrre.
6. Conferma e paga. Il primo ordine di prototipi (5 schede assemblate) è costato circa 195 USD, spedizione compresa.

## Componenti da saldare a mano

| Rif | Cosa | Dove comprarlo | Note |
|---|---|---|---|
| **K1** | PhotoMOS DIP-6 a foro passante: **TLP3546A(F)** (Toshiba) oppure **AQV252G** (Panasonic) | DigiKey, Mouser | Su LCSC c'è solo la versione SMD, che non entra |
| **J1, J6** | Morsettiera 2 poli, passo **5,0 mm** | LCSC C474950 (KF128-5.0-2P) | |
| **J3** | Morsettiera 4 poli, passo 5,0 mm | 2× C474950 | I KF128 si incastrano di fianco mantenendo il passo |
| **J5** | Morsettiera 5 poli, passo 5,0 mm | C474950 + C474951 (KF128-5.0-3P) | |
| **J4** | Morsettiera 7 poli, passo 5,0 mm | 2× C474950 + 1× C474951 | Incastrale fra loro prima di infilarle |
| **J7, J8, J9, J10** | Pin header maschio 2,54 mm | C2337 (strip da 40 da spezzare) | Opzionali: servono per il debug e la scheda audio |

> **Attenzione al passo:** servono morsettiere a **5,0 mm**, non 5,08 mm. Su 7 poli la differenza si accumula a quasi mezzo millimetro e i pin non entrano nei fori.

**Per 5 schede:** 5× K1, 25× C474950, 10× C474951, 1× C2337.

## Montaggio

1. **K1:** rispetta il verso. Il pin 1 (tacca o puntino) va verso il segno sulla serigrafia. Salda due piedini opposti, controlla che sia appoggiato alla scheda, poi salda gli altri.
2. **Morsettiere:** montale con l'ingresso dei fili verso il bordo della scheda. Tienile premute mentre saldi il primo pin di ognuna.
3. **Header:** J8/J9/J10 solo se monterai la scheda audio; J7 solo per il debug seriale.
4. **Controllo prima di alimentare:**
   - con il tester, fra +5V e GND e fra +3V3 e GND **non deve esserci un corto**. Le piazzole sono accessibili su C6/C7;
   - JP1 e JP2 devono essere chiusi (ponticello di stagno di fabbrica);
   - JP3 deve essere aperto.

Poi passa al [firmware](../firmware/README.md).

## Modificare il progetto

Il progetto è in KiCad, nella cartella [`hardware/mainboard`](../hardware/mainboard):
- schema: `CitofonoVintage.kicad_sch`;
- circuito stampato: `CitofonoVintage.kicad_pcb`;
- regole e classi di rete: `CitofonoVintage.kicad_pro`.

I file di produzione si rigenerano con il plugin **Fabrication Toolkit** (Bennymeg) di KiCad, che produce direttamente Gerber, BOM e CPL nel formato di JLCPCB. Il codice LCSC di ogni componente è nel campo **"LCSC Part#"** dello schema.
