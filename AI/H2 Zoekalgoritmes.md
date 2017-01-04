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

**Algoritme**

![alt text](http://users.hogent.be/~427143la/images/AlgoritmeGraafgebaseerd.PNG "Algoritme 2.1")
