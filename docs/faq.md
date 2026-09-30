# Domande frequenti

**Il telefono funziona ancora come citofono se il WiFi non c'è o Home Assistant è spento?**
Sì. Squillo, apertura con il disco e conversazione con la cornetta funzionano in locale. Il WiFi serve solo per notifiche, apertura remota e storico.

**E se la scheda è spenta o guasta?**
La conversazione funziona lo stesso: l'audio passa in analogico dai ponticelli JP1/JP2 e il gancio accende la fonia come nel citofono originale. Non si sente la chiamata e non si apre il portone finché la scheda non torna in funzione.

**La scheda registra o trasmette l'audio?**
No. Nella scheda principale l'audio passa in analogico e l'ESP32 non lo riceve. Ascolto e risposta da remoto richiedono la [scheda audio](../hardware/audio-board/README.md), ancora in sviluppo, e saranno sempre funzioni da attivare esplicitamente.

**Posso usarla con un citofono di un'altra marca?**
Se è un 4+N analogico, probabilmente sì, ma serve identificare i morsetti con il tester. Vedi [compatibilità](compatibilita.md).

**Serve per forza un alimentatore?**
Sì. Il citofono 4+N non alimenta il posto interno: la tensione sulla linea di chiamata c'è solo mentre qualcuno suona. Si può usare un alimentatore 12 V su J1 oppure un caricatore USB-C da 5 V.

**Suonano i campanelli originali dell'S62?**
Di serie suona il buzzer della scheda. I campanelli originali si possono collegare a J6 chiudendo JP3, ma è sperimentale: servono molta più potenza di quella che molti impianti danno sulla linea di chiamata.

**Posso aprire il portone con un codice invece che con un numero qualsiasi?**
Sì: imposta il "Codice rotella" (fino a 8 cifre) da Home Assistant o dalla pagina web.

**Posso comprare la scheda già fatta?**
Per ora no: il progetto è solo fai-da-te e si ordina da JLCPCB con i file della repository. Se ci sarà interesse, potrebbe arrivare una versione più economica già pronta.

**Posso modificare il progetto e venderlo?**
Sì, rispettando le [licenze](../LICENSE.md). L'hardware è CERN-OHL-S: chi distribuisce schede modificate deve pubblicare anche i sorgenti delle modifiche con la stessa licenza.

**Il guscio o il telefono vengono modificati?**
No. La scheda sostituisce quella originale sugli stessi supporti e con le stesse viti. Conserva la scheda originale: l'operazione è reversibile.
