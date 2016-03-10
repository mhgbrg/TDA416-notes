# Sammanfattning

## Abstrakta datatyper

En abstrakt datatyp är den matematiska definitionen av en datastruktur. Det som definieras i en abstrakt datatyp är vilka operationer som datatypen har stöd för och vilka egenskaper den har.

Abstrakta datatyper representeras av interface i objektorienterade program. T.ex. så är `List` en abstrakt datatyp medan den konkreta implementationen `ArrayList` är en datastruktur.

### Generics & Comarable/Comparator

För att kunna utveckla generiska datatyper och strukturer i Java så använder man *generics*. När man initierar en instans av en datatyp så får man specificera vilken typ av element som ska kunna lagras i den.

För att jämföra två element av en generisk typ så använder man `Comparable<E>` och `Comparator<E>`. `Comparable<E>` är ett interface som innebär att ett element kan jämföras med ett annat element av typen `E`. Själva jämförelsen görs med metoden `int compareTo(E other)`, som returnerar ett negativt tal om `this` kommer före `other`, ett positivt tal om `this` kommer efter `other`, och 0 om de ska ligga på samma plats. `Comparator<E>` fungerar på liknande sätt, men den tillhandahåller istället en metod `int compare(E o1, E o2)` som jämför två objekt med varandra.

## Rekursion

### Rekursiva funktioner

Rekursion i funktioner betyder att man har en funktion som anropar sig själv. Det här används flitigt vid t.ex. sortering av listor. Fördelen med att använda rekursion istället för en iterativ lösning kan vara att det blir lättare att se hur delproblemet ser ut. Man behöver bara lösa det generella fallet och basfallet så är lösningen i princip klar. Rekursion har dock även sina nackdelar, bland annat att de rekursiva anropen kan äta upp onödigt mycket minne.

### Rekursiva datatyper

En rekursiv datatyp är en datatyp där varje element kan betraktas som en egen instans av datatypen. T.ex. så kan man betrakta varje nod i ett träd som ett eget delträd. För att manipulera rekursiva datatyper så använder man oftast rekursiva metoder.

## Komplexitetsanalys

Det man mäter med en komplexitetsanalys är hur mycket tidsåtgången för en funktion ökar när indatans storlek växer. De olika klassificeringarna som man brukar använda är (ordnade efter stigande ökning):

* O(1) - konstant
* O(log(n)) - logaritmisk
* O(n) - linjär
* O(n * log(n)) - lin-log
* O(n^2) - kvadratisk
* O(n^3) - kubisk
* O(n^k) - polynomiell
* O(c^n) - exponentiell
* O(n!) - faktoriell

Den formella definitionen för ordonotationen är:

    Om f(n) tillhör O(g(n)) finns N och k så att för alla n > N gäller f(n) < k * g(n).

I klarspråk betyder det att `g(n)` multiplicerat med en fast konstant alltid ska vara större än `f(n)` för tillräckligt stora `n`. `g(n)` måste alltså växa lika snabbt eller snabbare än `f(n)`.

### Räkneregler

De räkneregler som kan användas vid komplexitetsanalys är:

* O(k * n) = O(n) - konstanter kan tas bort.
* O(n^2 + n) = O(n^2) - endast den snabbast växande termen behöver vara kvar.
* O(log2(n)) = O(log3(n)) - alla logaritmfunktioner är likvärdiga, oavsett bas.

När man gör komplexitetsanalys för loopar kan man sätta upp matematiska formler för varje loop. Om man har nästlade loopar blir även summorna nästlade. När man sedan räknar ut summorna så kan man förenkla bort onödiga termer i varje steg.

Vid analys av rekursiva funktioner så kommer man inte lika lätt undan. Här är det oftast lättare att räkna ut komplexiteten för **samtliga** rekursiva anrop samtidigt. Alltså om man kan resonera sig fram till att en funktion körs exakt en gång för varje nod i en graf så blir komplexiteten för samtliga rekursiva anrop till den funktionen `O(|V|)`.

## Listor

Listor är en abstrakt datatyp som innehåller ett antal element med en given inbördes ordning. Det här betyder inte att listan är sorterad, utan bara att ordningen på elementen spelar roll.

### Representation

Två vanliga sätt att implementera listor på är med en **array** eller som en **länkad lista**. Om man väljer att använda en array så får man konstant tid för access av elementet på ett givet index, och även konstant tid för tillägg i slutet av listan. Däremot så tar det linjär tid att lägga till saker mitt i listan (speciellt dyrt i början), eftersom alla efterkommande element då måste flyttas framåt ett steg i arrayen.

