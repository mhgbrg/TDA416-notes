# Grafalgoritmer

## Prims algoritm

*Prims algoritm* är en algoritm för att hitta ett minimum spanning tree i en **oriktad graf**. Man utgår alltid från en mängd `S` med besökta noder (`S` kommer bara innehålla en nod i första iterationen). Algoritmen går sedan ut på att alltid ta den "billigaste" kanten som går mellan den här mängden och någon av de resterande noderna i grafen (någon nod i mängden `V-S`). Om det finns två minimala kanter med samma vikt mellan `S` och `V-S` så spelar det ingen roll vilken av dem man väljer.

Se [Wikipedia](https://en.wikipedia.org/wiki/Prim%27s_algorithm) för mer information.

Tidskomplexiteten för Prims algoritm är `O(|E| + |V|*log(|V|))` om man använder sig av en *Fibonacci heap* och metoden adjacency list för att representera grafen. Om man istället använder en *Binary heap* så blir komplexiteten `O((|V| + |E|)*log(|V|)) = O(|E| * log(|V|))`.

## Krukals algoritm

*Kruskals algoritm* är en annan algoritm för att hitta ett minimum spanning tree i en oriktad graf. Den här algoritmen går ut på att hitta en minimal kant som kopplar samman två **fria träd** i en **fri skog**.

För att få fram ett komplett minimum spanning tree så kollar man först på alla noder som separata fria träd utan kanter mellan varandra. Man tar sedan sen "billigaste" kanten som inte har tagits tidigare och kopplar samman de två noderna som kanten går mellan. Vi får alltså då `n-1` fria träd (där alla förutom ett av dem endast består av en nod).

Vi upprepar sedan algoritmen tills vi bara har kvar ett träd. I varje iteration måste vi dock se till att den minimala kanten vi får ut inte kopplar samman två noder i samma fria träd. Om den gör det så bortser vi helt enkelt från den. Det kan vi göra eftersom vi alltid börjar med de billigaste kanterna (i varje iteration kommer alla de fria träden vara minimum spanning trees för just de noderna i grafen).

Den här algoritmen är mycket lättare att resonera kring när man visualiserar vad som händer. [Wikipedia](https://en.wikipedia.org/wiki/Kruskal%27s_algorithm) har bra beskrivningar och illustrationer.

Tidskomplexiteten för Kruskals algoritm är `O(|E| * log(|V|))` eller `O(|E| * log(|E|))` (båda ger samma komplexitet).

### Implementering

För att representera mängden med fria träd i algoritmen kan vi skapa en array med ett element per nod i grafen. Varje element pekar sedan på en mängd med kanter (som från början är tom för alla noder). När du "kopplar ihop" två noder genom att välja en minimal kant så lägger du till kanten i mängden för den första noden, och pekar om den andra noden så att den pekar på samma mängd. När alla element i arrayen pekar på samma mängd så är algoritmen klar (och du har ett minimum spanning tree!).

## Skillnader mellan Prims och Kruskals algoritmer

Man brukar säga att Kruskals algoritm är mer *generell* än Prims, men att den är lite mer komplicerad att implementera. Kruskals algoritm kan hitta en *minimum spanning forest* i en graf som inte är sammanhängande, vilket inte Prims algoritm kan.

Skillnaden i tidskomplexitet gör att Prims algoritm är bra för täta grafer, medan Kruskals är bättre för glesa grafer. Det här gör att Kruskals algoritm används oftare, eftersom det är vanligare med glesa grafer.

## Dijkstras algoritm

*Dijkstras algoritm* är en algoritm för att hitta den kortaste vägen mellan två noder i en graf. Den ursprungliga versionen av Djikstras algoritm hittade bara den kortaste vägen mellan en startnod och en slutnod, men uppdaterade versioner av algoritmen kan även hitta kortaste vägen från en nod till samtliga andra noder i grafen.

Det algoritmen går ut på är att hela tiden hålla reda på hur lång den kortaste vägen från startnoden till alla noder som algoritmen har besökt är. Den här kortaste vägen kan uppdateras upprepade gånger innan algoritmen är klar, eftersom det kan finnas flera olika vägar till samma nod som är olika dyra.

Hur vet man när man har hittat den kortaste vägen till en nod utan att behöva besöka alla noder i hela grafen? När du har besökt alla grannar till en nod så har du utforskat alla sätt att komma till den noden, och noden är då "klar". När slutnoden har markerats som klar (algoritmen har alltså besökt alla grannar till slutnoden) så är hela algoritmen klar.

För att kunna backtracka från slutnoden till startnoden (för att ta reda på vilken den kortaste vägen var) så sparar för varje nod en "previous node" - alltså vilken nod du gick från för att komma till noden. När man hittar en kortare väg så uppdaterar man den här parent-referensen.

More information can be found at [Wikipedia](https://en.wikipedia.org/wiki/Dijkstra%27s_algorithm).