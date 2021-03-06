# iOS Interview Questions



## BASE

### D1. Quali sono i principali stati di un'applicazione iOS?
* Non-running — L'applicazione non è in esecuzione. 
* Inactive — L'applicazione è in esecuzione in foreground ma non sta ricevendo eventi. Un'app iOS può essere messa nello stato "inactive" dal sistema operativo quando ad esempio si riceve una telefonata o un SMS.  
* Active — L'applicazione è in esecuzione e sta ricevendo eventi.  
* Background — L'applicazione è in esecuzione in background e sta eseguendo del codice.  
* Suspended — L'applicazione è in esecuzione in background e non sta eseguendo alcun codice.  

### D2. Quali sono le differenze tra frame e bounds?
Il bound di una UIView rappresenta il rettangolo espresso con una posizione (x,y) e le sue dimensioni (width, height) relative al suo sistema di coordinate (0, 0).  
Il frame di una UIView è allo stesso modo un rettangolo con una posizione e dimensione ma riferito rispetto alle coordinate della superview che lo contiene.

### D3. Spiegare il pattern MVC
MVC è il pattern consigliato da Apple per lo sviluppo su iOS ed è la base dei framework Cocoa e Cocoa Touch.
MVC sta per Model View Controller, definisce una separazione logica dei componenti e come essi comunicano.  
* Models — responsabile per il dominio riguardante l’accesso ai dati dell’applicazione
* Views — responsabile per il livello di presentazione e interazione (GUI).
* Controller — è il mediatore tra il modello e la view, solitamente riceve i comandi dall’utente e aggiorna il modello. Viceversa un evento può scatenare un aggiornamento del modello, tipicamente con il KVO o tramite un sistema di notifiche viene avvertito il Controller che aggiornerà a sua volta la view di conseguenza.
 
### D4. Qual'è la differenza tra: fileprivate, private e public private(set) in Swift?
* fileprivate è accessibile all'interno del file corrente  
* private è riferito all'interno della corrente dichiarazione  
* public private(set) significa che il getter è pubblico mentre il setter è privato.

### D5. Cos'è il "Forced Unwrapping" in Swift?
Quando si dichiara una variabile opzionale, per ottenere il suo valore è necessario l'unwrap. Ciò è possibile tramite l'optional binding o il force unwrap.  
Il force unwrap fa si che si forzi l'unwrap in modo che il valore non sia più opzionale. Il force unwrap è potenzialmente pericoloso perchè nel caso la variabile sia nil il force unwrap lancerà un'eccezione che manderà in crash l'applicazione.

### D6. Perchè si utilizzano le struct al posto delle classi?
Le classi supportano l'ereditarietà, cosa non supportata dalle struct.
Le struct sono di tipo "value type" ovvero una sua copia o il passaggio di parametro avviene per valore mentre nel caso delle classi la copia avviene per "reference" ovvero viene copiato il puntatore.
Dove usare uno o l'altro dipende dal contesto. Si preferiscono le classi quando copiare o comparare le istanze non ha senso nel contesto corrente.  
La functional programming predilige i value types quindi sono da preferire le struct mentre la OOP predilige le classi.

### D7. Qual'è la differenza tra viewDidLoad e viewDidAppear? Nel quale è preferibile inserire una chiamata ad un server remoto in una classica architettura che utilizza il pattern MVC?
Nel contesto degli UIViewController il ViewDidLoad è il metodo eseguita dal sistema dopo che la view viene caricata in memoria e soprattutto una sola volta. ViewDidAppear viene chiamata ogni volta che la view è appare visibile all'utente.
Inserendo una chiamata di rete nel viewDidAppear scatenerebbe una richiesta ad ogni visualizzazione della view del controller in questione. E’ quindi preferibile, a meno di casi particolari, inserire la chiamata nel viewDidLoad.

### D8. Cosa non è possibile inserire in un array o in un dizionario (Obj-C)?
Nil non è un oggetto che è possibile inserire in un array o un dizionario in Objective-C. Questo è il motivo per il quale addObject(nil) manda in crash l'applicazione.