En array måste skapas med en bestämd storlek, och dess längd kan inte ändras efter skapandet. För att ha en lista med dynamisk längd som backas av en array så kan man göra så att man ökar listans storlek med en bestämd storleksordning (t.ex. dubbla) när arrayen blir full. Man måste då allokera en helt ny array och kopiera över alla gamla värden, vilket går på `O(n)` tid. Det här görs dock såpass sällan, så den amorterade tidskomplexiteten för insättningn i slutet av en sådan lista är fortfarande `O(1)`.

När man implementerar listan som en länkad lista så håller varje nod i listan reda på vilken nod som kommer efter (och före i en dubbellänkad lista). Man kan ge användaren direkt access till den första noden i listan (`head`), men det är vanligare att ha ett objekt som wrappar listan. Det här objektet kan då ha referenser till både första och sista noden i listan (`tail`).

I länkade listor är det lätt att sätta in element mitt i listan, eftersom man då endast behöver ändra några pekare (vilket går på konstant tid). Tidskomplexiteten för access av ett element på ett givet index blir dock linjär, eftersom vi bara har direktaccess till första och sista noden i listan och således måste gå igenom listan tills vi kommer till det index vi söker.

För att förbättra accesstiden i en lista så kan man använda sig av en *självorganiserande lista*. Det här är en lista där element som accessas läggs först i listan. Ofta är 80 % av accesserna till 20 % av elementen, om om de här 20 % av elementen ligger långt fram i listan så går det snabbare att accessa dem.

En fördel med att använda en array istället för en länkad lista är att alla element ligger nära varandra i minnet i en array, till skillnad från en länkad lista där elementen (förmodligen) är utspridda på olika platser i minnet. När en array accessas kan processorn läsa in delar av arrayen (eller hela om den är tillräckligt liten) i sitt cacheminne, och behöver då inte accessa långsammare minne lika ofta.

## Träd

Träd är en datastruktur som består av *noder*, där varje nod kan ha flera barn. I ett *binärt träd* så kan varje nod ha max 2 barn. Den relation som binder samma en nod med sina barn kallas för *edge* (samma som i grafer).

### Terminologi

* **child** = en nod som ligger direkt under en annan nod.
* **parent** = en nod som ligger direkt ovanför en annan nod.
* **ancestor** = en nod som ligger indirekt ovanför en annan nod (t.ex. grand parent).
* **descendant** = en nod som ligger indirekt under en annan nod (t.ex. grand child).
* **sibling** = två noder som har samma förälder.
* **root** = noden som ligger längst upp i trädet (den enda noden som inte har någon förälder).
* **leaves** = de noder i trädet som inte har några barn (de som ligger längst ned i trädet)
* **forest** = en mängd av träd
* **ordnat träd** = syskon har en inbördes ordning. T.ex. i ett binärt sökträd är det skillnad på egenskaperna hos det högra och det vänstra barnet. Kan jämföras med en lista.
* **oordnat träd** = syskon har ingen inbördes ordning. Det spelar ingen roll i vilken ordning man betraktar dem. Kan jämföras med en mängd.
* **path** = en väg mellan två noder.
* **length of path** = längden av en väg mellan två noder = antal passerade bågar.
* **depth** = ett mått på hur långt en nod ligger från roten av trädet. Längden av vägen från noden till roten.
* **height** = höjden av en nod är längsta vägen ner till ett löv från noden. Höjden av ett löv är 0, och höjden av roten är trädets höjd.
* **size** = antalet noder i trädet.

### Representation

Träd kan även dem representeras på två olika sätt - med en array eller med objekt och pekare. Objekt och pekare är vanligast (och lättast att resonera kring), men när man har ett vänsterbalanserat träd (t.ex. en heap) så är det vettigare att använda en array. När man använder objekt och pekare är det vanligast att en nod har en pekare till varje barn, men man kan även göra så att noden har en pekare till sitt första barn, och att alla barn har pekare till "nästa" barn i ordningen. Det här är inte ett speciellt vanligt sätt att representera träd på.

När man representerar ett träd med en array (vilket är fallet i en heap), så är index till en nods vänstra barn `2i + 1` och högra barn `2i + 2`, där `i` är indexet till noden själv. Ett vänsterbalanserat träd är ett träd med optimal höjd (`log(n)`, alla noder har maximalt antal barn) där man hela tiden "fyller på från vänster". Anledningen till att sådana träd kan representeras med en array är att det aldrig blir några "glapp" i arrayen, eftersom man alltid fyller på på den första lediga platsen.

### Binära träd

Binära träd är ett specialfall av träd där varje nod får ha max 2 barn. Sådana här träd kan vara både *partiellt* och *totalt* ordnade.

