# Home Assistant, notifiche e assistenti vocali

La scheda usa ESPHome, quindi Home Assistant la trova da solo:

1. dopo il primo avvio compare in **Impostazioni → Dispositivi e servizi** come "Citofono Vintage";
2. clicca su **Configura**;
3. inserisci la chiave `api_encryption_key` che hai scritto in `secrets.yaml`.

## Entità disponibili

| Entità | Tipo | A cosa serve |
|---|---|---|
| **Apri portone** | pulsante | Apre il portone, con lo sgancio simulato se attivo |
| **Campanello** | evento (doorbell) | Tipo `chiamata` quando suonano, `apertura` quando si apre |
| **Chiamata in corso** | sensore binario | ON finché dura la chiamata |
| **Cornetta alzata** | sensore binario | |
| **Disco in rotazione** | sensore binario | |
| **Ultimo numero composto** | testo | Ultima cifra del disco |
| **Ultima chiamata / Ultima apertura** | testo | Data e ora |
| **Chiamate ricevute** | contatore | |
| **Non disturbare** | interruttore | Silenzia lo squillo |
| **Apertura automatica alla chiamata** | interruttore | Si spegne da sola al riavvio |
| **Apertura remota con sgancio simulato** | configurazione | Attiva K2 prima di aprire |
| **Rotella apre solo a cornetta alzata** | configurazione | |
| **Durata apertura portone** | configurazione | Da 300 a 4000 ms |
| **Codice rotella** | configurazione | Codice segreto (vuoto = qualsiasi numero apre) |
| **Test suoneria**, **Riavvia**, **Segnale WiFi**, **Uptime** | diagnostica | |

I nomi completi delle entità dipendono dal nome del dispositivo, per esempio `button.citofono_vintage_apri_portone`.

## Notifica con il pulsante "Apri"

Nel file [`home-assistant/automazioni-citofono.yaml`](../home-assistant/automazioni-citofono.yaml) ci sono due automazioni pronte:

1. **Notifica:** quando suonano, arriva sul telefono una notifica con il pulsante **"Apri il portone"**.
2. **Apertura:** toccando il pulsante nella notifica il portone si apre, e arriva la conferma "Portone aperto".

**Come usarle:**

1. Installa l'app **Home Assistant** sul telefono (iOS o Android) ed effettua l'accesso.
2. In Home Assistant: **Impostazioni → Automazioni → Crea automazione → ⋮ → Modifica in YAML**, e incolla una automazione alla volta.
3. Sostituisci `mobile_app_tuo_telefono` con il nome del tuo telefono. Lo trovi in **Strumenti per sviluppatori → Azioni**, cercando "notify.mobile_app".

## Google Home, Apple Home, Alexa

Si collegano **attraverso Home Assistant**. La scheda non deve fare nulla di speciale.

| Assistente | Integrazione di Home Assistant |
|---|---|
| Apple Home / Siri | **HomeKit Bridge** |
| Google Home | **Google Assistant**, tramite Home Assistant Cloud (Nabu Casa) o configurazione manuale |
| Alexa | **Amazon Alexa**, tramite Home Assistant Cloud o configurazione manuale |

Il modo più semplice e compatibile con tutti e tre:
1. crea in Home Assistant uno **script** "Apri portone" che preme il pulsante `button.citofono_vintage_apri_portone`;
2. esponi lo script all'assistente, che lo vede come una scena da attivare.

Per esempio: "Ok Google, attiva Apri portone".

> **Sicurezza:** uno script esposto come scena **non chiede conferme né PIN**: chiunque possa parlare all'assistente può aprire il portone. Valuta se esporlo, oppure esponi solo lo stato del campanello.

## Senza Home Assistant

La scheda ha una **pagina web locale** su `http://citofono-vintage.local`, oppure sull'indirizzo IP che le assegna il router.

- Si entra con utente `admin` e con la password `web_password` di `secrets.yaml`.
- Dalla pagina si possono aprire il portone, vedere lo stato e cambiare le impostazioni.
