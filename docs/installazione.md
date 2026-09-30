# Installazione nel telefono e collegamento al citofono

> Prima di tutto carica il firmware e fai le prove "da banco" del [collaudo](collaudo.md), passi 1–7, con la scheda **fuori dal telefono**. È molto più comodo trovare un problema adesso che dentro il guscio.

## Mappa dei morsetti

![Mappa dei morsetti della scheda rev 0.5](img/mappa-morsetti.png)

| Morsetto | Pin | Collegamento |
|---|---|---|
| **J1** alimentazione | 1-2 | 9–36 V DC o 9–24 V AC, polarità indifferente |
| **J2** USB-C | | Programmazione, log e alimentazione a 5 V |
| **J3** disco | 1-2 | Contatto impulsi **cid**: chiuso a riposo, si apre a ogni impulso |
| | 3-4 | Contatto rotazione **cld**: chiuso mentre il disco torna indietro |
| **J4** cornetta e gancio | 1-2 | **HS2-HS1:** contatto del gancio usato per sapere se la cornetta è alzata |
| | 3 | **M/R:** filo comune della cornetta |
| | 4-5 | **HK1-HK2:** contatto del gancio che accende la fonia, chiuso a cornetta alzata |
| | 6 | **R:** capsula ricevente |
| | 7 | **M:** microfono |
| **J5** citofono | 1, 2, 6, 9, CA | Ai fili dell'impianto con lo stesso nome (vedi [compatibilità](compatibilita.md)) |
| **J6** suoneria (opz.) | 1-2 | Campanelli originali S62, attivi solo con JP3 chiuso |
| **J7** debug | TX, RX, GND | UART 3,3 V, facoltativo |
| **J8/J9/J10** | | Scheda audio (facoltativa) |

Il pin 1 di ogni connettore ha la **piazzola quadrata**. Guardando la scheda dal lato componenti con J4 e J5 in alto, il pin 1 di J1, J4 e J5 è a sinistra, mentre per J3 e J6 (sul lato destro) è quello più in basso.

## 1. Smontare il telefono

1. Svita la vite del guscio e sfila il coperchio.
2. **Fotografa tutto prima di staccare qualcosa:** quale filo va su quale morsetto e il colore dei fili del disco, del gancio e del cordone della cornetta.
3. Stacca i fili dalla vecchia morsettiera e svita la scheda originale. Conservala: se un giorno vuoi tornare indietro, tutto è reversibile.

## 2. Identificare i contatti con il tester

I colori dei fili variano fra un esemplare e l'altro, quindi conviene misurare con il tester in continuità (il "bip").

**Disco (4 fili):**
- **cid** è la coppia **chiusa a riposo** che "sfarfalla" (apre e chiude) mentre il disco torna indietro. Va su **J3 1-2**.
- **cld** è la coppia **aperta a riposo** che si chiude appena giri il disco e si riapre quando è tornato a riposo. Va su **J3 3-4**.
- J3 1 e J3 3 sono entrambi a massa. Se un filo è in comune fra i due contatti, mettilo su 1 e ponticella 1 con 3.

**Gancio:** il commutatore del gancio ha diversi contatti. Servono due contatti **che si chiudono quando la cornetta è alzata** e che non abbiano fili in comune fra loro:
- il primo va su **J4 4-5** (HK1-HK2) e accende la fonia;
- il secondo va su **J4 1-2** (HS2-HS1) e informa l'ESP32.

  Se il secondo contatto disponibile è invece **aperto** a cornetta alzata, va bene lo stesso: al collaudo si inverte la logica nel firmware con una riga (`inverted`).

**Cornetta (cordone a 3 fili):**
- il filo **comune** va su **J4 3** (M/R);
- la **capsula** va su **J4 6** (R);
- il **microfono** va su **J4 7** (M).

Per distinguerli: la capsula ha una resistenza bassa, qualche decina o centinaio di ohm, fra il suo filo e il comune.

## 3. Montare la scheda

1. Avvita la scheda sui due perni originali con le viti originali (fori Ø 3 mm).
2. Collega disco, gancio e cornetta come identificato sopra. Spella circa 6 mm. Se i fili sono a trecciola molto sottile, stagnali prima o usa i puntalini.
3. Fai passare i fili dell'impianto e dell'alimentazione dal passacavo originale.

## 4. Alimentazione

Il citofono 4+N non alimenta il posto interno. Ci sono tre possibilità:

| Soluzione | Quando conviene |
|---|---|
| **Alimentatore 12 V DC da 1 A** su J1 | Soluzione tipica: una presa vicina e due fili in più nel tubo del citofono |
| **12 V AC** dal trasformatore dell'impianto su J1 | Solo se i morsetti del trasformatore sono raggiungibili e hai il permesso di usarli |
| **Caricatore USB-C 5 V** | Il più semplice, anche per l'uso quotidiano, ma serve un cavo USB che esca dal telefono |

## 5. Collegare il citofono (J5)

1. **Stacca l'alimentazione della scheda.**
2. Identifica i fili dell'impianto come spiegato in [compatibilità](compatibilita.md).
3. Collega **1, 2, 6, 9, CA** agli stessi morsetti di J5.
4. Richiudi il telefono solo dopo aver fatto i passi 8–11 del [collaudo](collaudo.md).

## 6. Ponticelli

| Ponticello | Di fabbrica | Quando cambiarlo |
|---|---|---|
| **JP1**: linea 1 ↔ capsula | chiuso | Si taglia solo se monti la scheda audio |
| **JP2**: linea 2 ↔ microfono | chiuso | Si taglia solo se monti la scheda audio |
| **JP3**: suoneria J6 ↔ CA | aperto | Si chiude con una goccia di stagno per provare i campanelli originali |