I ett partiellt ordnat träd (även kallat **min heap/max heap**) är värdet på en nods barn mindre (eller större) än noden själv. Här spelar det alltså ingen roll om det högra eller vänstra barnet är större eller mindre än det andra. 

I ett totalt ordnat binärt träd (även kallat **binärt sökträd, BST**) så är varje nod i en nods vänstra delträd mindre än noden själv, och varje nod i en nods högra delträd större än noden själv. Alla vänstra descendants till noden ska alltså vara mindre än noden, och alla högra descendants ska vara större än noden.

För att ta bort en nod i ett binärt sökträd krävs lite jobb. Om noden har 0 barn så kan man ta bort den direkt, och om den har 1 barn så ersätter man noden med sitt barn. Om noden har två barn så letar man upp antingen det största värdet i det vänstra subträdet eller det minsta värdet i det högra subträdet, och ersätter noden med den noden. Man tar sedan bort den noden man hittade. Om man gör på det här sättet så reduceras problemet till att ta bort en nod med max 1 barn.

### Tidskomplexitet

Binära sökträd har bättre tidskomplexitet för access än länkade listor, eftersom man i binära sökträd i varje steg i sökningen kan halvera antalet noder som man behöver söka igenom. Man gör det genom att i varje steg kontrollera om elementet (eller indexet) man söker efter är mindre än eller större än noden man står på. Om den är mindre så går man till det vänstra delträdet, om det är större så går man till det högra delträdet. Om det är samma så har vi hittat vårt element och sökningen är slut.

Den genomsnittliga (och även best-case) tidskomplexiteten för sökning i ett binärt sökträd är `log(n)`. Worst-case är dock `O(n)`, vilket uppstår om man när man bygger upp trädet sätter in alla element i sorterad ordning. **I ett vanligt binärt sökträd spelar det alltså roll i vilken ordning man sätter in elementen!**

### Självbalanserande träd

För att komma runt det här så brukar man istället använda *självbalanserande träd*. Det här är alltså träd som automatiskt omvandlar sig själva så att de har optimal höjd (`log(n)`), så att sökning i trädet tar så kort tid som möjligt.

Det finns några olika typer av självbalanserande träd, men de vanligaste är:

* AVL
* Splay
* B (Ej gåtts igenom i kursen)
* Red black (Specialfall av B-träd som är binärt. Ej gåtts igenom i kursen)

#### AVL

AVL-träd är självbalanserade träd där skillnaden i höjd mellan en nods högra och vänstra delträd inte får vara högre än 1. När man sätter in en nod i ett AVL-träd så gör man på samma sätt som i ett vanligt binärt sökträd. När noden har lagts in på rätt plats så måste man dock se till att trädet fortfarande är balanserat. Det gör man genom att gå tillbaka samma väg upp i trädet och för varje nod räknar ut balansen. Om balansen är -2 eller 2 så måste den noden ombalanseras. Det görs via trädrotationer, och det finns fyra olika fall - vänster-vänster, höger-höger, vänster-höger och höger-vänster.

#### Splay

Splay-träd är ett träd där de mest accessade noderna ligger närmast roten (ungefär som en självorganiserande lista).

### Traversering

Man kan använda två olika tekniker för att traversera ett träd. Den första är *Depth First Search (DFS)*. Här följer man en väg hela vägen ner i trädet innan man börjar följa en annan väg. Det finns tre olika typer av DFS - pre-order, in-order och post-order. Det som skiljer dem är bara när i ordningen man tar föräldernoderna. I pre-order så tar man dem innan deras barn, i in-order så tar man dem mellan deras barn, och i post-order så tar man dem efter sina barn. DFS implementeras lättast rekursivt.

    void dfs(Node n) {
        if (n == null) {
            return;
        }
        // Pre-order
        dfs(n.left);
        // In-order
        dfs(n.right);
        // Post-order
    }

Den andra tekniken är *Breadth First Search (BFS)*. Här utforskar man en nivå i trädet åt gången innan man går ner till nästa nivå. BFS implementeras lättast iterativt med hjälp av en kö.

    void bfs(Node n) {
        Queue<Node> q = new LinkedList<>();
        q.add(n);
        while (!q.isEmpty()) {
            Node n = q.poll();
            q.add(n.left);
            q.add(n.right);
        }
    }

## Hashtabbeller

Hashtabeller är en sjukt bra datastruktur som ger genomsnittlig konstant tid för alla vanliga operationer (search, insert, delete). De används för att koppla ihop en nyckel med ett värde, där nyckeln och värdet kan vara av vilka typer som helst. Den enda nackdelen mad hashtabeller är att det inte går att traversera igenom innehållet i ordning.

