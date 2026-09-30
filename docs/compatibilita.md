# Compatibilità

Questa tabella viene aggiornata con le prove della community. Se provi la scheda su un impianto o su un telefono che non è elencato, [apri una segnalazione di compatibilità](../../../issues/new?template=compatibilita.md), anche se non funziona: serve anche quella.

## Telefoni

| Telefono | Stato | Note |
|---|---|---|
| **Siemens S62 da parete** | ✅ Progettata su questo | Forma, fori e posizione dei morsetti rilevati su un esemplare reale |
| **Italtel S62 da parete** | ✅ Atteso compatibile | Stesso telefono: dagli anni '80 lo stesso modello è stato prodotto e marchiato Italtel |
| Siemens / Italtel S62 da tavolo | ❓ Da verificare | L'elettronica è la stessa, ma la scheda e il suo fissaggio potrebbero essere diversi. Se ne hai uno, misura la scheda e confrontala con il [DXF](../hardware/mechanical/S62_board_outline.dxf) |
| Altri telefoni a disco (Face, Bobo, Grillo…) | ❌ Non adatta come forma | L'elettronica sarebbe riutilizzabile, ma la scheda non entra. Serve un nuovo contorno del PCB: contributi benvenuti |

**Cosa deve funzionare nel telefono:**
- il **disco**, che deve tornare indietro regolare e senza impuntarsi;
- il **gancio** con almeno due contatti;
- la **cornetta**, con capsula e microfono.

La vecchia scheda e le vecchie morsettiere vengono tolte.

## Impianti citofonici

| Impianto | Stato | Note |
|---|---|---|
| **URMET 4+N analogico** (posto esterno con scheda CS 1133) | 🔧 In collaudo | Impianto di sviluppo. Morsetti confermati con misure sul campo: 1, 2, 6, 9, CA |
| Altri URMET 4+N analogici | ❓ Probabile | Stessa logica di morsetti; verifica con il tester (vedi sotto) |
| Altri 4+N analogici (BTicino, Comelit, Elvox, Farfisa, Terraneo…) | ❓ Possibile | Il principio è identico (chiamata in AC, apriporta a contatto, fonia su due fili), ma **la numerazione dei morsetti è diversa**. Serve identificare i fili con il tester |
| Impianti digitali o a 2 fili (URMET 2Voice, BTicino 2 fili, Comelit Simplebus…) | ❌ Non supportati | Usano un bus digitale: serve un'interfaccia diversa |
| Videocitofoni | ❌ Non supportati | |

### Come riconoscere il tuo impianto

1. **Guarda il citofono che hai in casa.** Un 4+N analogico ha di solito **da 4 a 6 fili** su una morsettiera con numeri (1, 2, 6, 9…) o sigle.
   - Se hai solo 2 fili, è quasi certamente un impianto digitale.
   - Marca e modello sono spesso stampati dentro, sul retro della cornetta o sulla scheda.
2. **Con il tester, mentre qualcuno suona dal portone,** cerca la coppia di morsetti su cui compare una **tensione alternata di 8–15 V**. Quelli sono **CA e 6**, cioè la chiamata e il comune.
3. **L'apriporta** è la coppia che, chiusa con un filo, fa scattare la serratura. Sul citofono originale sono i due contatti del pulsante a chiave. Su URMET sono **9 e 6**.
4. **La fonia** sono i due fili che vanno alla capsula (1) e al microfono (2) della cornetta del citofono originale.

Se i tuoi morsetti hanno nomi diversi, basta collegarli alle funzioni giuste su J5. Il firmware non cambia.

> Le misure prese direttamente sull'impianto sono più affidabili di qualsiasi identificazione fatta dalle foto: in fase di progetto l'impianto era stato scambiato per un Comelit, finché le misure non hanno dimostrato che era un URMET.