### D9. Per eseguire una ricerca tra un insieme di elementi risulta più veloce un NSArray o un NSSet?
Quando l'ordine degli elementi in una collection non è importante NSSet è molto più performante in quanto i Set utilizzano dei valori di hash per trovare gli elementi (similarmente a un dizionario) mentre un array deve essere iterato finchè non si trova l’elemento desiderato (complessità O(1) vs O(n)).

### D10. Per fare una richiesta http di rete è preferibile usare URLConnection o URLSession?
Da iOS 7.0 URLSession è preferito, prevede la funzionalità del download in background (mentre l’app non è in esecuzione) e il raggruppamento di varie richieste rendendo più semplice la loro cancellazione relativa ad un determinato lavoro. URLSession prevede una migliore sintassi che utilizza i “blocks”.

### D11. Perchè dovresti preferire una chiamata http asincrona rispetto alla controparte sincrona? In Swift qual è la sintassi per eseguire un task in modo asincrono?
Una chiamata asincrona non blocca il thread di UI detto il main thread.  
In swift si utilizza il seguente codice:
```swift
Dispatch.main.async {
	// codice da eseguire
}
```
### E1. Cosa stampa il seguente codice?
```swift
class Person {
    var name: String
    var age: Int
}

let myFriend = Person(name: "Peter", age: 24)
print(myFriend.name)
```

- [ ] ""
- [ ] "Peter"
- [ ] "myFriend"
- [ ] nil
- [ ] Non viene stampato nulla
- [ ] Questo codice compila ma va in crash
- [ ] Questo codice non compila
*********************************
##### Riposta corretta: Questo codice non compila
##### Spiegazione  
Le Struct supportano la "memberwise initialization" mentre le classi non ancora nelle attuali versioni di Swift. Questo codice non compila perchè la classe non ha un inizializzatore 

### E2. Scrivere una funzione che ritorni true quando una stringa data in input è palindroma, false altrimenti
*********************************
##### Riposte di esempio:
```swift
func isPalindrome(_ input: String) -> Bool {
  let reversed = String(input.reversed())
  if input == reversed {
    return true
  }
  return false
}
```
oppure (soluzione case sensitive)
```swift
func isPalindrome2(_ input: String) -> Bool {
  return String(input.reversed()).lowercased() == input.lowercased()
}
```
oppure con l'uso di un'extension
```swift
extension String {
    func isPalindrome() -> Bool {
        return self == String(self.reversed())
    }
}
```


### E3. Cosa produce il seguente codice?
 ```swift
var i = 2

do {
    print(i)
    i *= 2
} while (i < 128)
```

- [ ] 2, 4, 8, 16, 32, 64
- [ ] 2, 4, 8, 16, 32, 64, 128
- [ ] Questo codice compila ma va in crash
- [ ] Questo codice non compila
*********************************
##### Risposta corretta: Questo codice non compila.
##### Spiegazione  
La keyword "do" non è corretta qui, occorre usare ia keyword "repeat".

### E4. Cosa conterrà la costante third alla fine dell'esecuzione del codice?
 ```swift
let first = ["Sulaco", "Nostromo"]
let second = ["X-Wing", "TIE Fighter"]
let third = first + second
```

- [ ] "Sulaco", "Nostromo"
- [ ] "Sulaco", "Nostromo", "Sulaco", "Nostromo"
- [ ] "Sulaco", "Nostromo", "X-Wing", "TIE Fighter"
- [ ] "X-Wing", "TIE Fighter", "Sulaco", "Nostromo"
- [ ] Il risultato è un array vuoto
- [ ] Questo codice compila ma va in crash
- [ ] Questo codice non compila
*********************************
##### Riposta corretta: "Sulaco", "Nostromo", "X-Wing", "TIE Fighter".
##### Spiegazione 
Gli array in Swift possono essere uniti usando l'operatore +, la sua applicazione comporta il concatenamento del secondo array alla fine del primo

### E5. Cosa stampa il seguente codice?
```swift
let bigNumber = Int.max
let biggerNumber = bigNumber + 1
print(biggerNumber)
```
- [ ] -9223372036854775808
- [ ] 0
- [ ] 9223372036854775807
- [ ] Non viene stampato nulla
- [ ] Questo codice compila ma va in crash
- [ ] Questo codice non compila
*********************************
##### Riposta corretta: Questo codice compila ma va in crash.
##### Spiegazione. 
L'aggiunta di 1 al massimo numero intero rappresentabile solleva un overflow.