En hashtabell är backad av en vanlig array med `n` platser. När man sätter ett nyckel-värde-par så omvandlar man först nyckeln till ett heltal (`hashCode()` i Java). Sedan kör man det här värdet genom en *hashfunktion* och får ut ett värde mellan 0 och `n`. Den simplaste hashfunktionen man kan ha är att helt enkelt ta heltalrepresentationen av nyckeln och köra `% n`. Ofta vill man dock använda sig av lite mer avancerade hashfunktioner som ger bättre spridning av nycklarna.

När man har fått ut ett index av hashfunktionen så vill man lagra värdet på den platsen i arrayen. Om den platsen redan är upptagen (flera nycklar kan hashas till samma värde) så blir det en *kollision*. Det finns två olika sätt att hantera kollisioner - *open addressing* och *closed addressing (chaining)*.

### Open addressing

När man använder open addressing och det uppstår en kollision så letar man upp en ny plats i arrayen att lagra sitt värde på. Det finns flera olika sätt man kan gå tillväga för att hitta en ny plats, men den enklaste är *linear probing*. Det man gör är att helt enkelt gå till nästa index i arrayen och kollar om den platsen är ledig. Om den är ledig sparar man värdet där, och om den inte är ledig går man till nästa. Det här sättet att hantera kollisioner på kan ge upphov till "kluster" av värden i tabellen, vilket gör att tabellen blir långsam.

För att komma runt det kan man istället använda *quadratic probing*. Det man gör då är att istället för att hela tiden bara gå ett steg framåt i arrayen så ökar man hela tiden antalet steg genom att kvadrera. Istället för att gå till `i + 1`, `i + 2`, `i + 3` osv. som i linear probing så går man istället till `i + 1^2 = i + 1`, `i + 2^2 = i + 4`, `i + 3^2 = i + 9` osv. Man får då bättre spridning på värdena i arrayen.

För att hitta ett värde i en hashtabell som använder open addressing så hashar man nyckeln precis som vid insättning, och sedan letar man framåt i arrayen tills man hittar sitt värde. Om man hittar en tom plats så vet man att värdet inte finns i tabellen. Det här gör det lite problematiskt att ta bort värden ur en hashtabell som använder open addressing, eftersom det kan resultera i att man inte hittar värden som man söker efter även fast de finns. Det man får göra då är att istället för att ta bort värdet bara markera det som borttaget. När man sätter in nya värden så kan man betrakta de här markerade värdena som borttagna, men när man sätter in värden så betraktar man dem som icke-borttagna.

### Closed addressing

När man använder closed addressing så letar man inte upp en ny plats i arrayen när det uppstår en kollision, utan man sparar alla kolliderande värden på samma plats i arrayen genom att lägga dem i t.ex. en länkad lista. Har man otur så hashas alla värden till samma index, och då blir hashtabellen inget mer än en glorifierad länkad lista.

### Rehashing

Om hashtabellen blir för full så kommer det uppstå många kollisioner, och det är därför bra att öka storleken på den bakomliggande arrayen. Det här gör dock att man måste hasha om alla tillagda nycklar, eftersom hashen för en nyckel räknas ut med hjälp av arrayens längd.

Det man gör är helt enkelt att allokera en ny, större array, loopar igenom alla gamla nycklar, räknar ut en ny hash och flyttar över nyckel-värde-paret till den nya platsen i den nya arrayen.

## Grafer

Grafer kan användas för att modellera många typer av problem. T.ex. busslinjer i Göteborg, datornätverk, todo-lists med dependencies etc. En graf består av noder och edges, precis som träd (träd är specialfall av grafer).

Det maximala antalet noder i en oriktad graf utan loopar är `n(n-1)/2`, och i en riktad graf med loopar är det `n^2`.

### Terminologi

