# Virksomhetskartlegger

Et selvdrevet verktøy for strategisk virksomhetskartlegging — kjører direkte i nettleseren uten installasjon, server eller pålogging.

## Kom i gang

1. Last ned eller klon dette repositoriet
2. Åpne `mapper_edit.html` i Chrome, Firefox eller Edge
3. Ferdig — ingen installasjon nødvendig

Data lagres lokalt i nettleseren (`localStorage`). Ta jevnlig sikkerhetskopi via **Fil ▾ → JSON (backup)**.

---

## Funksjoner

### Visninger
| Visning | Beskrivelse |
|---|---|
| **Kart** | Grafisk visning med noder og relasjonslinjer. Dra, koble og organiser fritt på et uendelig lerret. |
| **Tabell** | Strukturert tabellvisning per kategori med inline-redigering av alle parameterverdier. |
| **Matrise** | Krysstabell mellom to kategorier — vis og rediger relasjoner som prikker i et rutenett. |
| **Tidslinje** | Gantt-lignende visning for noder med dato-parametere. |
| **Dashboard** | Egendefinert kontrollpanel med valgfrie widgets fra ulike kategorier og objekter. |

### Datamodell
- **Kategorier** — grupperinger av objekter (f.eks. Strategisk mål, KPI, Kapabilitet, Initiativ, IT-system)
- **Objekter** — noder innenfor en kategori, med egendefinerte parameterverdier
- **Parametere** — feltdefinisjoner per kategori: tekst, tall, status (trafikklys), dato, valgliste, flervalg, ja/nei, beregnet (formel)
- **Relasjoner** — koblinger mellom objekter med type: hierarkisk, måler, støtter, leverer til
- **Perspektiver** — filtreringsvisninger som avgjør hvilke kategorier og grupperingsbokser som er synlige

### Dashboard-widgets
- KPI / enkeltverdi (med valgfri fremdriftslinje mot mål)
- Antall objekter
- Statusfordeling (trafikklys-fordeling)
- Fremdriftsindikator (progress bar)
- Objektliste (mini-tabell)
- Tekst / overskrift
- Avviksliste (objekter som utløser betingede regler)

### Andre funksjoner
- **Grupperingsbokser** — visuelle svømmebaner på kartet
- **Betinget formatering** — automatisk fargesetting av noder og tabellrader basert på parameterverdier
- **Beregningsparametere** — formler med `{ParamNavn}`-referanser og matematiske funksjoner
- **Perspektivfiltrering** — tilpassede visninger for ulike målgrupper (styre, CIO, driftsteam)
- **Auto-layout** — automatisk plassering av noder i kartet
- **Angre / Gjenta** — fullstendig undo/redo-støtte (Ctrl+Z / Ctrl+Shift+Z)

### Import og eksport
- **JSON** — full backup og gjenoppretting av all data
- **CSV** — import av objekter til en kategori
- **SVG / PNG** — eksport av kartvisningen som bilde
- **HTML-rapport** — frittstående rapport med tabeller og betinget formatering
- **Dashboard HTML / PNG** — eksport av dashboardvisningen

---

## Filer

| Fil | Beskrivelse |
|---|---|
| `mapper_edit.html` | Hele applikasjonen — én selvdreven HTML-fil (~4700 linjer) |
| `hjelp.html` | Fullstendig brukerveiledning på norsk |

---

## Teknisk

- **Ren vanilla JS + CSS** — ingen avhengigheter, ingen byggsteg
- **Single-file arkitektur** — alt i `mapper_edit.html`: CSS (linje ~1–545), HTML-skall (~545–720), JavaScript (~720–slutt)
- **Global state** `S` — alle mutasjoner kaller `save()` (localStorage) og `render()`
- **localStorage-nøkkel:** `nx2`

### Tilbakestille til demo-data
Åpne DevTools → Console og kjør:
```js
localStorage.removeItem('nx2'); location.reload();
```

### Syntakssjekk etter endringer
```bash
node -e "
const fs = require('fs');
const html = fs.readFileSync('mapper_edit.html','utf8');
const match = html.match(/<script>([\s\S]*?)<\/script>/);
new Function(match[1]);
console.log('JS syntax OK');
"
```

---

## Lisens

Privat bruk. Ikke for videredistribusjon uten tillatelse.
