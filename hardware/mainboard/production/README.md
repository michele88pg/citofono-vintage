# File di produzione (JLCPCB)

| File | Contenuto |
|---|---|
| `CitofonoVintage_GERBER_JLCPCB.zip` | Gerber e forature, da caricare in "Add gerber file" |
| `BOM_JLCPCB.csv` | Distinta con i codici LCSC Part# |
| `CPL_JLCPCB.csv` | Posizionamento: Designator, Mid X, Mid Y, Rotation, Layer. Le coordinate sono del **centro del corpo** di ogni componente |

La procedura passo passo è in [docs/ordinare-la-scheda.md](../../../docs/ordinare-la-scheda.md).

**Note dal primo ordine:**
- Se il caricamento dei CSV dà "File processing failed", salva i file in formato **XLSX** e ricaricali.
- Il modulo ESP32-S3 richiede l'assemblaggio **Standard**, non Economic.
- K1 e le morsettiere non sono nella BOM di assemblaggio: vanno saldati a mano.

Questi file corrispondono alla revisione **0.5**. Se modifichi il progetto, rigenerali da KiCad con il plugin **Fabrication Toolkit**.
