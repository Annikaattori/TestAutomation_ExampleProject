# Tiede ja Teknologia -julkaisujen automaattinen yhteenveto

Tämä repository sisältää Python-pohjaisen automaation, joka hakee
[tiede ja teknologia -ryhmän](https://www.avoindata.fi/data/fi/group/tiede-ja-teknologia)
julkaisut avoindata.fi -palvelusta, lataa PDF- ja PowerPoint-tiedostot, tuottaa niistä
tekoälyllä suomenkielisen yhteenvedon ja tallentaa yhteenvedon PDF-muodossa.
Automaatiota ajetaan GitHub Action -workflow'lla, joka voidaan ajastaa tai
käynnistää käsin.

## Käyttöönotto

1. Luo OpenAI API -avain ja lisää se repositoryn salaisuuksiin nimellä
   `OPENAI_API_KEY` (`Settings` → `Secrets and variables` → `Actions`).
2. (Valinnainen) Jos haluat käyttää jotain toista mallia kuin oletuksena olevaa
   `gpt-4o-mini`:ä, lisää repositoryyn tai workflow'n ympäristömuuttuja
   `OPENAI_SUMMARY_MODEL`.
3. Suorita automaatio paikallisesti komennolla:

   ```bash
   pip install -r requirements.txt
   OPENAI_API_KEY=your-key-here python -m automation.summarize_publications --verbose --limit 1
   ```

   `--limit`-argumentilla voi testata prosessointia ilman, että kaikkia julkaisuja
   käydään läpi.

4. Workflow on määritelty tiedostossa `.github/workflows/summarize.yml`. Se
   käynnistyy joka päivä klo 05:00 UTC sekä manuaalisesti `workflow_dispatch`-toiminnolla.

## Tiedostorakenne

- `automation/summarize_publications.py` – pääskripti, joka huolehtii datan
  hakemisesta, tekoälytiivistelmän muodostamisesta ja PDF-tiedostojen kirjoittamisesta.
- `data/processed_resources.json` – pitää kirjaa jo käsitellyistä resursseista,
  jotta sama julkaisu ei prosessoidu uudelleen.
- `summaries/` – hakemisto, johon tuotetut yhteenvedot tallennetaan.
- `.github/workflows/summarize.yml` – GitHub Action -workflow, joka asentaa
  riippuvuudet ja ajaa skriptin ajastetusti.

## Vinkkejä jatkokehitykseen

- Lisää yksikkötestejä, jotka validoivat esimerkiksi tekstin esikäsittelyä ja
  PDF:n tuottamista.
- Laajenna toimintaa muihin avoindata.fi -ryhmiin lisäämällä uusia
  workflow-steppejä tai tukemalla useita ryhmä-ID:tä kerralla.
- Toteuta virhelokitukset ja ilmoitukset esimerkiksi Slackiin tai sähköpostiin,
  jotta mahdolliset ladattavien tiedostojen virheet huomataan nopeasti.
