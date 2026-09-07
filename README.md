# Infoskjerm

En nettside for infoskjerm/TV i sameiet. Den roterer automatisk gjennom tre typer innhold:

1. **Meldinger** — det du selv skriver inn i `data.json`.
2. **Avganger** — sanntids neste tog/buss fra Jessheim stasjon (hentet fra Entur).
3. **Vær** — værmelding for Gotaasalléen 17, Jessheim (hentet fra Meteorologisk institutt/Yr).

Klokke og dato vises hele tiden øverst. Alt oppdaterer seg selv — du trenger bare å redigere `data.json` når dere har nye meldinger.

## 1. Legg filene på GitHub

1. Gå til [github.com](https://github.com) og logg inn (opprett konto hvis du ikke har en).
2. Trykk **New repository**. Gi det et navn, f.eks. `infoskjerm`. La det være **Public** (Pages krever det på gratis-kontoer). Trykk **Create repository**.
3. Trykk **Add file → Upload files**, og last opp `index.html` og `data.json` fra denne mappen (og gjerne `bilder/`-mappen).
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
2. Trykk på blyant-ikonet (**Edit this file**) — ikke "code view"/github.dev, den er skrivebeskyttet.
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
- **Viktig:** skriv teksten din på **én sammenhengende linje** i redigeringsfeltet. Trykker du Enter for å lage avsnitt inni `"tekst"`, ødelegger det filen. Bruk `\n` der du vil ha linjeskift, f.eks. `"Første avsnitt.\n\nAndre avsnitt."`.

4. Scroll ned og trykk **Commit changes**.
5. Skjermen henter automatisk ny data i løpet av noen minutter (sjekker `data.json` på nytt hvert 5. minutt) — du trenger ikke gjøre noe på selve skjermen.

Tre eksempel-meldinger følger med i `data.json`. Slett dem og legg inn deres egne når dere er klare.

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

## Avganger (tog/buss) — ingenting å redigere

Denne slidjen henter automatisk sanntids neste avganger for tog og buss fra Jessheim, direkte fra Entur (det nasjonale reiseplanleggersystemet — samme data NSB/Vy og Ruter selv bruker). Oppdaterer seg hvert 30. sekund.

Merk: tog og buss ligger på **to ulike holdeplass-ID-er** hos Entur, selv om de fysisk er samme sted — Jessheim stasjon (tog) og Jessheim bussterminal (buss) er registrert separat. Begge er allerede lagt inn i `STOPPESTED_IDER` i `index.html`.

Flytter dere til et annet sted, endre `STOPPESTED_IDER`-listen i `<script>`-delen i `index.html`. Finn riktig ID ved å søke opp stedet på [entur.no](https://entur.no), klikke på stoppestedet i reiseplanleggeren, og lese `NSR:StopPlace:...`-verdien i nettleserens adresselinje — husk å sjekke om bussen ligger på en egen ID slik som her.

*(Filen `avganger.html` er også med i mappen, som en frittstående versjon av kun avgangstavla — nyttig om dere noen gang vil vise avganger alene på en annen skjerm.)*

## Vær — ingenting å redigere

Værsliden henter data fra Meteorologisk institutt (samme kilde som Yr) for koordinatene til Gotaasalléen 17, Jessheim, og oppdaterer seg hvert 20. minutt.

Vil dere vise været for et annet sted, endre `VAER_LAT`, `VAER_LON` og `VAER_STED_NAVN` i `<script>`-delen i `index.html`. Finn koordinater ved å søke opp adressen på [norgeskart.no](https://norgeskart.no) eller [yr.no](https://www.yr.no).

## 4. Vis siden på TV-en

Enklest er en gammel nettbrett/PC/Chromecast-with-Google-TV/Fire TV Stick koblet til TV-en, eller en Raspberry Pi:

- Åpne nettleseren, gå til lenken fra steg 2.
- Sett nettleseren i fullskjerm/kiosk-modus (på Chrome: `chrome --kiosk https://dittbrukernavn.github.io/infoskjerm/`).
- La den stå på. Siden oppdaterer klokke og innhold av seg selv, og laster seg selv på nytt hver natt kl. 04 for å holde seg stabil over lang tid (så dere slipper akkurat det problemet dere hadde med styretavla).

## Endre utseende

Alt gjøres i `index.html`:
- Sameie-navn: feltet `"sameie"` i `data.json`.
- Visningstid per melding: `VIS_SEKUNDER_STANDARD` nær toppen av `<script>`-delen. Avganger- og vær-sliden har egne visningstider satt via `data-varighet` på selve `<div class="slide ...">`-elementene lenger opp i filen.
- Farger: `:root { ... }`-delen øverst i `<style>`.
