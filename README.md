# 🏰 Animated Procedural Dungeon Generator (UE5)

**Engine:** Unreal Engine 5.3+
**Language:** Blueprints (Visual Scripting)
**Assets:** Quixel Bridge Megascans
**Focus:** Procedural Generation, Runtime Visualization, Async Loops.

## 📝 Descrizione Breve
Un generatore di livelli procedurali basato su griglia. L'algoritmo non si limita a creare la mappa istantaneamente, ma visualizza il processo di costruzione in tempo reale ("Runtime Visualization") utilizzando un sistema di iterazione basato su Timer, simulando un effetto "costruzione animata".



## 🌟 Key Features

* **Generazione Procedurale a Griglia:** Creazione dinamica di mappe di dimensioni variabili (X, Y definibili dall'utente).
* **Animated Build Process:** Sostituzione dei loop sincroni standard con un sistema asincrono (`Set Timer by Event`) per mostrare la generazione blocco per blocco senza bloccare il Game Thread.
* **Variazione Organica:** Applicazione di rotazioni casuali (Yaw 0-360°) e scaling adattivo su ogni istanza per evitare l'effetto "ripetizione a griglia".

### 🌍 Biome Logic & Assets
Il sistema gestisce tre tipi di blocchi distinti, utilizzando asset ad alta fedeltà (**Quixel Bridge**):

1.  ⛰️ **Montagne (Strutturali & Ostacoli):**
    * **Mesh:** Actor composito personalizzato formato da una base cubica e una sommità conica (Cube + Cone) per stilizzare l'ambiente.
    * **Texture:** Materiali fotorealistici da Quixel Bridge.
    * **Spawn Logic:** Vengono generate obbligatoriamente su tutto il perimetro (Bordi) per chiudere la mappa, ma hanno anche una probabilità percentuale di spawnare all'interno come ostacoli.
2.  🏖️ **Sabbia (Variazione):**
    * **Spawn Logic:** Spawnano randomicamente all'interno della griglia per spezzare la monotonia del terreno.
    * * **Texture:** Materiali fotorealistici da Quixel Bridge.
3.  🌿 **Erba (Base):**
    * **Spawn Logic:** Il terreno di default che riempie tutte le celle dove non sono presenti montagne o sabbia.
    * * **Texture:** Materiali fotorealistici da Quixel Bridge.

## 🔧 Dettagli Tecnici (Under the Hood)

### 1. Algoritmo di Iterazione (The Engine)
Invece di un classico *Nested For Loop* (che genererebbe il livello in un singolo frame invisibile), ho implementato un **Iteratore Lineare Temporizzato**:
* Utilizzo di due variabili contatore (`CurrentX`, `CurrentY`).
* Un evento personalizzato richiamato da un Timer (0.05s delay) che posiziona un singolo attore e incrementa le coordinate.
* Condizioni di reset (Row Completion) e invalidazione del timer (Map Completion).

### 2. Calcolo delle Coordinate (Math)
Conversione da "Indice Griglia" a "Coordinate Mondo":
WorldLocation.X = CurrentX * TileSize
WorldLocation.Y = CurrentY * TileSize
Questo garantisce che il sistema sia scalabile indipendentemente dalla dimensione delle mesh utilizzate.

3. Logica Decisionale (Branching Logic)
Ogni cella passa attraverso una serie di controlli condizionali (Logic Gates):

Border Check: IF (X==0 OR Y==0 OR X==Width-1 OR Y==Height-1) -> Spawn Montagna (Bordo).

Internal Random Check:

IF (RandomFloat < 0.1) -> Spawn Montagna (Ostacolo Interno).

ELSE IF (RandomFloat < 0.2) -> Spawn Sabbia.

ELSE -> Spawn Erba.

🛠 Tech Stack
Engine: Unreal Engine 5.3+

Scripting: Blueprints Visual Scripting

Assets: Quixel Megascans, Custom Composite Meshes.

Concetti Chiave: Arrays, Flow Control, Vector Math, Timers & Delegates, Procedural Generation.

Developed by Osvaldo Francesca 
