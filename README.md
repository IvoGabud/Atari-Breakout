# Breakout

Jednostavnija verzija klasične arkadne igre **Atari Breakout**, izrađena kao HTML5 web stranica. Igra je napisana u čistom JavaScriptu (vanilla JS) bez korištenja vanjskih biblioteka ili frameworka — sve iscrtavanje odvija se ručno pomoću HTML5 Canvas API-ja.

**Igra uživo:** [breakout-7mou.onrender.com](https://breakout-7mou.onrender.com/)

## Struktura projekta

```
├── index.html      # HTML struktura stranice i <canvas> element
├── style.css       # Izgled stranice (pozadina, centriranje, rub canvasa)
├── constants.js     # Sve konstante igre (dimenzije, brzine, boje...)
└── game.js         # Cijela logika igre (crtanje, kolizije, stanja igre)
```

Igra se sastoji od jedne HTML datoteke, jedne CSS datoteke i dvije JavaScript datoteke (konstante su odvojene od glavne logike radi preglednosti).

## Pokretanje

Igra ne zahtijeva nikakvu instalaciju, build proces ni pakete — dovoljno je otvoriti `index.html` u web pregledniku, primjerice dvostrukim klikom na datoteku, ili posluživanjem mape putem bilo kojeg statičkog web servera (npr. VS Code Live Server ekstenzija).

Online, igra je dostupna na [breakout-7mou.onrender.com](https://breakout-7mou.onrender.com/) (Render, besplatni plan za statički hosting).

## Kontrole

| Tipka       | Akcija                                          |
| ----------- | ----------------------------------------------- |
| `←` ili `A` | Pomicanje palice ulijevo                        |
| `→` ili `D` | Pomicanje palice udesno                         |
| `Space`     | Početak igre / ponovni pokušaj nakon kraja igre |

## Tijek igre

1. Nakon učitavanja stranice prikazuje se početni ekran s natpisom **BREAKOUT** i uputom **"Press SPACE to begin"**.
2. Pritiskom na razmaknicu generira se 50 cigli raspoređenih u 5 redaka i 10 stupaca, a loptica kreće sa sredine palice pod kutem od 45° (nasumično ulijevo ili udesno).
3. Igrač palicom odbija lopticu kako bi razbio što više cigli — svaka razbijena cigla nosi 1 bod.
4. Igra završava:
   - **porazom** (`GAME OVER`, žuti tekst), ako loptica ispadne kroz donji rub ekrana, ili
   - **pobjedom** (`YOU WIN!`, zeleni tekst), ako igrač razbije sve cigle.
5. Nakon kraja igre, ponovnim pritiskom na razmaknicu igra se pokreće iznova.

## Implementirane funkcionalnosti

- Igra se prikazuje unutar `<canvas>` fiksnih dimenzija (720×576 px) koje se ne mijenjaju promjenom veličine prozora preglednika.
- Crna pozadina canvasa i vidljivi bijeli rub širine 5 piksela.
- Igra počinje odmah po učitavanju stranice; prikazuje se centrirani naslov **BREAKOUT** (bold, 36px, Helvetica/Verdana) i ispod njega **"Press SPACE to begin"** (bold, italic, 18px), oboje bijele boje.
- Igra se pokreće pritiskom na razmaknicu (Space).
- Generira se 50 cigli raspoređenih u 5 redaka × 10 stupaca.
- Loptica se generira na sredini palice i kreće se pod kutem od 45° nasumično ulijevo-gore ili desno-gore.
- Upravljanje palicom tipkovnicom (strelice lijevo/desno ili tipke A/D).
- Loptica se kreće konstantnom brzinom do prvog udarca u ciglu; brzina se povećava pri udarcu u kut cigle, a smjer se mijenja pri svakom sudaru s palicom, ciglama ili rubovima ekrana.
- Broj cigli i početna brzina loptice definirani su kao simboličke konstante (`TOTAL_BRICKS`, `BRICK_ROWS`, `BRICK_COLS`, `INITIAL_BALL_SPEED`, ...) u `constants.js`.
- Cigle su raznobojni pravokutnici iscrtani na canvasu (nisu slike), s definiranim razmakom i bojama prema zadanim RGB vrijednostima:
  - smeđa `rgb(153, 51, 0)`
  - crvena `rgb(255, 0, 0)`
  - ružičasta `rgb(255, 153, 204)`
  - zelena `rgb(0, 255, 0)`
  - žuta `rgb(255, 255, 153)`
- Loptica je prikazana kao mali bijeli kvadrat, iscrtan na canvasu.
- Palica je bijeli/svijetlosivi pravokutnik dimenzija 60×10 px (iznad minimalnih 25×10 px), iscrtana na canvasu.
- Cigle, palica i loptica imaju iscrtan rub sa svjetlijom i tamnijom stranom (efekt "3D" sjenčanja).
- Detekcija kolizije loptice sa svakom pojedinom ciglom, palicom i rubovima ekrana provjerava se u svakom koraku animacije; pogođena cigla nestaje, a igrač dobiva 1 bod.
- Poruka **"GAME OVER"** (žuto, bold, 40px) prikazuje se centrirano ako loptica ispadne kroz donji rub ekrana.
- Poruka **"YOU WIN!"** (zeleno, bold, 40px) prikazuje se centrirano ako igrač razbije sve cigle.
- Bodovanje: svaka razbijena cigla nosi 1 bod, trenutni rezultat prati se tijekom cijele igre.
- Najbolji dosad ostvareni rezultat pohranjuje se trajno pomoću `localStorage` (Web Storage API) i učitava pri svakom pokretanju stranice.
- Trenutni broj bodova prikazuje se u gornjem lijevom kutu canvasa (`SCORE: ...`), a najbolji dosadašnji rezultat u gornjem desnom kutu (`BEST: ...`).
- Izvorni kôd (HTML i JavaScript) sadrži komentare koji objašnjavaju deklaracije, objekte i funkcije.

## Neimplementirane opcionalne funkcionalnosti

- **Zvučni efekti** pri sudaru loptice s ciglom/palicom/rubom ekrana te pri početku/kraju igre nisu implementirani. Ova funkcionalnost je prema opisu zadatka izričito opcionalna ("nije obavezno") i namjerno je izostavljena kako bi fokus ostao na obaveznim funkcionalnostima igre.

Sve obavezne funkcionalnosti opisane u zadatku su implementirane.

## Korištene tehnologije

- HTML5 (`<canvas>`)
- CSS3
- Vanilla JavaScript (bez vanjskih biblioteka, frameworka ili gotovih rješenja za igre/animacije)
- `localStorage` (Web Storage API) za pohranu najboljeg rezultata

## Parametri igre (konstante)

Svi ključni parametri igre definirani su kao konstante u `constants.js`, čime ih je jednostavno mijenjati na jednom mjestu:

| Konstanta                        | Vrijednost | Opis                        |
| -------------------------------- | ---------- | --------------------------- |
| `CANVAS_WIDTH` / `CANVAS_HEIGHT` | 720 / 576  | Dimenzije canvasa           |
| `BRICK_ROWS` / `BRICK_COLS`      | 5 / 10     | Broj redaka i stupaca cigli |
| `TOTAL_BRICKS`                   | 50         | Ukupan broj cigli           |
| `BRICK_WIDTH` / `BRICK_HEIGHT`   | 40 / 15    | Dimenzije cigle             |
| `BALL_SIZE`                      | 8          | Veličina loptice            |
| `INITIAL_BALL_SPEED`             | 3          | Početna brzina loptice      |
| `PADDLE_WIDTH` / `PADDLE_HEIGHT` | 60 / 10    | Dimenzije palice            |
| `PADDLE_SPEED`                   | 7          | Brzina pomicanja palice     |