### E6. Quanti elementi conterrà l'array names alla fine dell'esecuzione del codice?
```swift
let names = [String]()
names.append("Amy")
names.append("Clara")
names.append("Rory")
```

- [ ] 0
- [ ] 1
- [ ] 2
- [ ] 3
- [ ] Questo codice compila ma va in crash
- [ ] Questo codice non compila
*********************************
##### Riposta corretta: Questo codice non compila
##### Spiegazione  
L'array names è dichiarato come let ovvero come costante. Per renderlo modificabile occorre usare var try to use append() to add strings to it.

### E7. Cosa stampa il seguente codice?
```swift
let names = ["Amy", "Rory"]

for name in names {
    name = name.uppercased()
    print("HELLO, \(name)!")
}
```

- [ ] "HELLO, AMY!"
- [ ] "HELLO, AMY!", "HELLO, RORY!"
- [ ] "HELLO, RORY!"
- [ ] Non viene stampato nulla
- [ ] Questo codice compila ma va in crash
- [ ] Questo codice non compila
*********************************
##### Riposta corretta: Questo codice non compila.
##### Spiegazione  
name è definito implicitamente come costante (let) non può essere modificato. Per renderlo modificabile bisognerebbe forzarlo come var

### E8. Cosa stampa il seguente codice?
 ```swift
 let i = 3

switch i {
case 1:
    print("Number was 1")
case 2:
    print("Number was 2")
case 3:
    print("Number was 3")
}
```

- [ ] Prints "Number was 2"
- [ ] Prints "Number was 3"
- [ ] Questo codice compila ma va in crash
- [ ] Questo codice non compila
*********************************
##### Riposta corretta: Questo codice non compila
##### Spiegazione 
Swift esige che tutti i casi di uno switch siano dichiarati, alternativamente è possibile utilizzare il case default.
 

## INTERMEDIATE

### D1. Cosa accade se si invoca un metodo su un puntatore che è a nil?
In Objective-C è considerata una no-op, è una operazione che non ha alcun effetto, l'app non va in crash.

### D2. Differenze tra setNeedsLayout, layoutIfNeeded e layoutSubviews?
Il metodo setNeedsLayout chiede al sistema di ridisegnare una vista e le sue sottoviste associate, il metodo ritorna immediatamente, è asincrono e dunque non è indeterminato il momento del completametno dell’operazione.
LayoutIfNeeded è un metodo sincrono, chiede al sistema l’immediato ridisegno di una vista e delle eventuali sottoviste.
Le sottoclassi di UIView possono fare l’override di LayoutSubviews per definire più precisamente il layout delle viste devono apparire all’interno della vista in oggetto.

### D3. Cos'è Git? Perchè lo si utilizza? Cosa sai di Gitflow?
Git è un sistema di controllo di versione distribuito (Distributed Version Control Systems o DVCS) sviluppato da Linus Torvalds che sta diventando rapidamente uno dei più diffusi in circolazione.  
Git, considera i propri dati come una serie di istantanee di un mini filesystem. Ogni volta che l’utente effettua un commit o salva lo stato del proprio progetto, crea un’immagine di tutti i file presenti in quel momento, salvando un riferimento. Se alcuni file non sono stati modificati, GIT non li clona ma crea un collegamento agli stessi file della versione precedente.  
Altro aspetto molto interessante è la possibilità di lavorare anche off-line con il server centrale. L’utente può lavorare sulla propria copia locale del repository e rendere pubbliche le modifiche quando il server torna online.

Git flow è un flusso di sviluppo, che descrive un modello di diramazione, (branching), ben preciso costruito intorno al concetto di release software. Esso permette il versioning semantico e la potenzialità di poter lavorare all'interno di un team su feature separate nello stesso momento che andranno poi "mergiate" nel branch di sviluppo o quello principale.

### D4. Cos'è Autolayout e cosa sono i Constraint?
Auolayout dinamicamente calcola la grandezza e la posizione di tutte le viste in una particolare gerarchia di view.
I constraints sono i vincoli che si aggiungono alle viste per far in modo che autolayout possa calcolare tutto con precisione.

