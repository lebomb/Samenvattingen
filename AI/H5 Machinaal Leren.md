# Artificiële Intelligentie
## Hoofdstuk 5: Machinaal Leren
---

### Machinaal Leren
> Het deelveld van de artificiële intelligentie dat computers de **mogelijkheid geeft om te leren zonder dat ze hiervoor _expliciet_ geprogrammeerd zijn**.  

### 3 Types van Machinaal Leren

1. **Gesuperviseerd Leren:** op basis van een **gelabelde dataset** een **hypothese** op bouwen waarmee, voor een nieuwe invoer het label kan voorspeld worden. 
    * Wanneer dit label een (reëel) getal is, dan spreekt men van een **Regressieprobleem**. 
    * Wanneer het label 1 van een (klein) aantal voorgedefinieerde klassen is dan spreekt men van een **Classificatieprobleem**. 
    In hetgeval dat er slechts 2 klassen zijn, spreekt men van een **binair** classificatieprobleem. 

    ![alt text](http://users.hogent.be/~427143la/images/GesuperviseerdSchema.JPG "Schema")

2. **Ongesuperviseerd Leren**: heeft de taak een **structuur te ontdekken** in een **ongelabelde** dataset. Voorbeelden:
    * **Clustering**: Het ontdekken van coherente groepen. (= Meest voorkomende)
    * **Anomaliedetectie**: Het ontdekken van zaken die niet conform zijn met het verwacht patroon of andere zaken in de dataset.
    * **Primaire Componenten Analyse**
    
3. **Reinforcement Learning**: Werkt in plaats van datasets met **beloningssignalen**, die negatief zijn wanneer de agent een “slechte” handeling stelt en positief wanneer de agent een goede handeling stelt. Eenvoudig gezegd bestaat de taak van reinforcement learning erin om te **leren welke acties**, i.e. welk beleid, **leiden tot de hoogste totale beloning.**


### Evaluatie van Hypothesen voor Gesuperviseerd Leren.