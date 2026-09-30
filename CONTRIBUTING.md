# Contribuire

Grazie dell'interesse! Il progetto è giovane e ogni aiuto conta. In ordine di utilità:

1. **Segnalazioni di compatibilità.** Hai provato la scheda, o anche solo misurato i morsetti, su un impianto o un telefono diverso? Apri una [segnalazione di compatibilità](../../issues/new?template=compatibilita.md). Anche un "non funziona" è prezioso, se contiene marca, modello e misure.
2. **Foto e guide di montaggio.** Foto nitide del cablaggio dentro l'S62 aiutano moltissimo chi inizia.
3. **Correzioni alla documentazione e traduzioni**, soprattutto in inglese.
4. **Modifiche a hardware e firmware** tramite pull request.

## Pull request

- **Hardware:**
  - modifica i file con **KiCad 10** o una versione successiva;
  - lancia ERC e DRC prima di proporre la modifica;
  - spiega nella descrizione cosa cambia elettricamente.
- **Firmware:**
  - verifica che la configurazione passi `esphome config firmware/citofono-vintage.yaml`;
  - non includere mai il tuo `secrets.yaml`.
- **Una modifica per pull request.** Se tocchi più cose, aprine più di una.

## Licenze dei contributi

Inviando un contributo accetti che venga rilasciato con la licenza della parte del progetto che modifica: CERN-OHL-S-2.0 per l'hardware, MIT per il firmware, CC BY-SA 4.0 per la documentazione. Vedi [LICENSE.md](LICENSE.md).