### D5. Qual'è lo scopo del reuseIdentifier?
Il reuseIdentifier è importante nell'utilizzo di UITableView e UICollectionView. Il reuseIdentifier dichiara quali sono le celle che possono essere riciclate senza dover doverle ricreare (risparmiando tempo e memoria). Internamente al sistema vengono raggruppate le celle che differiscono solo del contenuto ma non del layout. L'utilizzo del reuseIdentifier è essenziale per avere una buona velocità nello scroll. E' compito del programmatore gestire il riciclo delle celle in modo che mentre l'utente scrolla il contenuto deve essere corretto e non anch'esso riciclato.

### D6. Cosa sono le closure in Swift?
Una closure è un blocco di funzionalità che puoi distribuire nel tuo codice. Puoi passare una closure come argomento di una funzione o puoi memorizzarla come proprietà di un oggetto. Le closure hanno molti casi d'uso.

Il nome closure suggerisce una delle loro caratteristiche chiave. Una closure prende le variabili e le costanti dal contesto nel quale è definita. Questo è definito talvolta come closing over.


### D7. Spiegare [weak self] e [unowned self]
Unowned (referenza non strong) è equivalente a weak ad una eccezione: la variabile non diventerà nil e non deve essere opzionale.
Bisognaa stare attenti all'utilizzo di [unowned self] perchè se l'oggetto è stato deallocato prima della chiamata della closure avverrà inevitabilmente un crash.
Dichiarando [weak self] occorre gestire il caso in cui self possa essere nil (ad esempio con un if o un gard).  
La dimenticanza di questo "sintactic sugar" può causare leak all'interno di closures se self ha una referenza strong alla closure e al suo interno è presente un'altra referenza strong verso self (concetto di referenza circolare).


### D8. Cos'è Core Data?
Core Data è il framework di Apple usato per gestire il layer di modelli. Ha la capacità di persistere un grafo di oggetti in un persistant store. I dati sono organizzati in un modello relazoinale e di entità-attributi.

### E1. Che output viene prodotto dal codice seguente?
```swift
let names = ["Pilot": "Wash", "Doctor": "Simon"]
let doctor = names["doctor"] ?? "Bones"
print(doctor)
```

- [ ] ""
- [ ] "Bones"
- [ ] "Doctor"
- [ ] nil
- [ ] "Wash"
- [ ] Non viene stampato nulla
- [ ] Questo codice compila ma va in crash
- [ ] Questo codice non compila
*********************************
##### Riposta corretta: "Bones"
##### Spiegazione  
Viene fatto accesso all'elemento "doctor" nel dizionario, chiave mai inserita: i dizionari sono case sentitive. Ciò comporta come risultato nil che scatena l'esecuzione del "nil coalescing operator" che valorizzerà la variabile doctor a "Bones".

### E2. Che cosa stampa il codice seguente?
 ```swift
let userLoggedIn: Bool? = false

if !userLoggedIn! {
    print("Message one")
} else {
    print("Message two")
}
```

- [ ] "Message one"
- [ ] "Message two"
- [ ] Non viene stampato nulla
- [ ] Questo codice compila ma va in crash
- [ ] Questo codice non compila
*********************************
##### Riposta corretta: "Message one"
##### Spiegazione  
L'espressione !userLoggedIn! significa: force unwrap del bool e negalo. Il booleano è impostato a false quindi la sua negazione viene valutata in true e il risultato è la stampa del primo messaggio.

### E3. Che differenza c'è tra nil e .None? In che cosa differiscono le seguenti variabili?
 ```swift
var optional1: String? = nil
var optional2: String? = .None
 ```
*********************************
##### Riposta corretta: Nessuna differenza semantica
##### Spiegazione  
Optional(.None), in breve .None è il corretto modo per esprimere un opzionale che ha come valore nil.
nil viene usato come "syntactic sugar" per .None
 
### E4
##### 1. Quale delle due pizzerie istanziate produrrà la margherita con il basilico?
 ```swift
struct Pizza {
  let ingredients: [String]
}

protocol Pizzeria {
  func makePizza(ingredients: [String]) -> Pizza
  func makeMargherita() -> Pizza
}

extension Pizzeria {
  func makeMargherita() -> Pizza {
    return makePizza(["tomato", "mozzarella"])
  }
}

struct Lombardis: Pizzeria {
  func makePizza(ingredients: [String]) -> Pizza {
    return Pizza(ingredients: ingredients)
  }
  func makeMargherita() -> Pizza {
    return makePizza(["tomato", "basil", "mozzarella"])
  }
}

let lombardis1: Pizzeria = Lombardis()
let lombardis2: Lombardis = Lombardis()

lombardis1.makeMargherita()
lombardis2.makeMargherita()
 ```
