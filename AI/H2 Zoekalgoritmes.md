# Artificiële Intelligentie
## Hoofdstuk 2: Zoekalgoritmes
----

### Inleiding
Een **zoekprobleem** bestaat uit volgende elementen:
* een **toestandsruimte S** die alle mogelijke toestanden bevat.
* een **verzameling** van mogelijke **acties**
* een **transitiemodel** dat zegt wat het effect is van het uitvoeren van een actie op een bepaalde toestand:

        T : (S, A) →  S : (s,a)  ↦ s'
    wanneer *s'* bereikt wordt door het uitvoeren van een actie *a* op een toestand *s* dan wordt *s'* een opvolger van *s* genoemd. (= Deterministische omgeving: wanneer *s* en *a* gekend zijn is er juist 1 opvolger: *s'*)
    Hetuitvoeren van een actie op een bepaalde toestand heeft meestal een bepaalde **kost**:

        C : (S, A) →  ℝ: (s,a)  ↦ c

* Een **initiële toestand** *sO* ∈ S, dit is de toestand van waaruit het zoeken zal vertrekken
* Een **doeltest**. Dit is een functie die voor elke toestand *s* aangeeft of het doel bereikt is of niet. Een toestand waarvoor de doeltest voldaan is noemen we een **doeltoestand**

De toestandsruimte *S* kan samen met de transitie- en kostfunctie gebruikt worden om een **toestandsruimtegraaf** *G* te maken. In deze graaf stellen knopen de toestanden voor en twee knopen zijn verbonden door een gerichte boog wanneer de ene knoop de opvolger is van de andere door het uitvoeren van een bepaalde actie. (kost boog = kost actie). **Elke toestand komt juist 1 keer voor.** (abstract concept: dergelijke graaf is veel te groot om bij te houden in het geheugen van een computer)

### Algemene Zoekalgoritmes
####Boomgebaseerd zoeken
Het algemene algoritme houdt een lijst bij van mogelijke partiële oplossingen die nog verder uitgewerkt (geëxpandeerd) moeten worden (= **open lijst**). Bij de start van de uitvoering bestaat deze open lijst enkel uit het plan corresponderend met de initiële toestand van het zoekprobleem. Bij elke iteratie van het algoritme wordt een plan uitgekozen uit deze lijst (volgens 1 of andere strategie) en wanneer de eindtoestand van het gekozen plan voldoet aan de doeltest, stopt het algoritme. Wanneer dit niet het geval is dan worden de plannen voor alle opvolgers van de eidntoestand van het gekozen plan toegevoegd aan de open lijst, waardoor deze ook beschikbaar worden voor expansie. Wanneer de open lijst op een bepaald moment leeg is dan geeft het algoritme aan dat er geen oplossing werd gevonden.
> Conceptueel bouwen we dus een **zoekboom** op, elke top van deze boom stelt een sequentie van acties voor. Het is dus de bedoeling zoekproblemen op te lossen met een zo klein mogelijke zoekboom. Elke top stelt dus een plan voor dat o.a. de huidige toestand bijhoudt, dezelfde toestand kan, en zal, in de het algemeen *meerdere malen* voorkomen in een zoekboom


**Algoritme:**

Invoer: Een zoekprobleem P.

Uitvoer:  Een sequentie van acties die een oplossing is van het zoekprobleem of error wanneer er geen oplossing werd gevonden.
```python
1: function TREESEARCH(P)
2:      f  <-  nieuwe lege lijst                                    > De open lijst
3:      f.ADD(nieuw plan gebaseerd op initiële toestand P)
4:      while f ≠ ∅ do
5:          c <- f .CHOOSEANDREMOVEPLAN                             > Kies het volgende plan
6:          if P.GOALTEST(c.GETSTATE) = true then
7:              return GETACTIONSEQ(c)
8:          else
9:              for (s, a) ∈ c.GETSTATE.GETSUCCESSORS do
10:                 f.ADD(nieuw plan gebaseerd op (s, a) en c)
11:             end for
12:         end if
13:     end while
14: return error: geen oplossing gevonden                           > Open lijst is leeg.
15: end function
```

**Implementatie van een plan**

Zoals reeds gezegd stelt elke top van de zoekboom een sequentie van acties voor, maar het is **niet nodig** om in elke top het **volledige pad op te slaan**. Door gebruik te maken van het **vorige pad in combinatie met de laatste gekozen actie**, kan je zo het volledige plan opstellen.

Voor de implementatie gebruiken we een klasse **Plan** dat bestaat uit **vier velden**:
1. **Huidige toestand**
2. **Laatst gekozen actie _a_**, deze is enkel leef voor het plan geassocieerd met de initiële toestand.
3. **Voorganger/ouder** van dit plan. Een referentie naar het plan waarvan dit plan is afgeleid door het toepassen van de huidige actie a.
4. **Totale kost _g_** van dit plan.  Strikt genomen kunnen we deze kost ook berekenen door het volgen van de voorganger-referenties, maar deze berekening heeft echter een uitvoeringstijd die lineair is in het aantal acties van het plan.

**Criteria voor Zoekalgoritmes**

Zoekalgoritmes kunnen op verschillende manieren worden geëvalueerd, deze vier worden vaak gebruikt:

1.  Een zoekalgoritme is **compleet** wanneer het algoritme, voor elk zoekprobleem met een oplissing, effectief een oplossing vindt.

2. Een zoekalgoritme is **optimaal**, wanneer het niet enkel *een* oplossing vindt, maar steeds een optimale oplossing teruggeeft voor elk zoekprobleem met een oplossing.

3. De **tijdscomplexiteit** van een zoekalgoritme bepaalt de uitvoeringstijd van het algoritme. We nemen aan dat de uitvoeringstijd evenredig is met het aantal gegenereerde toppen.

4. De **ruimtecomplexiteit** van een zoekalgoritme bepaalt de hoeveelheid geheugen die het algoritme nodig heeft tijdens de uitvoering. Dit wordt meestal uitgedrukt als het maximaal aantal toestanden dat gelijktijdig moet worden bijgehouden.

**Maten die gebruikt wordt om tijds- en ruimtecomplexiteit van zoekalgoritmes uit te drukken:**
* De **vertakkingsfactor _b_** geeft het maximaal aantal opvolgers van een top in de zoekboom.
* Een **doeltop_d_** (= diepte van de meest ondiepe top waarvan de toestand een doeltoestand is).
* De **maximale lengte _m_** is de maximale lengte (gemeten  als het aantal genomen acties) van een pad in de toestandsruimte.

Eigenschap: Het aantal toppen in een zoekboom met vertakkingsfactor *b* en maximale diepte *m* wordt gegeven door:
![alt text](http://users.hogent.be/~427143la/images/Eigenschap2-1.PNG "Eigenschap 2.1")

*Opmerking:* De laag met diepte *m* in een zoekboom met vertakkingsfactor *b* bevat *b^m* toppen, dit is meer dan alle voorgaande lagen samen. Die bevatten in totaal slechts:

![alt text](http://users.hogent.be/~427143la/images/Eigenschap2-2.PNG "Opmerking 2.1")

toppen.

#### Graafgebaseerd zoeken
Het grootste probleem van boomgebaseerd zoeken is dat dit algoritme **niet onthoudt waar het reeds geweest is**. Dit kan oneindige lussen veroorzaken, en in vele andere gevallen een grote hoeveelheid werk herhaaldelijk uitvoeren.

De oplossing hiervoor is gebruik maken van een **gesloten lijst** in plaats van een open lijst. Deze lijst onthoudt welke toestanden reeds geëxpandeerd zijn. **Let op!** Een gesloten lijst houdt **toestanden** bij, geen plannen zoals een open lijst.

> Bij graafgebaseerd zoeken wordt elke toestand **hoogstens 1 maal** geëxpandeerd. Wanneer een plan an de open lijst wordt gehaald dat een toestand bevat die reeds geëxpandeerd is, dan wordt deze niet opnieuw geëxpandeerd.

#### Algoritme

![alt text](http://users.hogent.be/~427143la/images/AlgoritmeGraafgebaseerd.PNG "Algoritme 2.1")

### Blinde Zoekmethoden
> Blinde Zoekmethoden kunnen enkel gebruikmaken van de informatie die verschaft wordt door de definitie van het zoekprobleem. Ze beschikken niet over extra informatie die hen kan helpen bij het zoekproces. Volgende zoekmethoden worden besproken:

#### Breedte Eerst Zoeken
Bij breedte eerst zoeken wordt voor de open lijst een *wachtrij* gebruikt. Dit is een FIFO datastructuur. Bij breedte eerst zoeken wordt de zoekboom systematisch laag per laag opgebouwd, hierdoor zal het algoritme steeds een oplossing vinden voor elke zoekprobleem dat effctief een oplossing heeft. Dit betekent dat breedte eerst een **compleet zoekalgoritme** is. Het algoritme vindt steeds de meest ondiepe doeltop. (= *minimaal aantal acties*). Wanneer acties een *verschillende kost* hebben is dit dus *niet* noodzakelijk *de oplossing met de kleinste kost*, maar is dus wel optimaal wanneer elke actie dezelfde kost heeft.

Breedte eerst zoeken is **exponentieel in de diepte van de meest ondiepe doeltop**. Het maximaalaantal toppen dat moet worden bijgehouden in de open lijst wordt bereikt wanneer men de doeltop expandeert. Op dit moment wordt zo goed als de volledige laag op diepte *d* + 1 bijgehouden in de open lijst.

Bij **graafgebaseerd breedte eerst zoeken** kan men veel tijd winnen (tov boomgebaseerd) wanneer veel toestanden meerdere malen voorkomen in de zoekboom. Het extra geheugen dat men moet spenderen aan het bijhouden van de gesloten lijst weegt niet op tegen de tijdswinst die men kan maken. Om deze reden wordt **breedte eerst zoeken meestal uigevoerd in zijn graafgebaseerde versie**.

![alt text](http://users.hogent.be/~427143la/images/BreedteEerstZoeken.PNG "Breedte Eerst Zoeken")

#### Diepte Eerst zoeken
Diepte eerst zoeken lijkt in zekere zin op breedte eerst zoeken, maar hier gebruikt men een **LIFO** structuur voor het bijhouden van de **open lijst**. Deze stapel zorgt ervoor dat men zo snel mogelijk zo diep mogelijk in de boom afdaalt.

Diepte eerst zoeken genereert steeds een **linkerdeel van de boom**. Wanneer *m* eindig is en de enige doeltop helemaal rechts onderaan in de boom zit, dan worden alle toppen van de boom gegenereerd. In het *slechtste geval* is de tijdscomplexiteit dus b^m. dit is dezelfde exponentiële tijdscomplexiteit als bij breedte eerst.

![alt text](http://users.hogent.be/~427143la/images/DiepteEerstZoeken.PNG "Diepte Eerst Zoeken")

In *bepaalde gevallen* kan diepte eerst zoeken toch in een **oneindige lus** geraken. Dus **diepte eerst zoeken is niet-compleet** en bijgevolg **niet optimaal**. Zelfs wanneer diepte eerst een oplossing vindt is deze niet gegarandeerd de optimale oplossing. (Meest linkse top kan dieper zijn dan een rechter doeltop).

**Diepte eerst scoort wel goed op _benodigd geheugen_**, wanneer een bepaalde top geëxpandeerd wordt behoren enkel de broers van zijn voorouders tot de open lijst. (maximaal tijdscomplexiteit: *m* niveaus x *b* broers -> lineair  -> veel beter dan exponentieel van breedte eerst)

#### Iteratief verdiepen
 > Iteratief verdiepen is geen directe toepassing van het boomgebaseerd zoekalgoritme dat hier reeds gegeven werd. In essentie is het een lus rond diepte-eerst zoeken waarbij het zoekproces wordt afgebroken wanneer een bepaalde diepte bereikt wordt. (= **Diepte Gelimiteerd Zoeken**)

 Volgend algoritme geeft een implementatie voor diepte-gelimiteerd zoeken.

![alt text](http://users.hogent.be/~427143la/images/IteratiefVerdiepenAlgoritme.PNG "Iteratief verdiepen")

Het algoritme heeft een bijzondere returnwaarde 'hit boundary' om aan te geven dat er geen oplossing werd gevonden binnen de opgegeven dieptelimiet, maar dat er wel *eventueel* een oplossing gevonden kan worden wanneer de maximale toegelaten diepte wordt verhoogd.

Dit gebeurt in volgend algoritme: **Iteratief Verdiepen**

![alt text](http://users.hogent.be/~427143la/images/IteratiefVerdiepenAlgoritme2.PNG "Iteratief verdiepen")

In tegen stelling tot wat je op eerste zicht zou denken, blijft de hoeveelheid extra werk beperkt. `Check Cursus 2.4`

Iteratief verdiepen is een **compleet zoekalgoritme** en zal steeds de oplossing met het minste aantal acties vinden. Het algoritme is in het algemeen niet optimaal buiten het geval dat alle acties de zelfde kost hebben. Het algoritme heeft een **exponentiële tijdscomplexiteit** maar slechts een lineaire ruimtecomplexiteit.

#### Uniforme Kost Zoeken
> Uniforme Kost Zoeken tracht het probleem dat breedte eerst heeft wanneer acties een verschillende kost hebben op te lossen door *steeds het plan te expanderen waarvoor de totale kost van dit plan minimaal is*. De **open lijst** wordt hier m.a.w. geïmplementeerd aan de hand van een **prioriteitswachtrij,** een kleinere kost betekent een grotere prioriteit. (~ Algoritme van Dijkstra)

Uniforme kost zoeken is (*wanneer alle acties een kost hebben die groter of gelijk aan één of andere positieve e is*)  een **compleet en optimaal algoritme**. De tijd- en ruimtecomplexiteit is *O(b^(1+C/e))*, waarbij *C* de kost van de optimale oplossing voorstelt.

### Geïnformeerde Zoekmethoden
> Geïnformeerde zoekmethoden gaan gebruik maken van een heuristiek om een goede indicator of schatting te hebben voor het zoekproces efficiënter te laten verlopen.

#### Heuristieken
Een heuristiek *h* is een afbeelding van de verzameling toestanden *S* naar de verzameling van niet-negatieve reële getallen R+, i.e.

        *h: S -> R+ : s -> h(s)*


#### Eigenschappen van Heuristieken
De heuristiek *h*: *S -> R+*:
* is **toelaatbaar** als voor elke toestand *s* geldt dat *h(s) <= C(S)* waarbij *C* de kost van een optimale oplossing voorstelt van *S* naar een doeltoestand.
        * Wanneer *h* toelaatbaar is, dan is *h(g) = 0* voor elke doeltoestand *g*.
* is **consistent** als voor elke doeltoestand *g* geldt dat *h(g) = 0* en als bovendien voor elke toestand *s* en elke actie *a* op *s* met *s' = T(s,a)* geldt dat:

![alt text](http://users.hogent.be/~427143la/images/consistent.PNG "consistent")

        bv. Wanneer we de heuristiek beschouwen als "in vogelvlucht op doel afgaan" dan is een heuristiekconsitent wanneer het steeds korter is om in vogelvlucht op doel af te geen ( -> *h(s)*) dan om eerst een actie te ondernemen adhv het transitiemodel (*c(s,a,s')*) en dan in vogelvluchtop doel af te gaan (*h(s'*).

> Een *consistente heuristiek is altijd toelaatbaar*, maar dit wil niet zeggen dat elke toelaatbare heuristiek ook consistent is.
