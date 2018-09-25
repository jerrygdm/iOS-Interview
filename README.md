# iOS-Interview



## BASE

### Quali sono i principali stati di un'applicazione iOS?
Non-running — L'applicazione non è in esecuzione
Inactive — L'applicazione è in esecuzione in foreground ma non sta ricevendo eventi. Un'app iOS può essere messa nello stato "inactive" dal sistema operativo quando ad esempio si riceve una telefonata o un SMS.
Active — L'applicazione è in esecuzione e sta ricevendo eventi.
Background — L'applicazione è in esecuzione in background e sta eseguendo del codice.
Suspended — L'applicazione è in esecuzione in background e non sta eseguendo alcun codice.

- Quali sono le differenze tra frame e bounds?
Il bounds di una UIView rappresenta il rettangolo espresso con una posizione (x,y) e le sue dimensioni (width, height) relative al suo sistema di coordinate (0, 0)
Il frame di una UIView è allo stesso modo un rettangolo con una posizione e dimensione ma riferito rispetto alle coordinate della superview che lo contiene.

- Spiegare il pattern MVC
MVC è il pattern consigliato da Apple per lo sviluppo su iOS ed è la base dei framework Cocoa e Cocoa Touch.
MVC sta per Model View Controller, definisce una separazione logica dei componenti e come essi comunicano. 
Models — responsabile per il dominio riguardante l’accesso ai dati dell’applicazione
Views — responsabile per il livello di presentazione e interazione (GUI).
Controller  — è il mediatore tra il modello e la view, solitamente riceve i comandi dall’utente e aggiorna il modello. Viceversa un evento può scatenare un aggiornamento del modello, tipicamente con il KVO o tramite un sistema di notifiche viene avvertito il Controller che aggiornerà a sua volta la view di conseguenza.
 
- Qual'è la differenza tra: fileprivate, private e public private(set) in Swift?
Fileprivate è accessibile all'interno del file corrente, private è riferito all'interno della corrente dichiarazione mentre public private(set) significa che il getter è pubblico mentre il setter è privato.

- Cos'è il "Forced Unwrapping" in Swift?
Quando si dichiara una variabile opzionale, per ottenere il suo valore è necessario l'unwrap. Ciò è possibile tramite l'optional binding o il force unwrap.
Il force unwrap fa si che si forzi l'unwrap in modo che il valore non sia più opzionale. Il force unwrap è potenzialmente pericoloso perchè nel caso la variabile sia nil il force unwrap lancerà un'eccezione che manderà in crash l'applicazione.

- Perchè si utilizzano le struct invece che le class?
Le struct sono di tipo "value type" ovvero una sua copia o il passaggio di parametro avviene per valore mentre nel caso delle classi la copia avviene per "reference" ovvero viene copiato il puntatore.
Dove usare uno o l'altro dipende dal contesto. Si preferiscono le classi quando copiare o comparare le istanze non ha senso nel contesto corrente.

- Qual'è la differenza tra viewDidLoad e viewDidAppear? Nel quale è preferibile inserire una chiamata ad un server remoto in una classica architettura che utilizza il pattern MVC?
Nel contesto degli UIViewController il ViewDidLoad è il metodo eseguita dal sistema dopo che la view viene caricata in memoria e soprattutto una sola volta. ViewDidAppear viene chiamata ogni volta che la view è appare visibile all'utente.
Inserendo una chiamata di rete nel viewDidAppear scatenerebbe una richiesta ad ogni visualizzazione della view del controller in questione. E’ quindi preferibile, a meno di casi particolari, inserire la chiamata nel viewDidLoad.

- Cosa non è possibile inserire in un array o in un dizionario (Obj-C)?
Nil non è un oggetto che è possibile inserire in un array o un dizionario in Objective-C. Questo è il motivo per il quale addObject(nil) manda in crash l'applicazione.

- Per eseguire una ricerca tra un insieme di elementi risulta più veloce un NSArray o un NSSet?
Quando l'ordine degli elementi in una collection non è importante NSSet è molto più performante in quanto i Set utilizzano dei valori di hash per trovare gli elementi (similarmente a un dizionario) mentre un array deve essere iterato finchè non si trova l’elemento (O(1) vs O(n)).

