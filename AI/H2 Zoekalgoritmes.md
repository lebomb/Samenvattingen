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
**Boomgebaseerd zoeken** 