* **riktad/oriktad** - en graf kan vara riktad eller oriktad. I en oriktad graf kan man gå över en kant i båda riktningarna, i en riktad graf har varje kant en specifierad riktning.
* **viktad/oviktad** - en graf kan vara viktad eller oviktad. I en viktad graf så har varje kant ett värde associerat med sig - en *vikt*. Det här kan ses som kostnaden att gå över den kanten. I en oviktad graf har kanterna inga vikter. Man kan även se det som att alla kanter har **samma vikt**.
* **adjacent** - två noder är adjacent om det finns en kant mellan dem. Man kan även säga att de är grannar.
* **path** - en väg mellan två noder. Kan ses som en serie av kanter som man ska gå över för att komma från en nod till en annan.
* **simple path** - en väg där ingen nod förekommer mer än en gång.
* **Hamiltoninan path** - en väg som besöker **alla noder exakt en gång**.
* **Euler path** - en väg som besöker **alla kanter exakt en gång**.
* **cycle** - en cykel är en väg som startar och slutar i samma nod.
* **simple cycle** - en cykel där ingen nod förekommer mer än en gång. 
* **acyklisk graf** - en graf är acyklisk om det inte finns några cykler i den. Träd är exempel på acykliska grafer.
* **sammanhängade graf** - en graf är sammanhängande om alla noder "sitter ihop". Grafen består alltså bara av en enda "ö".
* **starkt sammanhängade graf** - en riktad graf kan vara starkt sammahängande. Det betyder att det från alla noder finns en väg till alla andra noder. En riktad graf kan vara sammahängande utan att vara starkt sammanhängande.
* **multigraf** - en graf där det är tillåtet att ha flera kanter åt samma håll mellan två noder.
* **komplett graf** - en graf där det finns en kant mellan varje par av noder. En komplett graf har alltså det maximala antalet kanter.
* **fritt träd** - en oriktad, sammanhängande acyklisk graf. 
* **fri skog** - en samling fria träd. Alltså en oriktad acyklisk graf (men inte sammanhängande).
* **DAG** - en riktad graf utan cykler.
* **densitet** - ett mått på hur tät en graf är. Alltså hur många kanter den har i relation till antalet noder.
* **uppspännande träd** - ett samling kanter som binder samman alla noder i grafen. Får inte ha några loopar!
* **minimalt uppspännande träd** - ett uppspännande träd som har så låg totalvikt som möjligt.
* **degree** - ett mått på hur många grannar en nod har. Antalet utgående kanter från en nod.
* **loop** - en båge som går från en nod till sig själv.

### Representation

Det finns två vanliga sätt att representera grafer på - med en matris eller med en lista.

### Matris

Här representeras grafen som en `n x n`-matris, där elementet på raden `i`, kolumnen `j` talar om ifall det finns en kant från nod `i` till nod `j`. Det här sättet att representera grafer på är bäst för täta grafer. För glesa grafer kräver den onödigt mycket minne.

### Lista

Här representeras grafen med en lista med ett element för varje nod i grafen. I det här elementet sparas alla nodens grannar (eller utgående kanter) i en mängd eller en lista. Det här sättet att representera grafer på är bäst för glesa grafer.

### Traversering av grafer

Grafer traverseras på ungefär samma sätt som träd. Det enda man måste tänka på är att det kan finnas flera olika vägar till samma nod i en graf, så man måste hela tiden kolla så att man inte besöker samma nod två gånger. En graf behöver inte heller vara sammanhängande, så man kan inte räkna med att besöka alla noder om man bara utgår från en nod och rekursivt besöker alla dess grannar.

#### DFS

    List<Integer>[] graph;
        
    void dfs() {
        boolean[] visited = new boolean[numberOfNodes];
        for (int i = 0; i < numberOfNodes; i++) {
            if (!visited[i]) {
                dfs(i, visited);
            }
        }
    }
    
    void dfs(int node, boolean[] visited) {
        visited[node] = true;
        for (int neighbour : graph[node]) {
            if (!visited[neighbour]) {
                dfs(neighbour, visited);
            }
        }
    }

#### BFS

    List<Integer>[] graph;
    
    void bfs() {
        boolean[] visited = new boolean[numberOfNodes];
        for (int i = 0; i < numberOfNodes; i++) {
            if (!visited[i]) {
                bfs(i, visited);
            }
        }
    }
    
    void bfs(int startNode, boolean[] visited) {
        Queue<Integer> queue = new LinkedList<>();
        queue.add(startNode);
        visited[startNode] = true;
        while (!queue.isEmpty()) {
            int node = queue.poll();
            for (int neighbour : graph[node]) {
                if (!visited[neighbour]) {
                    visited[neighbour] = true;
                    queue.add(neighbour)
                }
            }
        }
    }

### Prims algoritm