- Per fare una richiesta http di rete è preferibile usare URLConnection o URLSession?
Da iOS 7.0 URLSession è preferito, prevede la funzionalità del download in background (mentre l’app non è in esecuzione) e il raggruppamento di varie richieste rendendo più semplice la loro cancellazione relativa ad un determinato lavoro. URLSession prevede una migliore sintassi che utilizza i “blocks”.

- Perchè dovresti preferire una chiamata http asincrona rispetto alla controparte sincrona? In Swift qual è la sintassi per eseguire un’istruzione in modo asincrono?
Una chiamata asincrona non blocca il thread di UI detto il main thread. In swift si utilizza il seguente codice:
Dispatch.main.async {
	// codice da eseguire
}

*********************************
```swift
class Person {
    var name: String
    var age: Int
}

let myFriend = Person(name: "Peter", age: 24)
print(myFriend.name)
```
----------------------------------------------

- [ ] ""
- [ ]  "Peter"
- [ ] "myFriend"
- [ ] nil
- [ ] Non viene stampato nulla
- [ ] Questo codice compila ma va in crash
- [ ] Questo codice non compila

 ++++++
Riposta corretta: Questo codice non compila
Spiegazione: Le Struct supportano la "memberwise initialization" mentre le classi non ancora nelle attuali versioni di Swift. Questo codice non compila perchè la classe non ha un inizializzatore 
*********************************
 
 ```swift
var i = 2

do {
    print(i)
    i *= 2
} while (i < 128)
 ```
 -----------

- [ ] 2, 4, 8, 16, 32, 64
- [ ] 2, 4, 8, 16, 32, 64, 128
- [ ] Non viene stampato nulla
- [ ] Questo codice compila ma va in crash
- [ ] Questo codice non compila

+++++
Risposta corretta: Questo codice non compila.
Spiegazione: La keyword "do" non è corretta qui, occorre usare ia keyword "repeat".
*********************************

```swift

let names = ["Amy", "Rory"]

for name in names {
    name = name.uppercased()
    print("HELLO, \(name)!")
}
```
-----------------------

- [ ] "HELLO, AMY!"
- [ ] "HELLO, AMY!", "HELLO, RORY!"
- [ ] "HELLO, RORY!"
- [ ] Non viene stampato nulla
- [ ] Questo codice compila ma va in crash
- [ ] Questo codice non compila
  
 +++++
Risposta corretta: Questo codice non compila.
Spiegazione: name è definito implicitamente come costante (let) non può essere modificato. Per renderlo modificabile bisognerebbe forzarlo come var
*********************************
```swift
let names = [String]()
names.append("Amy")
names.append("Clara")
names.append("Rory")
```
------------

- [ ] 0
- [ ] 1
- [ ] 2
- [ ] 3
- [ ] Questo codice compila ma va in crash
- [ ] Questo codice non compila
 
 ++++
 Risposta corretta: Questo codice non compila
 Spiegazione: L'array names è dichiarato come let ovvero come costante. Per renderlo modificabile occorre usare var try to use append() to add strings to it.
 *********************************
 ```swift
let first = ["Sulaco", "Nostromo"]
let second = ["X-Wing", "TIE Fighter"]
let third = first + second
```
------------

- [ ] "Sulaco", "Nostromo"
- [ ] "Sulaco", "Nostromo", "Sulaco", "Nostromo"
- [ ] "Sulaco", "Nostromo", "X-Wing", "TIE Fighter"
- [ ] "X-Wing", "TIE Fighter", "Sulaco", "Nostromo"
- [ ] Il risultato è un array vuoto
- [ ] Questo codice compila ma va in crash
- [ ] Questo codice non compila
 +++++
 Risposta corretta: "Sulaco", "Nostromo", "X-Wing", "TIE Fighter".
Spiegazione: Gli array in Swift possono essere uniti usando l'operatore +, la sua applicazione comporta il concatenamento del secondo array alla fine del primo
 *********************************
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
-----------

- [ ] Prints "Number was 2"
- [ ] Prints "Number was 3"
- [ ] Questo codice compila ma va in crash
- [ ] Questo codice non compila

++++++++
Risposta corretta: Questo codice non compila
Spiegazione: Swift esige che tutti i casi di uno switch siano dichiarati, alternativamente è possibile utilizzare il case default.
*********************************
```swift
let bigNumber = Int.max
let biggerNumber = bigNumber + 1
print(biggerNumber)
```
-------------
- [ ] -9223372036854775808
- [ ] 0
- [ ] 9223372036854775807
- [ ] Non viene stampato nulla
- [ ] Questo codice compila ma va in crash
- [ ] Questo codice non compila

 ++++++++
 Rispota corretta: Questo codice compila ma va in crash.
