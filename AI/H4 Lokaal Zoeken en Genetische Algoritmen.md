# Artificiële Intelligentie
## Hoofdstuk 4 : Lokaal Zoeken en Genetische Algoritmen
---

## Lokale Zoekmogelijkheden
> Worden gebruikt wanneer het **oplossingspad irrelevant** is aangezien deze zich niet bezig houdt met het oplossingspad maar met het toestanden. Dergelijke algoritmen houden dus 1 (of een beperkt aantal) huidige toestand bij en vervangen in het algemeen deze huidige toestand door 1 van zijn opvolgers in de hoop zo een *optimale* toestand te bereiken.

Hoewel lokale zoekmethoden minder systematisch zijn dan de reeds besproken Zoekalgoritmes, hebben ze twee grote voordelen:

1. Ze gebruken een beperkte (constante) hoeveelheid geheugen
2. Ze kunnen vaak redelijk goede oplossingen (= toestanden) vinden in zeer grote of continue toestandsruimten waar de systematische zoekalgoritmes falen.

**Lokale zoekalgoritmes** worden dus voornamelijk gebruikt voor **optimalisatieproblemen** waar er een doelfunctie is die met elke toestand een waarde associeert. Uitgeschreven:

*h* : *S* -> R : *s* -> *h(s)*

Het is deze doelfunctie die geoptimaliseerd moet worden.

### Globaal Maximum
Een toestand *s* is een **globaal maximum** voor een doelfunctie *h* als **voor alle toestanden** *s'* uit *S* geldt dat:

*h(s)* >= *h(s')*

### Lokaal Maximum
Een toestand *s* is een **lokaal maximum** voor een doelfunctie *h* als **voor alle opvolgers** van *s'* geldt dat:

*h(s)* >= *h(s')*

*Gelijkaardige definities gelden voor globaal en lokaal minimum*


Elk maximalisatieprobleem kan echter omgezet worden in een equivalent minimalisatieprobleem en omgekeerd. Bijvoorbeeld door de negatie te nemen van de doelfunctie en omgekeerd. Dit betekent dat het geen beperking is om de algoritmen slechts op 1 manier te schrijven.

### Hill Climbing
Hill climbing is het meest eenvoudige zoekalgoritme, het algoritme start in een willekeurige begintoestand en in elke iteratie vervangt het deze toestand door de beste toestand onder zijn opvolgers op voorwaarde dat deze beter is. Het algoritme stopt wanneer er geen verbetering meer mogelijk is.

Voordelen: **eenvoudig en snel**, en het kan initieel (vanuit een slechte toestand) vaak snel vooruitgang boeken.

#### Algoritme
![alt text](http://users.hogent.be/~427143la/images/AlgoritmeHillClimbing.PNG "HillClimbing")

Er zijn verschillende redenen waarom hill climbing kan falen in het vinden van een globaal maximum:

        * Lokale maxima: het hill climbing algoritme loopt cast wanneer een lokaal maximum wordt bereikt omdat in zo'n lokaal maximum geen enkele opvolger strikt beter is.
        * Plateaus: een plateau is een 'vlak stuk' in het landschap, in bovenstaande implementatie wordt er niet verder gegaan wanneer de beste opvolger even goed is als de huidige toestand. Men kan het algoritme wel eenvoudig aanpassen om wel zo'n zijwaartse stap te nemen. Hierbij moet men echter maatregelen nemen om te voorkomen dat het algoritme vastraakt in een oneindige lus (op zo'n plateau), Bijvoorbeeld wanneer de acties omkeerbaar zijn. De eenvoudigste manier om dit op te lossen is om het aantal toegelaten zijwaartse stappen te beperken.

> Het is duidelijk dat Hill Climbing **geen compleet** algoritme is, het vindt niet steeds een globaal optimum. Echter, door gebruik te maken van *random herstarten* kan het algoritme compleet worden gemaakt met waarschijnlijkheid 1. Dit laatste is vooral van theoretisch belang, in de praktijk kan het veel te lang duren voor effectief een globaal optimum wordt bereikt.

### Simulated Annealing

In elke iteratie wordt een random opvolger gegenereerd: wanneer deze strikt beter is dan de huidige oplossing dan wordt deze steeds aanvaard als nieuwe huidige toestand. Wanneer de random opvolger niet beter is dan de huidige oplossing dan aanvaarden we deze met een zekere waarschijnlijkheid. Deze waarschijnlijkheid wordt beinvloed door 2 twee factoren. Ten eerste is er het tijdstip in het algoritme en ten tweede is er de mate waarin de potentiële opvolger slechter is dan de huidige toestand.

De aanvaardingswaarschijnlijkheid daalt naarmate het algoritme vordert: in het begin zijn we tamelijk bereid om neerwaartse stappen te zetten maar naarmate het einde dichterbij komt, worden we meer weigerachtig om neerwaartse stappe te zetten.

![alt text](http://users.hogent.be/~427143la/images/Aanvaardingswaarschijnlijkheid.PNG "Aanvaardingswaarschijnlijkheid")

Op hetzelfde tijdstip in het algoritme is de waarschijnlijkheid ook kleiner wanneer de opvolger minder goed is, anders gezegd: **hoe slechter de opvolger hoe kleiner de kans om deze te aanvaarden.**

#### Algoritme
![alt text](http://users.hogent.be/~427143la/images/AlgoritmeSimulated.PNG "Simulated Annealing")

`Bekijk voorbeeld cursus 4.1.2`

### Gradient Descent

`Bekijk in Cursus`

## Genetische Algoritmen

Bij de klassieke zoekalgoritmen wordt een open lijst bijgehouden die al snel veel te groot wordt. Aan de andere kant zijn hill climbing en simulated annealing misschien iets te ver in de andere rihting doorgeslagen, in die zin dat ze slechts 1 toestand bijhouden.

### Populatie en Encodering

### Fitnessfunctie en Selectiemechanisme

### Recombinatie en Mutatie

### Eenvoudige implementatie

### Besluit en Toepassingen