Prims algoritm är en algoritm för att ta fram ett minimalt uppspännande träd i en graf. Det den här algoritmen gör är att börja med ett träd med en godtycklig nod. Trädet byggs sedan ut genom att välja den billigaste kanten som binder samman trädet med valfri nod som inte redan ingår i trädet.

    Set<Edge>[] graph;
    
    Set<Edge> prims() {
        boolean[] inTree = new boolean[numberOfNodes];
        Set<Edge> mst = new HashSet<>();
        
        for (int i = 0; i < numberOfNodes) {
            if (!inTree[i]) {
                mst.addAll(prims(i, inTree));
            }
        }
        
        return mst;
    }
    
    Set<Edge> prims(int node, boolean[] inTree) {
        Set<Edge> mst = new HashSet<>();
        
        inTree[node] = true;
        
        PriorityQueue<Edge> queue = new PriorityQueue<>(numberOfEdges, new EdgeComparator());
        queue.addAll(graph[node]);
        
        int nodesInTree = 1;
        while (nodesInTree < numberOfNodes && !queue.isEmpty()) {
            Edge edge = queue.poll();
            
            if (!inTree[edge.to]) {
                mst.add(edge);
                inTree[edge.to] = true;
                for (Edge e : graph[edge.to]) {
                    if (!inTree[e.to]) {
                        queue.add(e);
                    }
                }
                nodesInTree++;
            }
        }
        
        return mst;
    }
    
    class Edge {
        double weight;
        int from;
        int to;
    }
    
    class EdgeComparator implement Comparator<Edge> {
        @Override
        public int compare(Edge e1, Edge e2) {
            return Double.compare(e1.weight, e2.weight);
        }
    }

### Kruskals algoritm

Kruskals algoritm är också en algoritm för att hitta ett minimalt uppspännande träd i en graf. Det den här algoritmen gör är att hela tiden lägga till den billigaste kanten i grafen i trädet, så länge den kanten inte skapar en cykel.

    Set<Edge>[] graph;
    
    Set<Edge> kruskals() {
        Set<Edge>[] trees = (Set<Integer>) new Set[numberOfNodes];
        PriorityQueue<Edge> queue = new PriorityQueue(numberOfNodes, new EdgeComparator());
        
        for (int i = 0; i < numberOfNodes; i++) {
            trees[i] = new HashSet<>();
            queue.addAll(graph[i])
        }
        
        while (!queue.isEmpty() && trees[0].size() < numberOfNodes - 1) {
            Edge edge = queue.poll();
            int from = edge.from;
            int to = edge.to;
            if (trees[from] != trees[to]) {
                merge(trees, from, to);
                trees[from].add(edge);
            }
        }
        
        return trees[0];
    }
    
    void merge(Set<Edge>[] trees, int from, int to) {
        if (trees[from].size() < trees[to].size()) {
            for (E edge : trees[from]) {
                trees[to].add(edge);
                trees[edge.from] = trees[to];
                trees[edge.to] = trees[to];
            }
            trees[from] = trees[to];
        } else {
            for (E edge : trees[to]) {
                trees[from].add(edge);
                trees[edge.from] = trees[from];
                trees[edge.to] = trees[from];
            }
            trees[to] = trees[from];
        }
    }
    
    class Edge {
        public double weight;
        public int from;
        public int to;
    }
    
    class EdgeComparator implement Comparator<Edge> {
        @Override
        public int compare(Edge e1, Edge e2) {
            return Double.compare(e1.weight, e2.weight);
        }
    }

### Dijkstras algoritm

Dijkstras algoritm används för att hitta den kortaste vägen från en nod till en annan i en graf. Den kan även användas för att hitta det kortaste avståndet från en nod till alla andra noder.

    Set<Edge>[] graph;
    
    List<Edge> dijkstra1(int from, int to) {
        boolean[] visited = new boolean[numberOfNodes];
        
        PriorityQueue<DijkstraNode> queue = new PriorityQueue<>();
        queue.add(new DijkstraNode(from, 0, null));
        
        while (!queue.isEmpty()) {
            DijkstraNode node = queue.poll();
            
            if (!visited[node.id]) {
                if (node.id == to) {
                    return node.path;
                }
                
                visited[node.id] = true;
                
                for (Edge edge : graph[node.id]) {
                    if (!visited[edge.to]) {
                        List<Edge> path = new ArrayList(node.path);
                        path.add(edge);
                        queue.add(new DijkstraNode(edge.to, node.distance + edge.weight, path));
                    }
                }
            }
        }
    }
    
    class DijkstraNode implements Comparable<DijkstraNode> {
        int id;
        double distance;
        List<Edge> path;
        
        public DijkstraNode(int id, double distance, List<Edge> path) {
            this.id = id;
            this.distance = distance;
            this.path = path;
        }
        
        @Override
        public int compareTo(DijkstraNode n1) {
            return Double.compare(this.distance, n1.distance);
        }
    }
    
    class Edge {
        double weight;
        int from;
        int to;
    }
    
    class EdgeComparator implements Comparable<Edge> {
        @Override
        public int compare(Edge e1, Edge e2) {
            return Double.compare(e1.weight, e2.weight);
        }
    }

## Sortering

Att sortera en lista med indata är ofta ett sätt att göra ett problem lättare än ursprungsproblemet. De snabbaste generella sorteringslösningarna har tidskomplexiteten `O(n * log(n))`, medan vissa enklare lösningar har komplexiteten `O(n^2)`. Vissa algoritmer kan göras *in place*, vilket innebär att man inte behöver allokera någon ny array för den sorterade listan. En sorteringsalgoritm kan även vara *stabil*, vilket innebär att element som sorteras på samma plats hamnar i samma ordning som i ursprungslistan.