*********************************
##### Riposta corretta: Tutte e due.
##### Spiegazione  
Pizzeria è un protocollo che richiede il metodo makeMargherita, nel codice viene fornita una implementazione di default tramite extension ma l'implementazione concreta (Lombardis) del protocollo è sovrascritta e quindi a runtime viene invocata la corretta implementazione.

##### 2. Cosa succede se il protocollo non richiedesse il metodo makeMargherita?
```swift
protocol Pizzeria {
  func makePizza(ingredients: [String]) -> Pizza
}

extension Pizzeria {
  func makeMargherita() -> Pizza {
    return makePizza(["tomato", "mozzarella"])
  }
}
```
*********************************
##### Riposta corretta: Solo lombardis2 farà la margherita con il basilico
##### Spiegazione  
Lombardis1 a runtime invocherà il metodo makeMargherita definito nell'extension che non prevede il basilico.


### E5. Che cosa stampa il codice seguente?
```swift
 let point = (556, 0)
switch point {
case (let x, 0):
    print("X was \(x)")
case (0, let y):
    print("Y was \(y)")
case let (x, y):
    print("X was \(x) and Y was \(y)")
}
```

- [ ] X was 556
- [ ] X was 556 and Y was 0
- [ ] Y was 0
- [ ] Non viene stampato nulla
- [ ] Questo codice compila ma va in crash
- [ ] Questo codice non compila
*********************************
##### Riposta corretta: X was 556
##### Spiegazione  
Swift esegue il primo blocco che viene "matchato" in uno switch. In questo caso point è una tupla con il valore (556, 0) quindi (let x, 0) risulta soddisfatto e il messaggio relativo stampato.



## AVANZATO

### D1. Quali sono le differenze tra il Manual e l'Automatic Rerenrence Counting?
Per gestire manualmente la memoria è necessario conoscere il concetto di Reference Counter e alla proprietà che ogni oggetto possiede: retainCount, questo è un intero che indica il numero di oggetti associati all'oggetto stesso.  
Se il retainCount di un oggetto arriva a 0 viene deallocato da l'AutoreleasePool.  
Quando un oggetto viene istanziato con alloc e init il retainCount vale 1, è possibile esplicitare retain e release per far aumentare e diminuire rispettivamente il retainCount.
Un retainCount errato può portare a leak o crash dell'applicazione.
Con ARC (gestione automatica della memoria) introdotto da Xcode 4.2 lo sviluppatore non deve più preoccuparsi di invocare esplicitamente retain e release per tenere coerente il valore di retainCount.  
ARC non è un garbage collector in quanto lavora a compile time e non a run-time.

### D2. Quali sono le differenze tra Sequence Protocol e Collection Protocol?
Una sequenza rappresenta un insieme di valori che possono essere iterati tramite un for-loop.
Collection è una Sequence dove ci si può accedere agli elementi tramite subscript, definisce inoltre l'indice di partenza (startIndex) e di fine (endIndex).  
Si può accedere più volte ad ogni singolo elemento di una Collection mentre nel caso della Sequence non è più possibile a meno di una nuova iterazione.
Come ultimo punto Collection eredita da Sequence. 

### D3. Cos'è MVVM?
MVVM sta per Model View ViewModeo, è un pattern architetturale per l'implementazione di interfacce utente
E' una versione ampliata di MVC dove la presentation logic è estrapolata dal controller e messa nel ViewModel.  

Il Model rappresenta il punto di accesso ai dati. Trattasi di una o più classi che leggono dati dal DB, oppure da un servizio Web di qualsivoglia natura.
La View rappresenta la vista dell’applicazione, l’interfaccia grafica che mostrerà i dati.
Il ViewModel è il punto di incontro tra la View e il Model: i dati ricevuti da quest’ultimo sono elaborati per essere presentati e passati alla View. Qui risiede la logica di business dell'app.
In iOS negli ultimi anni esiste un popolare framework chiamato RxSWift che aggiunge le reactive extension al mondo iOS mentre un altro framework RxCocoa aggiunge tali estensioni al mondo della UI di iOS.

