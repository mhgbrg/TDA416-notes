# Länkade listor

En länkad lista kan implementeras med en tom startnod (typ ett ankare). Den här noden kan användas för att spara extra information, som t.ex. pekare till första (`head`), sista (`tail`), längd (`length`), minsta (`min`), största (`max`) elementet osv.

När man implementerar en *iterator* så använder man en referens till *actual*, alltså den nod du har kommit till i iterationen.

Den simplaste implementation är att referensen till listan helt enkelt pekar till det första elementet i listan.

I länkade listor är det enkelt att sätta in element mitt i listan, så länge du redan har hittat platsen som elementet ska läggas in på. Det gäller alltså att du redan har en referens till noden som ligger precis före eller efter platsen du vill lägga till på.

**Fördelar med länkade listor:**
+ Konstant tid för att sätta in element och ta bort element (om du hittat platsen för det).
+ Kräver ej kontinuerligt minne (till skillnad från en array där alla element sparas i rad i minnet). Det här är bra när du inte vet hur många element du ska ha i listan när du skapar den.

**Nackdelar med länkade listor:**
- Kräver mer minne än array (eftersom man utöver elementen måste spara pekare till alla element).
- Att noderna ej sparas kontinuerligt i minnet kan vara dåligt, eftersom processorn *cachar* ett helt block av minne när den jobbar. När elementen ligger utspridda i minnet så måste man hela tiden läsa in nya minnesblock för att komma åt nya element.
- I enkellänkade listor kan man inte gå baklänges, men det fixad med hjälp av dubbellänkade listor. De kräver dock mer minne.
- Sökning i listor tar `O(n)` tid, eftersom alla element måste kollas igenom för att hitta ett specifikt element.

## Premature optimization is the root of all evil

Optimisera inte för tidigt i processen. Det är bättre att först ta fram en naiv lösning, och sedan utveckla den så att den blir effektivare.

## När ska man använda länkade listor?

Länkade listor är bra att använda när man vill kunna sätta in och ta ut element mitt i listan, men inte behöver söka efter element. Det kan till exempel vara när du får ut referensen till ett element via en kö och sen vill komma åt elementen som ligger precis bredvid.

De kan t.ex. vara bar för att lagra en todo-list där varje uppgift kan generera en ny uppgift som ska göras direkt efter.

    * todo 1
    * todo 2 <-- you are here
    * todo 3
    
    ---
    
    * todo 1
    * todo 2 <-- you are here
        * todo 2.1 <-- inserted directly after current item
    * todo 3

Länkade listor kan användas för att lagra polynom (t.ex. 5x^12 + 2x^9 + ...). Varje term lagras som en nod med koefficient och exponent. För att addera två polynom kan man helt enkelt slå ihop (*merge*) två länkade listor.

Det är dock oftast bäst att använda en dynamisk array (t.ex. `ArrayList`), och om det funkar hyfsat så är det lika bra att köra på det.

## Static vs. non-static inre klasser

En inre `static` klass kan inte komma åt den yttre klassens instansvariabler, en `non-static` klass kan det. Använd `static` när den inre klassen ska kunna användas separat från ett element av den yttre klassen, eller när det helt enkelt är onödigt för den inre klassen att veta något om den yttre.

Används t.ex. för `Node` i `LinkedList` och `HashMap`.

## Självorganiserande lista

Man kan ibland förbättra prestandan hos en länkad lista genom att lägga element som accessas ofta i början av listan. Det här bygger på idén att accesserna ofta i 80% av fallen är till 20% av elementen.

Det finns tre olika metoder för att åstadkomma en självorganiserande lista:
1. **Move-to-front** - när ett elements söks så flyttas det till början av listan.
2. **Transpose** - när ett element söks så flyttas det ett steg längre fram i listan.
3. **Count** - varje element har en `count` som ökas varje gång det accessas. Listan sorteras sedan efter alla elements count (det accessade elementet flyttas fram tills dess count är lägre än elementet innans).