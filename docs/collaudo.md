# Collaudo

Le prove vanno fatte **in quest'ordine**. Ogni passo usa solo cose già verificate nei passi precedenti, così un problema salta fuori subito e si capisce da dove viene.

**Cosa serve:**
- un tester;
- il firmware già caricato (vedi [firmware/README.md](../firmware/README.md));
- il log aperto sul computer: `esphome logs citofono-vintage.yaml`.

## Prove da banco (scheda fuori dal telefono, non collegata al citofono)

| # | Prova | Risultato atteso | Se non va |
|---|---|---|---|
| 1 | Alimenta da USB-C | LED e log attivi, WiFi connesso | Misura 5 V su C6 e 3,3 V su C7 |
| 2 | Alimenta da J1 con 12 V (AC o DC) | Come sopra | Controlla il ponte D1, il fusibile F1 e il buck U1 (5 V su C4) |
| 3 | Collega il disco a J3 e componi "3" | Log: `Composta cifra 3 (3 impulsi)` | Se la cifra è sbagliata, vedi la nota sul disco più sotto |
| 4 | Componi "0" | `Composta cifra 0 (10 impulsi)` | |
| 5 | Collega il gancio a J4 1-2, alza e abbassa la cornetta | "Cornetta alzata" ON/OFF | Se lo stato è al contrario, cambia `inverted` del sensore `handset` nel firmware |
| 6 | Premi il pulsante BOOT (SW2) | "Apertura portone" nel log e **continuità fra J5-9 e J5-6** per 1,5 s | R15, K1 (verso di montaggio!) |
| 7 | Premi "Test suoneria" da Home Assistant o dalla pagina web | Il buzzer suona 3 doppi squilli | Q1, R17, BZ1 |

## Prove sull'impianto

| # | Prova | Risultato atteso | Se non va |
|---|---|---|---|
| 8 | Collega J5 al citofono e fai suonare dal portone | "Chiamata in corso" ON, squillo, evento "Campanello" in Home Assistant | Misura circa 12 V AC fra 6 e CA; controlla OK1, D5, R14 |
| 9 | Alza la cornetta durante una chiamata e parla | Audio nei due sensi, come con il citofono originale | JP1/JP2 devono essere chiusi; controlla il cablaggio di J4 3, 6, 7 e il contatto HK1-HK2 |
| 10 | Componi un numero a cornetta alzata | Il portone si apre | Aumenta "Durata apertura portone" |
| 11 | "Apri portone" da Home Assistant a cornetta abbassata | Il portone si apre | Prova ad attivare o disattivare "Apertura remota con sgancio simulato" |
| 12 | Componi un numero a cornetta **abbassata** | Il portone si apre (se non hai attivato "Rotella apre solo a cornetta alzata") | Alcuni impianti accettano l'apriporta solo con la fonia attiva: in quel caso attiva l'opzione e apri a cornetta alzata |

## Note

- **Disco.** Il firmware conta gli impulsi del contatto `cid` (J3 1-2) e chiude la cifra quando il contatto `cld` (J3 3-4) segnala il ritorno del disco, oppure dopo 300 ms senza impulsi.
  - Se le cifre risultano sbagliate di ±1, il problema è quasi sempre il filtro anti-rimbalzo (`delayed_on/off: 8ms` del sensore `dial_pulse`): prova 5 ms oppure 12 ms.
  - Se il disco è molto lento o molto veloce, fallo revisionare: deve fare circa 10 impulsi al secondo.
- **Sgancio simulato.** Se il passo 11 funziona anche con l'opzione disattivata, lasciala spenta: l'apertura remota è più rapida e la linea condominiale non viene impegnata.
- **Durata dell'apertura.** 1,5 s va bene per la maggior parte delle serrature elettriche; alcune vogliono 2–3 s.

## Misure da annotare (aiutano tutti)

Se fai il collaudo su un impianto nuovo, annota queste misure e condividile in una [segnalazione di compatibilità](../../../issues/new?template=compatibilita.md):

| Misura | Come | Tester |
|---|---|---|
| Tensione di chiamata | Fra CA e 6 mentre suonano | V AC |
| Tensione su 9 a riposo | Fra 9 e 6, cornetta abbassata | V DC e V AC |
| Tensione sulla fonia | Fra 1 e 6 e fra 2 e 6, cornetta alzata, durante una chiamata | V DC e V AC |
| Apertura a cornetta abbassata | Passo 12 | sì / no |
| Serve lo sgancio simulato | Passo 11 | sì / no |
