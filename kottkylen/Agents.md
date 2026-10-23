# Projekt: Köttkylen Action-Äventyr

## Storyline
Spelet följer tre actionhjältar (Ted, Marcus och Samuel) som återförenas på Navet i Sundsvall för en After Work. 

1. **Starten:** Hjältarna möts på Navet och måste välja var de ska ta sin första öl (Bishops Arms, Daltons, Estreet eller Olearys). Daltons beskrivs som ett rått "sunk".
2. **Transit:** Mellan valet av krog och ankomsten sker en transitfas där hjältarna beger sig mot sitt mål.
3. **Vändpunkten:** Under tiden de njuter av sin öl på krogen/gatan hörs ett avgrundsvrål från stadens centrum.
4. **Konfrontationen på torget:** Hjältarna rusar ut på torget och upptäcker att ryska drönare har ockuperat Sundsvall och att Putin sitter på en tron av skrot.
5. **Val på torget:**
   - **Full Frontal Attack:** En riskfylld stormning av drönarna.
   - **Taktisk manöver:** En flankning genom gränderna för att överraska vakterna.
   - **Diplomatisk lösning:** Utmana Putin på en öl (leder till en dryckesduell).
   - **Ge upp:** Gå hem (Game Over).
6. **Särskilda vägar:**
   - **Flankningen:** Hjältarna smyger genom gränder (`flank.jpg`). Om de tvekar i sista sekunden dör de, annars når de slutstriden.
   - **Dryckesduellen:** Utmanar Putin med alternativ som att "dricka honom av tronen" eller hånle hans smak. Att hälla ut ölen leder till omedelbar död.
7. **Finalen:** Vid framgång leder vägarna till en episk slutstrid på torget (`victory_torget.jpg`) där drönarna krossas och Putin flyr. Äventyret avslutas med en välförtjänt segeröl på Navet.

## Spelmekanik (Logic)

### Öl- och Hälsosystem
- **Individuella mätare:** Varje karaktär har en egen Beer-bar och Health-bar.
- **Ölförbrukning:** Öl minskar slumpmässigt vid varje steg.
- **Hälso-koppling:** Om en karaktärs öl-bar sjunker under **50%**, börjar hälsa tappas i samma takt som ölen.
- **Påfyllning (Beer Refill):** Genom att välja "Take a Beer" (`beer.jpg`) återställs öl till 100% och karaktärerna får **+10 Health** (max 100%).
- **Nådperiod (Grace Period):** Efter en påfyllning sker ingen ölförlust under de kommande **två stegen**.
- **Game Over:** Sker om någon karaktärs öl når 0% eller om man väljer att ge upp. Vid Game Over sätts alla bars till 0.

### Särskilda Händelser
- **Full Frontal Attack:** 
  - Lyckas endast om **alla** karaktärer har **100% öl**.
  - Annars: Våldsam död (`full_frontal_death`).
- **Putins Dryckesduell:**
  - Chansen att förlora direkt (innan man får välja dialog) baseras på den lägsta öl-baren.
  - **Sannolikhet för förlust = lägsta öl-procent**.
  - Om man är för förfriskad (100% öl) är risken 100% att man förlorar då man är "för full för sitt eget bästa" (`putin_drunk_death`).

## UI & Design
- **Font:** Modern systemfont-stack med storlek 16px.
- **Dialogruta:** Maxhöjd 164px med scrollbarhet.
- **Status-paneler:** 
  - Etiketterna "Health" och "Beer" är placerade till vänster om mätarna i ett grid-system (40px etikett, 90px mätare).
  - Centrerad alignering under karaktärernas porträtt.
- **Layout:** Karaktärsytan ligger precis intill bildcontainern (`column-gap: 0.2rem`), medan dialogen har ett större avstånd nedåt (`row-gap: 1rem`).
- **Bilder:** Dynamiska bilder för varje steg, inklusive transit, flankning, seger och olika typer av nederlag.
