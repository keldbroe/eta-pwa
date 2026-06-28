# ETA · VFR

Beregner forventet ankomsttid (ETA) for VFR flyveplaner. Bygget til brug i flyveledelse — kører som en PWA direkte på telefonen, virker offline og bruger 24-timers UTC.

---

## Funktioner

- Stort, touch-venligt numpad — intet tastatur der popper op
- Live UTC-ur i headeren tikker hvert sekund
- Automatisk skift til flyvetid-feltet når starttid er indtastet
- Midnat-advarsel med "+1 dag" badge hvis ankomst er næste dag
- Virker **helt offline** når den er installeret som PWA
- Keyboard-support (f.eks. Bluetooth numpad)

---

## Brug

1. Tast starttid (ATD) i formatet `HHMM` — f.eks. `1435`
2. Tast flyvetid (EET) i formatet `HHMM` — f.eks. `0045`
3. ETA vises automatisk — i dette eksempel `1520`

Tryk **Ryd** for at nulstille begge felter.

---

## Installation som PWA på Android

1. Åbn siden i **Chrome**
2. Tryk på de tre prikker øverst til højre
3. Vælg **"Tilføj til startskærm"**
4. Appen får sit eget ikon og kører i fullscreen uden adresselinje

Siden skal ligge på en HTTPS-adresse for at PWA-installation virker (se hosting nedenfor).

---

## Hosting med GitHub Pages (gratis)

```bash
# 1. Opret et nyt repository på github.com, f.eks. "eta-pwa"

# 2. Upload filerne
git init
git add .
git commit -m "første version"
git remote add origin https://github.com/DITBRUGERNAVN/eta-pwa.git
git push -u origin main

# 3. Aktivér GitHub Pages
# Gå til: Settings → Pages → Source → Deploy from branch (main)
# Din side er tilgængelig på: https://DITBRUGERNAVN.github.io/eta-pwa
```

---

## Filstruktur

```
eta-pwa/
├── index.html      # Al logik og UI
├── manifest.json   # PWA-metadata (navn, ikon, farver)
├── sw.js           # Service worker — håndterer offline-caching
└── icons/
    ├── icon-192.png
    └── icon-512.png
```

---

## Offline-understøttelse

Service workeren cacher alle filer ved første besøg. Efterfølgende åbninger — også uden netværk — serveres fra cachen. For at opdatere til en ny version: bump `CACHE`-konstanten i `sw.js` (f.eks. `eta-v1` → `eta-v2`).

---

## Teknisk

Ren HTML/CSS/JavaScript — ingen frameworks, ingen build-step, ingen afhængigheder. Kører i enhver moderne browser.

ETA-beregningen håndterer midnat-overgange korrekt:

```
ETA = (ATD + EET) mod 1440 minutter
```

Hvis summen overstiger 1440 minutter (24 timer) vises en "+1 dag" advarsel.
