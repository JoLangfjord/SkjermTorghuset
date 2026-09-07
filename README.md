# Infoskjerm

En enkel nettside for infoskjerm/TV i sameiet. Meldingene roterer automatisk, og klokke/dato vises hele tiden. Du redigerer innholdet ved å endre filen `data.json` — ingen koding nødvendig.

## 1. Legg filene på GitHub

1. Gå til [github.com](https://github.com) og logg inn (opprett konto hvis du ikke har en).
2. Trykk **New repository**. Gi det et navn, f.eks. `infoskjerm`. La det være **Public** (Pages krever det på gratis-kontoer). Trykk **Create repository**.
3. Trykk **Add file → Upload files**, og last opp `index.html` og `data.json` fra denne mappen.
4. Trykk **Commit changes**.

## 2. Skru på GitHub Pages

1. I repoet, gå til **Settings → Pages** (i menyen til venstre).
2. Under **Build and deployment**, velg **Deploy from a branch**.
3. Velg branch `main` og mappe `/ (root)`, trykk **Save**.
4. Etter ca. 1 minutt vises en lenke øverst i stil med:
   `https://dittbrukernavn.github.io/infoskjerm/`
   Det er denne lenken du åpner på skjermen.

## 3. Slik redigerer du meldinger

1. Gå inn i repoet på github.com og åpne `data.json`.
2. Trykk på blyant-ikonet (**Edit this file**).
3. Legg til, endre eller slett meldinger i listen. Hver melding ser slik ut:

```json
{
  "type": "info",
  "tittel": "Overskrift her",
  "tekst": "Selve teksten. Kan gå over flere linjer.",
  "dato": ""
}
```

- `type` kan være `"info"` (nøytral), `"viktig"` (rød, til viktige beskjeder) eller `"arrangement"` (til ting med dato/klokkeslett, f.eks. dugnad).
- `dato` er valgfri — la stå tom `""` hvis meldingen ikke har en dato, ellers skriv f.eks. `"Lørdag 20. september kl. 11:00"`.
- `bilde` er valgfri — se eget avsnitt under.
- Husk komma mellom hver melding i listen, men **ikke** etter den siste.

4. Scroll ned og trykk **Commit changes**.
5. Skjermen henter automatisk ny data i løpet av noen minutter (den sjekker `data.json` på nytt hvert 5. minutt) — du trenger ikke gjøre noe på selve skjermen.

Øverst i `data.json` som følger med her ligger tre eksempel-meldinger. Slett dem og legg inn deres egne når dere er klare.

## Legge til bilder

1. I repoet: trykk **Add file → Upload files**, og last opp bildet inn i mappen `bilder/` (opprettes automatisk hvis du skriver `bilder/filnavn.jpg` som filnavn ved opplasting).
2. I `data.json`, legg til feltet `"bilde"` i meldingen, med filnavnet:

```json
{
  "type": "info",
  "tittel": "Dugnad i går",
  "tekst": "Takk til alle som stilte opp!",
  "bilde": "bilder/dugnad-2026.jpg",
  "dato": ""
}
```

- Har meldingen både tekst og bilde, vises de side om side.
- Har meldingen **kun** tittel og bilde (tom `"tekst": ""`), vises bildet stort som en plakat/poster.
- Bruk `.jpg` eller `.png`. Landskapsbilder (bredere enn høye) fungerer best på en TV-skjerm. Hold filstørrelsen rimelig (under et par MB) så skjermen laster raskt.
- Bildet kan også være en full lenke (URL) til et bilde som ligger et annet sted, i stedet for et filnavn i `bilder/`.

## 4. Vis siden på TV-en

Enklest er en gammel nettbrett/PC/Chromecast-with-Google-TV/Fire TV Stick koblet til TV-en, eller en Raspberry Pi:

- Åpne nettleseren, gå til lenken fra steg 2.
- Sett nettleseren i fullskjerm/kiosk-modus (på Chrome: `chrome --kiosk https://dittbrukernavn.github.io/infoskjerm/`).
- La den stå på. Siden oppdaterer klokke og innhold av seg selv, og laster seg selv på nytt hver natt kl. 04 for å holde seg stabil over lang tid (så dere slipper akkurat det problemet dere hadde med styretavla).

## Endre utseende

Vil dere endre navnet som vises øverst («Torghuset»), farger, eller hvor lenge hver melding vises (standard 12 sekunder), gjør dere det i `index.html`:
- Sameie-navn: feltet `"sameie"` i `data.json`.
- Visningstid per melding: `VIS_SEKUNDER` nær toppen av `<script>`-delen i `index.html`.
- Farger: `:root { ... }`-delen øverst i `<style>`.
