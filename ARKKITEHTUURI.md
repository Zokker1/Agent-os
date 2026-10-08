# Agent OS:n arkkitehtuuritiivistelmä

Loogiset kerrokset ovat erillisiä, mutta API, sovelluspalvelut, agenttimoottori ja ajastus toimivat yhdessä paikallisessa Rust-taustapalvelussa. Työpöytäkuoren toteutusta ei ajettu tässä työnäytteessä; koeajo tehtiin selainkäyttöliittymällä.

```mermaid
flowchart TB
    UI["Yhteinen käyttöliittymä<br/>React + TypeScript<br/>Selain / Tauri-työpöytäkuori"]
    subgraph RUST["Paikallinen Rust-taustapalvelu — yksi prosessi"]
        API["Loopback HTTP API<br/>ja tapahtumien välitys"]
        SERVICES["Sovelluspalvelut<br/>projektit, keskustelut, ajot ja ajastus"]
        HARNESS["Agenttimoottori<br/>konteksti, mallikierrokset ja ajon ohjaus"]
        POLICY["Käyttöoikeudet ja hyväksynnät<br/>tarkistetaan mallin ulkopuolella"]
        TOOLS["Työkalusovittimet<br/>tiedostot, haku, Git ja komennot"]
        API --> SERVICES
        SERVICES --> HARNESS
        HARNESS --> POLICY --> TOOLS
        TOOLS -->|"havaittu tulos"| HARNESS
    end
    UI --> API
    PROVIDERS["Mallipalvelujen sovittimet<br/>Koeajo: OpenRouter → DeepSeek"]
    STORAGE["Paikallinen pysyvä tila<br/>SQLite + tuotostiedostot"]
    FUTURE["Jatkokehityksen suunta<br/>laaja aliagenttiohjaus, MCP,<br/>selain- ja työpöytätyöntekijät"]
    HARNESS <-->|"mallikutsu / vastaus"| PROVIDERS
    SERVICES --> STORAGE
    TOOLS -.-> FUTURE
    classDef future fill:#f4f4f7,stroke:#9b9ca9,stroke-dasharray:7 5;
    class FUTURE future;
```

Kaavio näyttää komponenttien vastuut. Se ei lupaa kaikkien tiekartan ominaisuuksien olevan toteutettuja tai tässä koeajossa testattuja. Paikallisen komentotulkin käyttö ei itsessään ole käyttöjärjestelmätason hiekkalaatikko.