### D4. Nel contesto MVVM un UIViewController a che parte appartiene? Model, View o ViewModel?
Nel caso di iOS è inevitabile la componente del Controller anche in un'architettura che prevede MVVM. Il controller farà parte comunque del mondo "view". Il controller conoscerà il ViewModel e la View e non viceversa.

### D5. Hai esperienza con il TDD/BDD in iOS? Se si, che framework utilizzi?
Il framework ufficiale per lo unit test è XCTest, importanti alternative specialmente per il BDD sono Kiwi, Quick (e Nimble  che è una mather library per le expectations).

### D6. A cosa serve la keyword defer in Swift?
Si utilizza defer quando si vuole che un determinato blocco di codice venga eseguito mentre si sta lasciando lo scope corrente (es. dopo una return in una funzione).

### D7. Nel mondo RxSwift-RxCocoa in cosa differisce la subscription utilizzando un observable rispetto ad un driver?
Con driver:
* non può mai essere emesso un errore
* il delivery avviene nel MainScheduler.instance
* viene applicato lo sharing strategy share(replay: 1, scope: .whileConnected)

### D8. Nel paradigma Rx se un observable per qualche motivo emette un Error cosa succede?
L'intero sottoscriizione viene terminata con errore e non è più possibile ricevere nuovi elementi a meno di una nuova sottoscrizione.

### E1. Quando questo codice viene eseguito, cosa conterrà la costante numbers?
```swift
let numbers = [1, 2, 3].flatMap { [$0, $0] }
```

- [ ] [1, 1, 2, 2, 3, 3]
- [ ] [1, 2, 3]
- [ ] [[1, 1], [1, 2], [1, 3]]
- [ ] [[1, 1], [2, 1], [3, 1]]
- [ ] [[1, 1], [2, 2], [3, 3]]
- [ ] [[1, 2, 3], [1, 2, 3], [1, 2, 3]]
- [ ] Questo codice compila ma va in crash
- [ ] Questo codice non compila
*********************************
##### Riposta corretta: [1, 1, 2, 2, 3, 3]
##### Spiegazione  
l'invocazione di map comporta in un loop di tutti gli elementi nell'array e risulta in una creazione di un nuovo array che contiene ogni numero duplicato. Ad esempio il primo numero (1) viene convertito in [1, 1] e così via. In questo caso viene utilizzato flatMap che appiattisce il tutto in un singolo array ovvero [[1, 1]] diventa [1, 1] (non array di array).

### E2. Quale dei due loop for stampa più linee?
```swift
import Foundation
let data: [Any?] = ["Bill", nil, 69, "Ted"]

for datum in data where datum is String? {
    print(datum)
}

for case let .some(datum) in data where datum is String {
    print(datum)
}
```

- [ ] Entrambi i loop stampano lo stesso numero di linee
- [ ] Il primo loop stampa più linee del secondo
- [ ] Il secondo loop stampa più linee del primo
- [ ] Questo codice compila ma va in crash
- [ ] Questo codice non compila
*********************************
##### Riposta corretta: Il primo loop stampa più linee del secondo
##### Spiegazione  
C'è solo una piccola differenza tra i due loop ed è causato dal tipo di dato dell'array: il primo è un array di Any? non un array di stringhe.  
Il primo loop tenta di fare il typecast sui suoi elementi come String? ovvero l'elemento può essere sia una stringa sia nil (ciò risulta vero per 3 elementi).  
Il secondo loop inizia facendo l'unwrap degli opzionali quindi i suoi elementi possono essere Any o String ma è posta una condizione nella clausola where dunque il loop stamperà solamente due linee dato che solo due elementi sono di tipo String.

### E3. Cosa stampa il seguente codice?
```swift
let string: String = String(describing: String.self)
print(string)
```

- [ ] ""
- [ ] "String"
- [ ] Swift.String
- [ ] nil
- [ ] Non viene stampato nulla
- [ ] Questo codice compila ma va in crash
- [ ] Questo codice non compila
*********************************
##### Riposta corretta: "String"
##### Spiegazione  
Tra i vari costruttori per le stringhe ce n'è uno che permtte di passare una classe per ottenere la stringa del nome di quella classe.  
String(describing: String.self) significa "produci una stringa contenente il nome della classe String".  
Questa è equivalente al metodo NSStringFromClass() che si utilizzava in Objective-c