De sorteringsalgoritmer som vi har gått igenom i kursen är:

* Selection sort - `O(n^2)`
* Insertion sort - `O(n^2)`
* Bubble sort - `O(n^2)`
* Quick sort - `O(n * log(n))`
* Merge sort - `O(n * log(n))`
* Heap sort - `O(n * log(n))`

### Selection sort

I selection sort går man igenom hela listan för att hitta det minsta elementet, sätter det på nästa plats i den sorterade listan, och fortsätter på samma sätt tills alla element har sorterats. Selection sort kan göras in place.

    void selectionSort(int[] arr) {
        for (int i = 0; i < arr.length; i++) {
            int minIndex = i;
            for (int j = i + 1; j < arr.length; j++) {
                if (arr[j] < arr[minIndex]) {
                    minIndex = j;
                }
            }
            swap(arr, i, minIndex);
        }
    }
    
    void swap(int[] arr, int i1, int i2) {
        int tmp = arr[i1];
        arr[i1] = arr[i2];
        arr[i2] = tmp;
    }

### Insertion sort

Insertion sort kan ses som ungefär motsatsen till selection sort. Istället för att hela tiden hitta det minsta elementet för att sätta det på nästa plats, så tar du istället nästa element och sätter det på rätt plats i listan. Den här algoritmen har samma komplexitet som insertion sort, men den används mer eftersom den i bästa fall har komplexiteten `O(n)` (selection sort har `O(n^2)`). Även den här algoritmen kan utföras in place.

    void insertionSort(int[] arr) {
        for (int i = 1; i < arr.length; i++) {
            int val = arr[i];
            for (int j = i - 1; j >= 0 && val < arr[j]; j--) {
                swap(arr, j, j + 1);
            }
        }
    }
    
    void swap(int[] arr, int i1, int i2) {
        int tmp = arr[i1];
        arr[i1] = arr[i2];
        arr[i2] = tmp;
    }

### Bubble sort

Bubble sort heter som den gör för att element i listan "bubblar upp" till sin rätta position. Det man gör är att gå igenom listan och byta plats på två närliggande element om de ligger i fel ordning. När man har gått igenom hela listan så börjar man om på nytt. Om man lyckas göra en hel genomlöpning utan att ha gjort en enda swap så är listan sorterad. Den här sorteringalgoritmen kan även den göras in place, och den kan vara väldigt effektiv om listan som ska sorteras redan är *nästan* sorterad, eftersom det då inte behövs så många genomlöpningar.

    void bubbleSort(int[] arr) {
        boolean swapped;
        do {
            swapped = false;
            for (int i = 0; i < arr.length - 1; i++) {
                if (arr[i] > arr[i + 1]) {
                    swap(arr, i, i + 1);
                    swapped = true;
                }
            }
        } while (swapped);
    }
    
    void swap(int[] arr, int i1, int i2) {
        int tmp = arr[i1];
        arr[i1] = arr[i2];
        arr[i2] = tmp;
    }

### Quick sort

Quick sort är en rekursiv sorteringsalgoritm som kan göras in place. Den här sorteringsalgoritmen är väldigt effektiv, och är oftast den sorteringsalgoritmen man vill använda. Den fungerar så att man i varje rekursivt anrop väljer ett *pivot-element*. Det här elementet sätts i mitten av listan, alla mindre element läggs till vänster om det och alla större element läggs till höger. Man vet då att pivot-elementet ligger på rätt plats i listan. Sedan utförs samma operationer rekursivt på både den högra och den vänstra sublistan.

För att den här algoritmen ska vara effektiv krävs det att pivot-elementet väljs på ett bra sätt. Om man i varje rekursivt anrop väljer det största eller minsta elementet så kommer komplexiteten för algoritmen bli `O(n^2)`. Om man har en redan sorterad lista och alltid väljer att pivotelementet ska vara det sista elementet i listan så kommer det här hända! Det man kan göra för att åtgärda det är att antingen explicit blanda listan innan den sorteras, eller välja pivot-elementet på ett bättre sätt.

    void quickSort(int[] arr) {
        quickSortRec(arr, 0, arr.length);
    }
    
    void quickSortRec(int[] arr, int start, int end) {
        if (end - 1 <= start) {
            return;
        }
        int pivotIndex = partition(arr, start, end);
        quickSortRec(arr, start, pivotIndex);
        quickSortRec(arr, pivotIndex + 1, end);
    }
    
    void partition(int[] arr, int start, int end) {
        int pivotIndex = end - 1;
        int pivot = arr[pivotIndex];
        
        int smallerIndex = start;
        int largerIndex = pivotIndex - 1;
        
        while (smallerIndex < largerIndex) {
            if (arr[smallerIndex] > pivot) {
                swap(arr, smallerIndex, largerIndex);
                largerIndex--;
            } else {
                smallerIndex++;
            }
        }
        
        if (arr[smallerIndex] > pivot) {
            swap(arr, smallerIndex, pivotIndex);
            return smallerIndex;
        } else {
            swap(arr, smallerIndex + 1, pivotIndex);
            return smallerIndex + 1;
        }
    }
    
    void swap(int[] arr, int i1, int i2) {
        int tmp = arr[i1];
        arr[i1] = arr[i2];
        arr[i2] = tmp;
    }

