# DeepSeek/OpenRouter-koeajo — 8.10.2026

Koeajo tehtiin projektin omalla paikallisella Rust-taustapalvelulla ja selainkäyttöliittymällä. Malli oli `deepseek/deepseek-v4.1-flash`, provider `openrouter`. Vastaukset pyydettiin suomeksi.

## Tehtävä ja havaittu tulos

Syötteenä oli kolmen rivin [demo.csv](demo/demo.csv). Agenttia pyydettiin lukemaan aineisto, kirjoittamaan siitä raportti, odottamaan kirjoituksen hyväksyntää ja lukemaan valmis raportti takaisin.

| Vaihe | Havainto |
| --- | --- |
| Aineiston luku | `files.read` palautti kolme riviä: Suunnittelu 2, Toteutus 5, Tarkistus 3. |
| Hyväksyntä | Ajo pysähtyi odottamaan `files.write`-päätöstä kohteelle `RAPORTTI.md`. |
| Kirjoitus | Hyväksyntä tehtiin käyttöliittymässä. Työkalun tulos ilmoitti uuden 299 tavun tiedoston. |
| Takaisinluku | `files.read` palautti raportin sisällön, mukaan lukien summan 10. |
| Riippumaton tarkistus | CSV:n luvut laskettiin erikseen yhteen ja raportin rivit sekä summa tarkistettiin. |
| Ajon tila | API palautti tilan `completed`. |

Ajon tunniste: `01a11cb5-6be2-74da-aa36-86a235cb4c94`. API:n aikaleimat: luotu `2026-10-08T18:10:14Z`, valmis `2026-10-08T18:11:41Z`. Hyväksynnän odotus sisältyy tähän aikaväliin.

Käyttö-API kirjasi 15 534 syötetokenia, 630 tulostokenia ja kustannukseksi 0,0054162 USD. Tämä on sovelluksen raportoima käyttö, ei erillinen laskutustarkastus. Työkalukutsujen käyttömittari jäi nollaan, vaikka keskustelutallenne sisältää työkalujen tulokset; se on havaittu mittariston puute.

## Rajat ja ensimmäinen yritys

Ensimmäinen yritys erillisessä esittelykansiossa onnistui lukemaan aineiston ja pyytämään hyväksyntää. Kirjoituksen väliaikaistiedoston luonti epäonnistui Windowsin käyttöoikeusvirheeseen. Raporttia tai sen väliaikaistiedostoa ei löytynyt tarkastuksessa; ajo keskeytettiin APIsta. Sitä ei kirjattu onnistumiseksi. Uusi koeajo tehtiin projektin erillisessä väliaikaisessa testihakemistossa ja onnistui.

APIin asetettiin rajat 60 000 tokenia, 0,10 USD raportoituun käyttöön perustuva kustannusraja ja 180 sekuntia. Rajat tarkistetaan mallikierrosten välissä; ne eivät ole yksittäisen mallikutsun tarkka enimmäiskustannus. Molempia yrityksiä valvottiin. Sovelluskoodia ei muutettu koeajoa varten.

Kuvakaappaukset ovat aidosta käyttöliittymästä. Esittely ja koeajon sisältö ovat suomeksi, mutta käyttöliittymän teksteissä on vielä englantia. Selainkonsolissa näkyi myös yksittäisiä API-kyselyjen 504-aikakatkaisuja. Tämän näytteen perusteella ei ole testattu esimerkiksi pitkien ajojen palautumista, kaikkia työkalutyyppejä, työpöytäkuorta tai laajoja aliagenttiajoja.

## Esittelyn lähteet

Rakennetta tarkistettiin suunnitelman kohdista 1–2, `CURRENT.md`:stä, `TASK_LOG.md`:n tuoreista tapahtumista sekä ohjeista `MODULE_BOUNDARIES.md`, `RUNTIME_BOUNDARIES.md`, `DAEMON_RUNTIME.md`, `TRY_WITH_OPENROUTER.md` ja `HARNESS_CRITICAL_PATH.md`. Työskentelyohjeet `AGENTS.md`, `FLASH_WORKFLOW.md`, `AGENT_TESTING.md` ja `VERIFICATION.md` luettiin myös.

Dokumentaation rinnalla tarkistettiin nykyiset manifestit, daemonin reitit, agenttiajon kytkentä ja työkalukatalogi sekä käyttöliittymän todellinen toiminta. Vanhat ohjeet väittävät paikoin, ettei käyttöliittymää ole kytketty tai budjetteja toteutettu; uusi koeajo ja nykyinen koodi osoittavat näiden kuvausten olevan osittain vanhentuneita. Työnäytteessä käytetty paikallinen Git-revisio oli `9be3368`.
