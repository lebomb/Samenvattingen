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
    