### Merge sort

Merge sort är precis som quick sort en rekursiv sorteringsalgoritm. En skillnad är dock att den inte kan göras in place, utan behöver `O(n)` extra plats. Det algoritmen går ut på är att kontinuerligt dela upp listan i två mindre delar, sortera de mindre delarna, och sedan *merga* ihop dem till en stor sorterad lista. När en uppdelad array har längden 1 så vet man att den är sorterad.

    void mergeSort(int[] arr) {
        if (arr.length <= 1) {
            return;
        }
        
        int[] left = new int[arr.length / 2];
        int[] right = new int[arr.length - left.length];
        
        arrayCopy(arr, left, 0);
        arrayCopy(arr, right, left.length);
        
        left = mergeSort(left);
        right = mergeSort(right);
            
        merge(arr, left, right);
        return arr;
    }
    
    void arrayCopy(int[] from, int[] to, int startIndex) {
        for (int i = 0; i < to.length; i++) {
            to[i] = from[startIndex + i];
        }
    }
    
    void merge(int[] to, int[] a, int[] b) {
        int aIndex = 0;
        int bIndex = 0;
        int toIndex = 0;
        
        while (aIndex < a.length && bIndex < b.length) {
            if (a[aIndex] < b[bIndex]) {
                to[toIndex] = a[aIndex];
                aIndex++;
            } else {
                to[toIndex] = b[bIndex];
                bIndex++;
            }
            toIndex++;
        }
        
        while (aIndex < a.length) {
            to[toIndex] = a[aIndex];
            aIndex++;
        }
        
        while (bIndex < b.length) {
            to[toIndex] = b[bIndex];
            bIndex++;
        }
    }

### Heap sort

Heap sort är en sorteringsalgoritm som utnyttjar datastrukturen heap, som har garanterad `O(log(n))` tid för extrahering av det minsta elementet. Det man gör är helt enkelt att skapa en heap av den ursprungliga arrayen (*heapify*), vilket går att göra på linjär tid. Sedan tar man kontinuerligt ut det minsta elementet tills heapen är tom. Man kommer då har gjort `n` extraheringar av det minsta elementet, vilket alltså har komplexiteten `n * log(n)`.

    void heapSort(int[] arr) {
        Heap heap = new Heap(arr);
        for (int i = 0; i < arr.length; i++) {
            arr[i] = heap.extractMin();
        }
    }
    
    class Heap {
        private int[] arr;
        private int length;
        
        public Heap(int[] arr) {
            this.arr = new int[arr.length];
            for (int i = 0; i < arr.length; i++) {
                this.arr[i] = arr[i];
            }
            this.length = arr.length;
            
            // Begin at index length / 2 - 1 and work towards the root
            for (int i = arr.length / 2 - 1; i >= 0; i--) {
                heapify(i);
            }
        }
        
        void heapify(int i) {
            int l = 2 * i + 1;
            int r = 2 * i + 2;
            
            // Does a right child exist?
            if (r < this.length) {
                // Yes, then there is a left child as well
                if (this.arr[l] < this.arr[i] || this.arr[r] < this.arr[i]) {
                    // Swap with the smallest child
                    if (this.arr[l] < this.arr[r]) {
                        swap(l, i);
                        heapify(l);
                    } else {
                        swap(r, i);
                        heapify(r);
                    }
                }
            } else if (l < this.length) {
                // No, just check left child
                if (l < this.arr[i]) {
                    swap(l, i);
                    heapify(l);
                }
            }
        }
        
        int extractMin() {
            int min = this.arr[0];
            swap(0, this.length - 1);
            this.length--;
            heapify(0);
            return min;
        }
        
        void swap(int i1, int i2) {
            int tmp = this.arr[i1];
            this.arr[i1] = this.arr[i2];
            this.arr[i2] = tmp;
        }
    }
