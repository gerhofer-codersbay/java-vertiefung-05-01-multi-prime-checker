# Java 

## Prime Checker 

Ziel dieser Übung ist es sich mit [completable Futures](https://www.baeldung.com/java-completablefuture) auseinander zu setzen. 
Vielleicht habt ihr in web schon über Promises gelernt, Futures sind Javas Pendant dazu. 

Deine Aufgabe ist es 1.000 zufällige Zahlen zwischen 0 und 999 999 999 zu generieren. 
Schreibe dann ein Programm, welches prüft ob es sich bei der generierten Zahl um eine Primzahl handelt. 

Für die 1.000 genierten Zahlen kann das schon ein bisschen dauern - die Probleme sind allerdings absolut unabhängig voneinander, 
können also parallel ausgeführt werden! Um immer über den aktuellen Progress informiert zu sein, Wollen wir in einer Zahl speichern, 
für wieviele unserer Zahlen die Berechnung bereits abgeschlossen wurde. 
Hierfür verwenden wir einen [atomic Integer](https://howtodoinjava.com/java/multi-threading/atomicinteger-example/),
denn dieser teilt sich über mehrere Threads hinweg seinen Wert und ist überall verfügbar. 

Deine Aufgabe ist es nun für jede generierte Zahl ein `CompletableFuture` zu starten, dass errechnet ob es sich um eine Primzahl handelt oder nicht.
Um die Ausführung etwas spannender zu gestalten lass die `boolean isPrime(Long longNumber)` für eine zufällige Zeit warten. 
Zu Beginn deiner `isPrime()` Methode solltest du einen `startedCounter` erhöhen und vor dem returnieren des Wertes von `isPrime` einen `finishedCounter`. 
Bei jeder Änderung der Werte solltest du den Wert loggen. Jeder Log sollte beinhalten in welchem Thread er ausgeführt wurde.
Mit `Thread.getCurrent()` kannst du auf den aktuellen Thread zugreifen.

Einige hilfreiche Methoden: 
* `CompletableFuture.supplyAsync(Supplier<U> supplier)` kann mit einer Lambda aufgerufen werden, die den supplier spezifiziert, sie wird verwendet um ein completable future zu erzeugen. 
* `myCompletableFuture.get()` oder `myCompletableFuture.join()` kannst du auf das Ergebnis eines Resultats warten und dieses holen. Der Unterschied zwischen `get()` und  `join()` liegt in den deklarierten checked Exceptions die `get()` werfen kann.

Der Konsolen Output sollte zirka so aussehen: 
```Thread #13 :started #1 checking 96299732
   Thread #18 :started #5 checking 97431235
   Thread #15 :started #4 checking 48305282
   Thread #16 :started #3 checking 11259924
   Thread #14 :started #2 checking 77518221
   Thread #17 :started #6 checking 15666916
   Thread #19 :started #7 checking 63520522
   Thread #13 :finished #1 96299732 is not a prime
   Thread #13 :started #8 checking 97692910
   Thread #13 :finished #2 97692910 is not a prime
   Thread #13 :started #9 checking 41544183
   Thread #16 :finished #3 11259924 is not a prime
   Thread #16 :started #10 checking 95629710
   Thread #19 :finished #4 63520522 is not a prime
   Thread #19 :started #11 checking 7611600
```