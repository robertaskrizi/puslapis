# BitKalvė — paaiškinimai gynimui

## Failų struktūra

```
bitkalve/
├── index.html        Pagrindinis
├── apie.html         Apie (lentelė, sąrašai)
├── galerija.html     Galerija (6 paveikslėliai + lentelė)
├── uzsakymas.html    Anketa (forma)
├── kontaktai.html    Kontaktai (lentelė, aprašų sąrašas dl)
├── css/stilius.css   VIENA bendra stilių lentelė visiems puslapiams
└── img/              logotipas + 6 SVG paveikslėliai
```

Nenaudojami jokie karkasai ar bibliotekos (nėra Bootstrap, jQuery ir pan.), net JavaScript nėra.

## Reikalavimų atitikimas

| Reikalavimas | Kur įgyvendinta |
|---|---|
| ≥ 4 puslapiai | 5 puslapiai |
| Vieningas dizainas | visi puslapiai naudoja tą patį `css/stilius.css`, tą patį header/nav/aside/footer |
| Content su 2–3 stulpeliais | `.stulpeliai-3` (index, apie), `.stulpeliai-2` (apie, kontaktai, anketa), `.galerija` (3 stulpeliai) |
| header, aside, footer | `<header id="virsus">`, `<aside>`, `<footer id="apacia">` |
| Lentelės | apie.html, galerija.html, kontaktai.html |
| Sąrašai | `ul` (meniu, aside, paketai), `ol` (index), `dl` (kontaktai) |
| Meniu ≥ 4 punktai | 5 punktai: Pagrindinis, Apie, Galerija, Anketa, Kontaktai |
| ≥ 6 paveikslėliai | galerija.html — 6 `<figure>` su `<img>` |
| Anketa su įvairiais input | text, email, tel, date, radio, number, range, checkbox, color, url, file, hidden, submit, reset + select, textarea |

## Tinklalapio schema (HTML)

Kiekvieno puslapio struktūra (ji įrašyta ir HTML komentare kiekvieno failo pradžioje):

```
html
├── head      – meta duomenys, title, nuoroda į CSS
└── body
    ├── header#virsus     – antraštė su logotipu
    ├── nav#meniu         – meniu: ul > li > a
    ├── div.puslapis      – išdėstymas (grid: turinys + šoninis blokas)
    │   ├── main.turinys  – "Content" dalis
    │   └── aside         – šoninis blokas
    └── footer#apacia     – poraštė
```

HTML dokumentas yra **medis**: kiekvienas elementas turi tėvą (išskyrus `html`) ir gali turėti vaikų. Šia schema remiasi:

- **elemento (žymės) selektoriai** — parenka visus tokio tipo elementus: `body`, `h2`, `table`, `aside`;
- **palikuonių selektoriai** — parenka elementą, esantį kito viduje: `#meniu a` (visos nuorodos meniu viduje), `aside h3`;
- **paveldėjimas** — vaikai paveldi kai kurias tėvo savybes (spalvą, šriftą).

## Klasė vs ID

| | Klasė (`class`) | ID (`id`) |
|---|---|---|
| CSS sintaksė | `.kortele` (taškas) | `#virsus` (grotelės) |
| Kiek kartų puslapyje | **daug kartų** | **tik vieną kartą** (unikalus) |
| Vienam elementui | gali turėti kelias: `class="kortele rekomenduojama"` | tik vieną ID |
| Specifiškumas (stiprumas) | 0-1-0 | 1-0-0 — **stipresnis už klasę** |
| Kam naudojama | pasikartojantiems komponentams (kortelės, mygtukai) | unikalioms dalims (header, meniu, footer) |

Pavyzdžiai faile: `.kortele`, `.mygtukas`, `.galerija` — klasės; `#virsus`, `#meniu`, `#apacia`, `#paketas-pro` — ID.

Esminis skirtumas: **klasė skirta grupei elementų, ID — vienam konkrečiam elementui**, ir ID taisyklė nugali klasės taisyklę, kai jos prieštarauja.

## `inherit` — paveldėjimo pavyzdys

```css
a { color: #b8501a; }          /* visos nuorodos — oranžinės */

#apacia { color: #cbbfae; }    /* poraštės tekstas — šviesiai pilkas */

#apacia a {
    color: inherit;            /* paimti spalvą iš tėvinio elemento */
}
```

**Ką reiškia:** `inherit` liepia elementui paimti savybės reikšmę iš savo **tėvinio elemento**. Nuorodos įprastai *nepaveldi* spalvos, nes naršyklė ir taisyklė `a {...}` joms duoda savo spalvą. `inherit` tai „perrašo“.

**Kur pamatyti:** poraštėje (puslapio apačioje) nuorodos „Kontaktai“ ir „info@bitkalve.lt“.

**Ištrynus `inherit`:** nuorodos poraštėje taps **oranžinės** (#b8501a) ir išsiskirs iš likusio pilko teksto. Patikrinta: su `inherit` spalva `rgb(203,191,174)`, be jo — `rgb(184,80,26)`.

(Papildomai `font: inherit` naudojamas mygtukams ir formos laukams — kad jie turėtų tą patį šriftą kaip puslapis, o ne naršyklės numatytąjį.)

## `!important` — kaskadų mechanizmo pavyzdys

**Kaskada** — tai taisyklės, pagal kurias naršyklė sprendžia, kuri CSS taisyklė laimi, kai kelios nustato tą pačią savybę tam pačiam elementui. Tvarka (nuo stipriausios):

1. `!important` deklaracijos
2. Specifiškumas: `style=""` atributas > ID > klasė > elementas
3. Jei specifiškumas vienodas — laimi vėliau faile parašyta taisyklė

```css
#paketas-pro {                         /* ID: specifiškumas 1-0-0 */
    border-color: #a89c8c;             /* pilkas */
}

.rekomenduojama {                      /* klasė: specifiškumas 0-1-0 */
    border-color: #e2702a !important;  /* oranžinis */
}
```

HTML: `<section id="paketas-pro" class="kortele rekomenduojama">`

Be `!important` laimėtų **ID** taisyklė (ji stipresnė už klasę), nors klasė parašyta vėliau. `!important` iškelia klasės deklaraciją aukščiau už įprastą specifiškumą, todėl laimi ji.

**Kur pamatyti:** pagrindiniame puslapyje, „Surinkimo paketai“ — paketas „Pro“.

**Ištrynus `!important`:** „Pro“ paketo rėmelis taps **pilkas**, kaip ir kitų paketų. Patikrinta: su `!important` — `rgb(226,112,42)`, be jo — `rgb(168,156,140)`.

## Kaip atidaryti

Išarchyvuokite ir atidarykite `index.html` naršyklėje. Serverio nereikia.