Spiegazione: L'aggiunta di 1 al massimo numero intero rappresentabile solleva un overflow.
 *********************************

 
 
 
 

## INTERMEDIATE

- Cosa accade se si invoca un metodo su un puntatore che è a nil?
In Objective-C è considerata una no-op, è una operazione che non ha alcun effetto, l'app non va in crash.

- Differenze tra setNeedsLayout, layoutIfNeeded e layoutSubviews?
Il metodo setNeedsLayout chiede al sistema di ridisegnare una vista e le sue sottoviste associate, il metodo ritorna immediatamente, è asincrono e dunque non è indeterminato il momento del completametno dell’operazione.
LayoutIfNeeded è un metodo sincrono, chiede al sistema l’immediato ridisegno di una vista e delle eventuali sottoviste.
Le sottoclassi di UIView possono fare l’override di LayoutSubviews per definire più precisamente il layout delle viste devono apparire all’interno della vista in oggetto.

- What is Git? Are you using and how?
- Explain Autolayout, Constraints
Auolayout dinamicamente calcola la grandezza e la posizione di tutte le viste una particolare gerarchia di view.
I constraints sono i vincoli che si aggiungono alle viste per far in modo che autolayout possa calcolare tutto con precisione.

- Qual'è lo scopo del reuseIdentifier?
Il reuseIdentifier è importante nell'utilizzo di UITableView e UICollectionView. Il reuseIdentifier dichiara quali sono le celle che possono essere riciclate senza dover doverle ricreare (risparmiando tempo e memoria). Internamente al sistema vengono raggruppate le celle che differiscono solo del contenuto ma non del layout. L'utilizzo del reuseIdentifier è essenziale per avere una buona velocità nello scroll. E' compito del programmatore gestire il riciclo delle celle in modo che mentre l'utente scrolla il contenuto deve essere corretto e non anch'esso riciclato.


- Explain MVVM 
UIKit independent representation of your View and its state. The View Model invokes changes in the Model and updates itself with the updated Model, and since we have a binding between the View and the View Model, the first is updated accordingly.
Your view model will actually take in your model, and it can format the information that’s going to be displayed on your view.
There is a more known framework called RxSwift. It contains RxCocoa, which are reactive extensions for Cocoa and CocoaTouch.


- Explain [weak self] and [unowned self] ?
unowned ( non-strong reference ) does the same as weak with one exception: The variable will not become nil and must not be an optional.
When you try to access the variable after its instance has been deallocated. That means, you should only use unowned when you are sure, that this variable will never be accessed after the corresponding instance has been deallocated.
However, if you don’t want the variable to be weak AND you are sure that it can’t be accessed after the corresponding instance has been deallocated, you can use unowned.
Every time used with non-optional types
Every time used with let
By declaring it [weak self] you get to handle the case that it might be nil inside the closure at some point and therefore the variable must be an optional. A case for using [weak self] in an asynchronous network request, is in a view controller where that request is used to populate the view.


**************
Che output viene prodotto dal codice seguente?
```swift
let names = ["Pilot": "Wash", "Doctor": "Simon"]
let doctor = names["doctor"] ?? "Bones"
print(doctor)
```
----------
- [ ] ""
- [ ] "Bones"
- [ ] "Doctor"
- [ ] nil
- [ ] "Wash"
- [ ] Non viene stampato nulla
- [ ] Questo codice compila ma va in crash
- [ ] Questo codice non compila
 
 +++++++
 Risposta corretta: "Bones".
Spiegazione: Viene fatto accesso all'elemento "doctor" nel dizionario, chiave mai inserita: i dizionari sono case sentitive. Ciò comporta come risultato nil che scatena l'esecuzione del "nil coalescing operator" che valorizzerà la variabile doctor a "Bones".
 *********************************
 ```swift
let userLoggedIn: Bool? = false

if !userLoggedIn! {
    print("Message one")
} else {
    print("Message two")
}
```
---------------
- [ ] "Message one"
- [ ] "Message two"
- [ ] Non viene stampato nulla
- [ ] Questo codice compila ma va in crash
- [ ] Questo codice non compila
 
 ++++++++
 Risposta corretta: "Message one".
