# Svensk OSINT – Sökbart inläggsarkiv

Statisk webbplats för Svensk OSINTs sökbara arkiv. Publiceras via GitHub Pages.

**Live:** https://veckobrevskartan.github.io/arkiv/  
**Inbäddad i Argus:** https://veckobrevskartan.github.io/arkiv/?embed=1

---

## Innehåll

- [Snabbstart](#snabbstart)
- [Lägga till inlägg](#lägga-till-inlägg)
- [Datamodell](#datamodell)
- [PMESII-värden](#pmesii-värden)
- [Kategorier](#kategorier)
- [Koppling till Argus](#koppling-till-argus)
- [Filstruktur](#filstruktur)

---

## Snabbstart

1. Klona repot
2. Redigera `index.html` – ersätt `POSTS`-arrayen med din data (se nedan)
3. Pusha till `main` – GitHub Pages publicerar automatiskt

---

## Lägga till inlägg

Inlägg hanteras direkt i `index.html` som ett JavaScript-objekt (`POSTS`). I nästa fas separeras datan till `/data/posts.json` och byggs om vid deploy.

### Flöde

```
1. Lägg till inläggsdata i POSTS-arrayen (eller data/posts.json)
2. git add . && git commit -m "vecka 2026-W26"
3. git push origin main
4. GitHub Pages publicerar automatiskt (ca 1-2 min)
```

### Exempelpost

```json
{
  "id": "2026-06-20-ryska-sabotageforsok-frankrike",
  "slug": "2026-06-20-ryska-sabotageforsok-frankrike",
  "title": "Ryska sabotage- och spionageförsök kartläggs i Frankrike",
  "summary": "Flera misstänkta ryska sabotage- och underrättelseoperationer kartlagda på franskt territorium.",
  "body": null,
  "published": "2026-06-20",
  "updated": null,
  "isoYear": 2026,
  "isoWeek": 25,
  "pmesii": ["P", "M", "INFO", "INFRA"],
  "categories": ["HYBRID", "INTEL"],
  "keywords": ["Ryssland", "sabotage", "spionage", "hybridverksamhet"],
  "countries": ["Frankrike", "Ryssland"],
  "places": [
    {
      "name": "Paris",
      "country": "Frankrike",
      "latitude": 48.8566,
      "longitude": 2.3522
    }
  ],
  "persons": [],
  "organisations": ["DGSI", "Le Monde"],
  "sources": [
    { "name": "Le Monde", "url": "https://lemonde.fr" }
  ],
  "linkedinUrl": "https://www.linkedin.com/posts/example",
  "imageUrl": null,
  "timeline": true,
  "map": true,
  "newsletter": "2026-W25",
  "language": "sv"
}
```

---

## Datamodell

### Obligatoriska fält

| Fält | Typ | Exempel |
|------|-----|---------|
| `id` | sträng | `"2026-06-20-rubrik"` |
| `slug` | sträng | `"2026-06-20-rubrik"` |
| `title` | sträng | Inläggets rubrik |
| `summary` | sträng | 1–3 meningar sammanfattning |
| `published` | datum ISO | `"2026-06-20"` |
| `pmesii` | array | `["M", "INFRA"]` |
| `categories` | array | `["HYBRID", "MIL"]` |
| `keywords` | array | `["Ryssland", "sabotage"]` |
| `countries` | array | `["Ryssland", "Sverige"]` |
| `sources` | array | `[{"name":"Reuters","url":"https://..."}]` |
| `linkedinUrl` | URL | LinkedIn-länk till originalinlägg |

### Valfria fält

| Fält | Typ | Beskrivning |
|------|-----|-------------|
| `body` | sträng | Fullständig artikeltext |
| `places` | array | Platser med koordinater |
| `persons` | array | Namngivna personer |
| `organisations` | array | Organisationer |
| `timeline` | boolean | Ska visas på tidslinjen |
| `map` | boolean | Ska visas på kartan |
| `newsletter` | sträng | Veckobrevskoppling `"2026-W25"` |
| `imageUrl` | URL | Bild till inlägget |

### Platser med koordinater

```json
"places": [
  {
    "name": "Kaliningrad",
    "country": "Ryssland",
    "latitude": 54.7104,
    "longitude": 20.5103
  }
]
```

---

## PMESII-värden

| Nyckel | Beskrivning |
|--------|-------------|
| `P` | Politiskt |
| `M` | Militärt |
| `E` | Ekonomiskt |
| `S` | Socialt |
| `INFO` | Information / informationspåverkan |
| `INFRA` | Infrastruktur |

Flera PMESII-värden kan anges per inlägg: `["M", "INFRA", "INFO"]`

---

## Kategorier

| Nyckel | Beskrivning |
|--------|-------------|
| `HYBRID` | Hybridkrigföring |
| `NUCLEAR` | Nukleärt |
| `DRONE` | Drönare |
| `INFRA` | Infrastrukturhot |
| `MIL` | Militärt |
| `INTEL` | Underrättelseverksamhet |
| `TERROR` | Terrorism |
| `POLICY` | Policy / beslut |
| `LEGAL` | Juridik / lag |
| `MAR` | Marint / sjöfart |
| `GPS` | GPS-störning / navigation |

---

## Sökfunktioner

### Söklägen

| Läge | Funktion |
|------|----------|
| `AND` | Alla ord måste förekomma (standard) |
| `OR` | Minst ett ord måste förekomma |
| `PHRASE` | Exakt fras söks |

### Söksyntax

| Exempel | Funktion |
|---------|----------|
| `Ryssland järnväg` | Båda ord (AND-läge) |
| `Ryssland järnväg -olycka` | Exkludera "olycka" med minustecken |
| `"rysk ubåt"` | Exakt fras (fungerar i PHRASE-läge) |

### Delningsbara sökningar

Sökningar sparas automatiskt i URL:en:

```
https://veckobrevskartan.github.io/arkiv/?q=ryssland+sabotage&pmesii=M,INFRA&year=2026
```

---

## API-endpoints (statiska JSON-filer)

| URL | Innehåll |
|-----|----------|
| `/data/posts.json` | Alla inlägg |
| `/data/latest.json` | De 10 senaste inläggen |
| `/data/map.json` | Inlägg märkta `"map": true` |
| `/data/timeline.json` | Inlägg märkta `"timeline": true` |

---

## Koppling till Argus

### Alternativ A – iframe

```html
<iframe
  src="https://veckobrevskartan.github.io/arkiv/?embed=1"
  title="Svensk OSINT – sökbart arkiv"
  loading="lazy"
  width="100%"
  height="700"
  frameborder="0">
</iframe>
```

I inbäddningsläget (`?embed=1`) döljs navigation och sidfot. Knapp för att öppna i ny flik visas.

### Alternativ B – länk

```
https://veckobrevskartan.github.io/arkiv/
```

---

## Filstruktur

```
/
├── index.html          Huvudsida (allt-i-ett, deploybar direkt)
├── _config.yml         GitHub Pages-konfiguration
├── robots.txt          Sökmotorkonfiguration
├── README.md           Dokumentation
└── data/
    ├── posts.json      Alla inlägg
    ├── latest.json     10 senaste
    ├── map.json        Kartinlägg
    └── timeline.json   Tidslinjeinlägg
```

---

## Fas 2 – planerade förbättringar

- Separation av data till `/data/YYYY/YYYY-WNN.json` per vecka
- GitHub Actions för automatisk validering av ny data
- Pagefind-integration för fulltextsökning utan ombyggnad
- CSV-importskript
- Automatisk sitemap.xml-generering
- Person- och organisationsregister

---

*Svensk OSINT – Omvärldsbevakning med öppen källanalys*