### E4. Quando questo codice viene eseguito cosa conterrà la costante example2?
```swift
var names = [String]()
names.append("Amy")
let example1 = names.removeLast()
let example2 = names.removeLast()
```

- [ ] ""
- [ ] "Amy"
- [ ] nil
- [ ] Non viene stampato nulla
- [ ] Questo codice compila ma va in crash
- [ ] Questo codice non compila
*********************************
##### Riposta corretta: Questo codice compila ma va in crash
##### Spiegazione  
Il metodo removeLast() ritorna lo stesso tipo di dato che è contenuto nell'array che, nel caso in oggetto è String. Siccome l'unica stringa nell'array è già stata rimossa, la seconda chiamata lancia un'eccezione e manda in crash l'applicaizone


### E5. Nel codice seguente quale sarà il tipo di dato di testVar?
  
In the code below, what data type is testVar?
```swift
let names = ["Pilot": "Wash", "Doctor": "Simon"]

for (key, value) in names.enumerated() {
    let testVar = value
}
```

- [ ] (String, String)
- [ ] String
- [ ] [String, String]
- [ ] Questo codice non compila
*********************************
##### Riposta corretta: (String, String)
##### Spiegazione  
Se si iterasse sulle chiavi di un dizionario senza utilizzare enumerated() le chiavi sarebbero Pilot e Doctor e i valori corrispondenti Wash e Simon. Utilizzando enumerated() ritornerà come key l'indice della posizione dell'elemento nel loop e il valore sarà di tipo (String, String).

### E6. Che output produrrà il seguente codice?
 ```swift
let status = "shiny"

for (position, character) in status.reversed().enumerated() where position % 2 == 0 {
    print("\(position): \(character)")
}
```

- [ ] 0: s
- [ ] 0: s, 2: i, 4: y
- [ ] 0: y
- [ ] 0: y, 2: i, 4: s
- [ ] 4: s, 2: i, 0: y
- [ ] 4: y, 2: i, 0: s
- [ ] Non viene stampato nulla
- [ ] Questo codice compila ma va in crash
- [ ] Questo codice non compila
*********************************
##### Riposta corretta: 0: y, 2: i, 4: s
##### Spiegazione  
Ci sono 3 cose da capire in questo codice:  
* reversed() è chiamato su una stringa prima di enumerated(), ciò significa che la stringa viene invertita ma non l'enumerazione
* enumerated() ritornerà ogni carattere nella stringa invertita con la sua posizione
* la posizione viene passata nella clausola where e verranno stampati solo i caratteri di posizione pari
 

### E7. Descrivi il flusso di esecuzione del seguente pseudo codice in RxSwift
 ```swift
searchBar.rx.text
    .throttle(0.3, scheduler: MainScheduler.instance)
    .flatMap { [spotifyClient] query in
        return spotifyClient.rx.search(query: query)
    }.map { tracks in
        return tracks.map(TrackRenderable.init)
    }.filter {
    	$0.timesListened > 1000000
    }.bindTo(tableView.rx.items(cellIdentifier: "TrackCell", cellType: TrackCell.self)) { index, track, cell in
        cell.render(trackRenderable: track)
    }.disposed(by: self.disposeBag)
 ``` 
 ### Risposta
* ad ogni carattere inserito in una textField viene notificata l'intera stringa
* vengono ignorate nuove emissioni nell'arco di 0.3 sec dall'ultimo carattere inserito
* con questa stringa si esegue una ricerca con il client di spotify
* i risultati vengono deserializzati in un array di oggetti TrackRenderable
* vengono filtrate solo le canzoni con più di 1000000 ascolti
* viene effettuato il binding tra i modelli e le celle di una tableview che popolerà i risultati delle canzoni contententi il testo cercato
* vengono popolate le celle con le varie label o immagini prese dall'oggetto track
* viene aggiunta la sottoscrizione al disposeBag che si preoccuperà della dispose quando verrà messo a nil (tipicamente quando viene deallocato self)
