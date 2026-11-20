# ✉️ Brieven typen – demo

Een kleine webapp waarmee leerlingen (basisschool) een brief schrijven en typen. Er is een schermtoetsenbord en de app geeft direct feedback op de tekst: tellingen, een cijfer en tips.

Alles staat in **één HTML-bestand** met HTML, CSS en JavaScript erin. Je hebt geen server, internet, installatie of build-stap nodig.

---

## Inhoudsopgave

1. [Snel starten](#snel-starten)
2. [Bestanden in deze repository](#bestanden-in-deze-repository)
3. [Wat kan de app?](#wat-kan-de-app)
4. [Hoe het bestand is opgebouwd](#hoe-het-bestand-is-opgebouwd)
5. [Opbouw van de pagina (HTML)](#opbouw-van-de-pagina-html)
6. [Vormgeving (CSS)](#vormgeving-css)
7. [Werking (JavaScript)](#werking-javascript)
   - [Schrijfopdrachten](#1-schrijfopdrachten)
   - [Schermtoetsenbord](#2-schermtoetsenbord)
   - [Tekstanalyse en tips](#3-tekstanalyse-en-tips)
   - [Cijferberekening](#4-cijferberekening)
   - [Statistieken bijwerken](#5-statistieken-bijwerken)
   - [Nieuwe brief](#6-nieuwe-brief)
   - [Bewaren als plaatje (PNG)](#7-bewaren-als-plaatje-png)
8. [Aanpassen en uitbreiden](#aanpassen-en-uitbreiden)
9. [Bekende aandachtspunten](#bekende-aandachtspunten)

---

## Snel starten

1. Download `Brief_schrijven_5.html`.
2. Open het bestand met dubbelklik in een moderne browser, zoals Chrome, Edge, Firefox of Safari.
3. Kies een schrijfopdracht en begin met typen.

Je kunt het bestand ook online zetten via **GitHub Pages**. Ga naar *Settings → Pages*, kies de branch `main` en open daarna `https://<gebruikersnaam>.github.io/Brief-schrijven-/Brief_schrijven_5.html`.

---

## Bestanden in deze repository

| Bestand | Toelichting |
|---|---|
| `Brief_schrijven_5.html` | **Het hoofdbestand.** Open dit in de browser. |
| `Brief schrijven 5 .html` | Precies dezelfde inhoud als hierboven (kopie). |
| `Brief schrijven 5.0.txt` | Dezelfde HTML-code, opgeslagen als tekstbestand. |
| `Brief_schrijven_5.docx` | Dezelfde HTML-code met de extensie `.docx`. Dit is **geen echt Word-document** en Word kan het daarom niet goed openen. |
| `README.md` | Deze uitleg. |

> De vier bestanden hebben dezelfde inhoud (zelfde MD5-hash). Als je iets wijzigt, doe dat dan in `Brief_schrijven_5.html`. Overweeg om de kopieën te verwijderen, zodat er maar één versie bestaat.

---

## Wat kan de app?

- **Drie schrijfopdrachten** met elk eigen schrijfpunten:
  - Brief aan een vriend(in)
  - Brief aan juf/meester
  - Brief aan jezelf (later)
- **Schrijfvak met schriftpapier-look**: lijntjes en een rode kantlijn. De browser controleert de spelling in het Nederlands (`spellcheck="true" lang="nl"`).
- **Schermtoetsenbord** dat je kunt aan- en uitzetten, met Shift voor hoofdletters en symbolen.
- **Live statistieken**: woorden, zinnen, tekens, leestekens en een **cijfer (1–10)**.
- **Feedback** met één korte tip, plus een **lijst met concrete fouten per zin**.
- **Checklist** van vijf punten die de leerling zelf afvinkt.
- **Nieuwe brief**: maakt de tekst en de checklist leeg (na een bevestiging).
- **Bewaar als plaatje**: downloadt de brief als PNG op schriftpapier (`mijn_brief.png`).

Er wordt niets opgeslagen of verstuurd. Alles gebeurt lokaal in de browser. Als je de pagina ververst, is de tekst weg.

---

## Hoe het bestand is opgebouwd

```
Brief_schrijven_5.html
├── <head>
│   ├── meta (utf-8, viewport) + <title>
│   └── <style>      → alle CSS
└── <body>
    ├── .ribbon      → kopbalk met titel
    ├── .container
    │   ├── .panel   → opdracht kiezen + opdrachttekst
    │   └── .panel   → schrijfvak, toetsenbord, stats, knoppen, feedback, checklist
    └── <script>     → alle JavaScript
```

---

## Opbouw van de pagina (HTML)

### Kopbalk (`.ribbon`)
De titel "Brieven typen – demo" met ✉️-logo en een korte ondertitel.

### Paneel 1 – Schrijfopdracht kiezen
| Element | Functie |
|---|---|
| `button.levelBtn[data-op="vriend"]` | Opdracht "Brief aan een vriend(in)" (groen) |
| `button.levelBtn[data-op="juf"]` | Opdracht "Brief aan juf/meester" (blauw) |
| `button.levelBtn[data-op="toekomst"]` | Opdracht "Brief aan jezelf (later)" (oranje) |
| `#promptText` | Hier staan de schrijfpunten van de gekozen opdracht |

### Paneel 2 – Jouw brief
| Element (id) | Functie |
|---|---|
| `#letterText` | Het schrijfvak (`<textarea>`) |
| `#showKeyboard` | Vinkje om het schermtoetsenbord te tonen of te verbergen |
| `#keyboard` met `#row0`–`#row4` | Container waarin JavaScript de toetsen zet |
| `#wordCount`, `#sentenceCount`, `#charCount`, `#punctCount`, `#grade` | Statistieken |
| `#btnNew` | Knop "Nieuwe brief" |
| `#btnImage` | Knop "Bewaar als plaatje" |
| `#feedback` | Korte feedbackregel |
| `#issues` | Lijst met fouten per zin |
| `#cl1`–`#cl5` | Checklist-vakjes (de leerling vinkt ze zelf aan) |

---

## Vormgeving (CSS)

- **Kleurvariabelen** staan in `:root`: `--bg`, `--panel`, `--line`, `--ink`, `--accent`, `--accent2` en `--btn-disabled`. Hier pas je het kleurenschema snel aan.
- **`.ribbon`**: kopbalk met een kleurverloop (blauw → geel → roze).
- **`.panel`**: witte kaarten met afgeronde hoeken en een lichte schaduw.
- **`.op1` / `.op2` / `.op3`**: kleuren van de opdrachtknoppen. `.op-active` geeft de gekozen knop een blauwe rand.
- **`.editor`**: het schriftpapier. Het is gemaakt met twee `linear-gradient`-achtergronden:
  - horizontale grijze lijnen om de **28px**. Dat is gelijk aan `line-height: 28px`, zodat de tekst precies op de lijnen staat;
  - een rode verticale kantlijn op 34px van links. De `padding-left: 52px` houdt de tekst rechts van de kantlijn.
- **`.kbd`, `.row`, `.key`**: opmaak van het schermtoetsenbord. `.wide` is voor brede toetsen, `.space` voor de spatiebalk en `.hl` licht de actieve Shift op.
- **`.stats` / `.stat-badge`**: de ronde labels met tellingen.
- **`.btn` / `.btn-main`**: knoppen. De groene knop is de hoofdactie.
- **`.feedback`, `.issues`, `.checklist`**: het feedbackgedeelte en de checklist.

---

## Werking (JavaScript)

Alle code staat in één `<script>` onderaan de pagina. Er worden geen externe bibliotheken gebruikt.

### 1. Schrijfopdrachten

```js
const prompts = { vriend: {...}, juf: {...}, toekomst: {...} };
let currentPromptKey = "vriend";
```

- Elke opdracht heeft een `key` en een `text` met de schrijfpunten.
- Een klik op een opdrachtknop:
  1. verplaatst de klasse `op-active` naar die knop;
  2. zet `currentPromptKey` op de waarde uit `data-op`;
  3. toont de tekst in `#promptText`;
  4. roept `updateStats()` aan, omdat het cijfer afhangt van de gekozen opdracht (zie *inhoudsscore*).

### 2. Schermtoetsenbord

**Opbouw (`buildKeyboard`)**: deze functie wordt meteen bij het laden uitgevoerd. Ze maakt vijf rijen toetsen in QWERTY-indeling:

| Rij | Toetsen |
|---|---|
| `row0` | `1 2 3 4 5 6 7 8 9 0 - =` (cijfers tonen ook hun Shift-teken, zoals `!` boven `1`) |
| `row1` | `q w e r t y u i o p [ ] \` |
| `row2` | `a s d f g h j k l ; '` |
| `row3` | `Shift z x c v b n m , . / Shift` |
| `row4` | `Ctrl Spatie Enter Backspace` |

Elke toets is een `<div class="key">`. In `data-key` staat de waarde die ingevoegd wordt (bijvoorbeeld `" "` voor spatie en `"\n"` voor Enter).

**Indrukken (`virtualKeyPress(dkey)`)**:
- **Shift**: zet `shiftOn` aan of uit en licht beide Shift-toetsen op (`setShiftActive`). Shift werkt voor **één toets** en gaat daarna vanzelf uit.
- **Ctrl**: doet niets, hij is er alleen voor de vorm.
- **Backspace**: verwijdert het teken vóór de cursor, of de selectie als er tekst geselecteerd is.
- **Letters**: kleine letter, of hoofdletter als Shift aan staat.
- **Cijfers met Shift**: `! @ # € % ^ & * ( )`.
- **`/` met Shift**: `?`.
- **Andere toetsen**: het teken wordt zo ingevoegd.

De tekst komt **op de plek van de cursor**, via `selectionStart`/`selectionEnd`. Daarna stuurt de functie een `input`-event, zodat de statistieken meteen bijwerken.

### 3. Tekstanalyse en tips

**`analyzeSentences(text)`**: splitst de tekst in zinnen met de regex `/[^.!?]+[.!?]?/g`. Een zin is dus alles tot en met een `.`, `!` of `?`. Het laatste stuk mag zonder leesteken eindigen.

**`buildIssues(text)`**: maakt de lijst met fouten die onder de brief staat:
- *Zin N: begin met een hoofdletter.* → de zin begint met een kleine letter.
- *Zin N: eindig met een punt, vraagteken of uitroepteken.* → er staat geen leesteken aan het eind.
- *Naam na "Beste" met hoofdletter schrijven…* → er staat een woord na "beste" (zie [Bekende aandachtspunten](#bekende-aandachtspunten)).

### 4. Cijferberekening

`calculateGrade(text, promptKey)` geeft `{grade, reason}` terug, of `null` als de tekst leeg is.

| Stap | Regel |
|---|---|
| Te kort | Minder dan 5 woorden of geen zinnen → cijfer **2,5** met de tip "Schrijf eerst wat meer zinnen…" |
| Start | **10** punten |
| Hoofdletters | −1 per zin die met een kleine letter begint (**max −3**) |
| Leestekens | −1 per zin zonder `.`, `!` of `?` aan het eind (**max −3**) |
| Begroeting | −1 als er geen regel met **"beste"** of **"hallo"** begint |
| Voorstellen | −1 als **"ik ben"**, **"ik heet"** of **"mijn naam"** ontbreekt |
| Afsluiting | −1 als **"groetjes"**, **"met vriendelijke groet"** of **"vriendelijke groeten"** ontbreekt |
| Lengte | −1 bij minder dan **5 zinnen** |
| Inhoud | 0 trefwoorden van de opdracht → −2; precies 1 trefwoord → −1 |
| Bonus | +0,5 als alles hierboven in orde is |
| Afronden | Het cijfer blijft tussen 1 en 10 en wordt afgerond op één decimaal |

**Trefwoorden per opdracht** (gezocht in kleine letters, ook als deel van een woord):

| Opdracht | Trefwoorden |
|---|---|
| `vriend` | hobby, vriend, vriendin, samen, spelen, dag, avontuur, leuk, plezier |
| `juf` | klas, school, leren, uitleg, juf, meester, stil, rustig, druk, regels, huiswerk |
| `toekomst` | later, worden, beroep, droom, toekomst, hoop, wens, piloot, dokter, leraar, baan |

**Tip (`reason`)**: de app toont één tip. Hij kiest de eerste die van toepassing is, in deze volgorde: hoofdletters → leestekens → begroeting/afsluiting → inhoud → "Je brief is al netjes…".

### 5. Statistieken bijwerken

`updateStats()` draait bij elke toetsaanslag (`input`-event), bij het kiezen van een andere opdracht en één keer bij het laden van de pagina. Deze functie:
- telt **woorden** (gesplitst op witruimte), **zinnen** (aantal `.`, `!` en `?`), **tekens** (alle tekens, ook spaties) en **leestekens** (`. ! ? , ; :`);
- toont het cijfer met een komma als decimaalteken, bijvoorbeeld `7,5`;
- zet de tip in `#feedback` en de foutenlijst in `#issues`.

### 6. Nieuwe brief

`#btnNew` vraagt eerst om een bevestiging (alleen als er tekst staat). Daarna maakt hij het schrijfvak leeg, zet de statistieken terug en haalt de vinkjes uit `#cl1`–`#cl5` weg.

### 7. Bewaren als plaatje (PNG)

`#btnImage` tekent de brief op een `<canvas>`:
1. De breedte is 900px, met 40px marge en 32px regelhoogte.
2. De tekst wordt per alinea in regels verdeeld (`wrapText`, met `measureText`). Tussen alinea's komt een lege regel.
3. De achtergrond is wit, met grijze lijnen en een rode kantlijn (net als in het schrijfvak).
4. Bovenaan staat de titel **"Mijn brief"** (26px). De tekst zelf is 20px.
5. Het plaatje wordt gedownload als `mijn_brief.png` en ook in een nieuw tabblad geopend. Sommige browsers blokkeren dat tabblad; de download werkt dan wel.

---

## Aanpassen en uitbreiden

| Wat wil je? | Waar pas je het aan? |
|---|---|
| Een **nieuwe opdracht** toevoegen | 1) Voeg een knop toe met `class="levelBtn op4" data-op="mijnkey"` (en maak eventueel een CSS-klasse `.op4`). 2) Voeg `mijnkey: { key, text }` toe aan `prompts`. 3) Voeg in `calculateGrade` een `else if(promptKey === "mijnkey")` toe met eigen trefwoorden. |
| Opdrachttekst wijzigen | `prompts` in het script. Pas ook de begintekst in `#promptText` in de HTML aan, want die wordt bij het laden getoond. |
| Trefwoorden wijzigen | De `keywords`-lijsten in `calculateGrade`. |
| Strenger of milder beoordelen | De aftrekregels en maxima in `calculateGrade`. |
| Checklist aanpassen | De `<li>`-items in `.checklist`. Gebruik je meer dan vijf items, pas dan ook de lus `for(let i=1;i<=5;i++)` bij "Nieuwe brief" aan. |
| Kleuren aanpassen | De CSS-variabelen in `:root` en de klassen `.op1`–`.op3` en `.ribbon`. |
| Toetsenbord aanpassen | De `rows`-array in `buildKeyboard`. Voor Shift-tekens pas je ook `virtualKeyPress` aan. |
| Bestandsnaam of titel van het plaatje | `a.download` en `title` in de handler van `#btnImage`. |

---

## Bekende aandachtspunten

- **"Beste"-controle geeft altijd een melding**: de tekst wordt eerst naar kleine letters omgezet. Daardoor ziet de app ook bij "Beste Jan" een kleine letter en geeft dan toch de tip om een hoofdletter te gebruiken.
- **Twee manieren om zinnen te tellen**: de teller "Zinnen" telt alleen leestekens (`.`, `!` en `?`). Het cijfer gebruikt `analyzeSentences`, die ook een laatste zin zonder punt meetelt. Daardoor kunnen de twee verschillende aantallen geven.
- **Trefwoorden worden ook binnen woorden gevonden**: "dag" telt ook mee in "vandaag" en "maandag".
- **Afkortingen en getallen** zoals "bijv." en "3.5" worden als einde van een zin gezien.
- **Checklist telt niet mee** voor het cijfer. De leerling vinkt de vakjes zelf aan.
- **Ctrl-toets** op het schermtoetsenbord doet niets.
- **Toetsenbord** heeft geen toetsen voor letters met accenten (é, ë). Die kun je wel typen met het gewone toetsenbord.
- **Niets wordt bewaard**: na het verversen of sluiten van de pagina is de brief weg. Bewaar hem eerst als plaatje.
- **Dubbele bestanden**: zie [Bestanden in deze repository](#bestanden-in-deze-repository).
