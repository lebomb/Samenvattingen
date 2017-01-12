# Analyse III
## Documentatie
---

## User stories
### Wat is een user story?
Een user story is een **korte beschrijving** (story) van **wat een gebruiker wil** (user).

Het zijn enkele zinnen in **gewone spreektaal** van de gebruiker waarin staat wat de gebruiker doet of moet doen, als onderdeel van z'n werk. User stories worden gebruikt binnen **agile software development** als een manier om de requierements te beschrijven. Hierin zegt men **'wie', 'wat', 'waarom'** doet. Het geheel is weinig gedetailleerd en moet passen op een **post-it**.

User stories kunnen ook geschreven worden door ontwikkelaars om niet functionele eisen vast te leggen bijvoorbeeld op het gebied van beveiliging, kwaliteit en performance. Maar de eerst verantwoordelijke voor de verzameling van de user stories is de **producteigenaar**.

```
As a <role>
I want to <goal/desire>
So that <benefit>
```

```
Als een <rol>
Wil ik <doel>
Zodat <voordeel>
```

### Waarom user stories?

- Verbale communicatie stimuleren/forceren
- Snelle feedback
- Face-to-face communicatie
- Geen technisch jargon
- Planning faciliteren
- Detailleer waar en wanneer nodig (korte termijn versus midellange termijn)

### User stories schrijven

Een goede user story focust op 6 elementen: (INVEST)
- Independent (Onafhankelijk)
- Negotiable (Onderhandelbaar)
- Valuable (Waardevol voor gebruiker en opdrachtgever)
- Estimatable (Schatbaar)
- Small (Klein)
- Testable (Testbaar)


#### Tips

- Identificeer de stories
    - Start met vastleggen van de doelstellingen
- "Slice the cake"
    - Bij het schrijven (opsplitsen) van user stories aandacht dat alle lagen van de applicatie voorkomen
- "gesloten" stories
    - Gesloten story eindigt met een user's goal
- Leg beperkingen vast
- Focus op de belangrijke zaken in de nabije toekomst
- GUI zo lang mogelijk uitstellen
- Gebruik "user roles" in de story
- Schrijf voor 1 user
- Schrijf in actieve taal
- Laat de gebruiker/opdrachtgever meeschrijven
- User stories worden niet genummerd
- Zijn niet gebonden aan IEEE-guidelines
- Zijn geen use cases
    - Scope
    - Volledigheid
    - Levensduur
    - Doel

### Story map

Een story map is een voorstelling van de user stories volgens prioriteit (hoog , laag, verticale as) en functionaliteit (van links naar rechts, horizontale as)

/* afbeeldingen proces mapping toevoegen */
**MOET IK NOG VERDER UITWERKEN**

##### Eco system
Zorg dat je de omgeving en stakeholders verstaat
Maak een lijst van aangeboden diensten en producten die aan eisen van klant voldoen
Creatie van Stakeholder Map en voeg de dimensiewaarde toe en maak een waarde netwerkmap aan
Verdeel je gebruikers en beschrijf hun Presonas
Beschrijf hun ervaring dmv Customer Journey Maps

##### Prepare
Beslis wat te onderzoeken: bestaande of nieuwe producen
Onderzoek de Customer Journey Maps en zoek naar kansen of bedreigingen
beschrijf je hypothese: Because... We might... Which will lead to ...
Indentificeer de mogelijkheden die nodig zijn om oplossing te ontwikkelen/af te leveren

##### Co-Create
Identificeer de requirements op verschillende levels: front & back stage
Creatie van een Story map

##### Verify


##### Prioritize
Geef priotiteiten aan de story map die de visie van de business reflecteert: Minimal Viable Product/Service
Voeg de visie van de klant toe: Minimal Lovable Product/Service
Daag de uitvoerbaarheid uit van de oplossing, visualiseer de verandingsimpact
Definieer implemenatie scenario's: creatie van een RoadMap(Proof of Concept, beta,
alpha,basic, full-featured)

#### Beheren Risico's
**Table stakes**: Functionaliteiten die nodig zijn om op de markt de kunnen gaan
**Enablers**: User stories met hoog risico bekeken van een technisch punt
**Exceptions & delighters**: Functionaliteiten die nodig zijn om te kunnen concurreren en klanten
te winnen
**Probe**: Een vooruitstekende functie om gebruikshypothese te valideren

/* voorbeeld storymap toevoegen */

## Use Cases 2.0 (Use case slices)

**Use Case 2.0**: een schaalbare, agile werkwijze die use cases gebruikt voor het va
stleggen van een set requirements (systeemeisen) en het aansturen van de incrementele ontwikkeling van het systeem dat hierin moet voorzien.
Use case 2.0 is **lichtgewicht, schaalbaar, veelzijdig en gemakkelijk te gebruiken.** Met deze aanpak worden use cases effectiever gepresenteerd, geadresseerd en gemanaged en worden ze geschikt voor een agile projectaanpak. Er gelden 6 principes die het hart vormen voor het succesvol toepassen van use cases: 

- Houd de informatie-uitwisseling **simpel** door het vertellen van verhalen (gebruik van stories)
- Overzie en begrijp het **grote geheel**
- **Focus** op de **(business) value**
- Bouw het systeem in **use case slices**
- Lever het systeem **incrementeel** op
- **Flexibiliteit**, ieder project kent eigen karakteristieken en vergt daarom een eigen aanpak

Een **use-case slice** is de selectie van een of meer verhalen uit een use case om een  werkpakket te vormen dat een duidelijke toegevoegde waarde heeft voor een klant.

/* Hier komen afbeeldingen */
De figuur laat de incrementele ontwikkeling van een systeem zien. Het eerste increment bevat maar één slice: de eerste slice van use case 1. Het tweede increment voegt een andere slice van use case 1 toe en de eerste slice van use case 2. Meer slices worden toegevoegd om zo het derde en vierde increment te vormen. Het vierde increment wordt compleet en bruikbaar genoeg geacht om te worden uitgeleverd.

### Stappenplan

- Beschrijf de **actoren** en de **use cases**
- **Verdeel** de use case in **use case slices**
- Voorbereiden van de use case slice
- Analyseer de use case slice
- Implementeer de software (voor een slice)
- **test het systeem** (voor een **slice**)
- **test het systeem in zijn geheel**
- **inspecteer en wijzig de use cases**