Spiegazione: L'espressione !userLoggedIn! significa: force unwrap del bool e negalo. Il booleano è impostato a false quindi la sua negazione viene valutata in true e il risultato è la stampa del primo messaggio.
 *********************************
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
------------
- [ ] X was 556
- [ ] X was 556 and Y was 0
- [ ] Y was 0
- [ ] Non viene stampato nulla
- [ ] Questo codice compila ma va in crash
- [ ] Questo codice non compila
 
 +++++++
 Risposta corretta: X was 556.
Spiegazione: Swift esegue il primo blocco che viene "matchato" in uno switch. In questo caso point è una tupla con il valore (556, 0) quindi (let x, 0) risulta soddisfatto e il messaggio relativo stampato.
 *********************************




## AVANZATO

- What are differences between Manual and Automatic Reference Counting?

- Quali sono le differenze tra Sequence Protocol e Collection Protocol?
Una sequenza rappresenta un insieme di valori che possono essere iterati tramite un for-loop.
Collection è una Sequence che dove ci si può accedere agli elementi tramite subscript e definisce l'indice di partenza (startIndex) e di fine (endIndex). Si può accedere più volte ad ogni singolo elemento di una Collection mentre nel caso della Sequence non è più possibile a meno di una nuova iterazione.
Come ultimo punto Collection eredita da Sequence. 



****************
 
 Quando questo codice è eseguito, cosa conterrà la costante numbers?
```swift
let numbers = [1, 2, 3].flatMap { [$0, $0] }
```
----------------
- [ ] [1, 1, 2, 2, 3, 3]
- [ ] [1, 2, 3]
- [ ] [[1, 1], [1, 2], [1, 3]]
- [ ] [[1, 1], [2, 1], [3, 1]]
- [ ] [[1, 1], [2, 2], [3, 3]]
- [ ] [[1, 2, 3], [1, 2, 3], [1, 2, 3]]
- [ ] Questo codice compila ma va in crash
- [ ] Questo codice non compila
 
 +++++++++
 Risposta corretta: [1, 1, 2, 2, 3, 3].
Spiegazione: l'invocazione di map comporta in un loop di tutti gli elementi nell'array e risulta in una creazione di un nuovo array che contiene ogni numero duplicato. Ad esempio il primo numero (1) viene convertito in [1, 1] e così via. In questo caso viene utilizzato flatMap che appiattisce il tutto in un singolo array ovvero [[1, 1]] diventa [1, 1] (non array di array).
****************
Quale dei due loop for stampa più linee?
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

 +++++++++
 Risposta corretta: Il primo loop stampa più linee del secondo
Explanation: There is a very subtle difference between the two loops, and it's triggered by the data type of the array: this is an array of Any? not an array of strings. The first loop will attempt to typecast its items as String?, which means the loop element must either be a string or nil – that's true of three items. The second loop, however, begins by unwrapping the optional, so it will either be Any or String, at which point our where clause will work. So, the second loop prints two lines.
****************
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
 
 +++++++++++
 Risposta corretta: "String"
Explanation: Among the many constructors for strings is one that lets you pass in a class to have the string set to the name of that class. That is, String(describing: String.self) means "create a string out of the name of the String class." This is equivalent to the NSStringFromClass() function that Objective-C developers often use.
 **********************
 
When this code is executed, what will example2 be set to?
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
 ++++++++++
Risposta corretta: Questo codice compila ma va in crash
Spiegazione: The removeLast() method returns the same data type as the array contains, which in this code is a String. As the only string in the array was already removed, the second call will throw an exception and crash

 *****************
  
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
+++++++++++

Risposta corretta: (String, String).
Explanation: If you iterate over the names dictionary without using enumerated(), key would be Pilot then Doctor, and value would be Wash then Simon. However, this code uses enumerated(), which means that key will be the integer position of the item in the loop, and value will be a (String, String) tuple containing the key and the value for this item in the dictionary: ("Pilot", "Wash") then ("Doctor", "Simon").
 ***************
 
 What output will be produced by the code below?
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
 ++++++++++++
Risposta corretta: 0: y, 2: i, 4: s.
Explanation: There are three things to understand about this code. First, reversed() is called on the string before enumerated(), which means the string is reversed but the enumeration (the positions) are not. Second, enumerated() will return each character in the reversed string, along with its position. Third, that position is passed into a where clause, and only even-numbered character indices will be printed.

 ******************
 
 
 
 
 
 

