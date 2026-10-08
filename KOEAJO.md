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


