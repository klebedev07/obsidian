В [[Java]] ключевое слово с помощью которого можно добиться синхронизации участка кода.


По сути с каждым объектом ассоциирована специфичная структура которая называется монитор([[monitor(concurrency)]]).
Когда поток([[Threads( Потоки)]]) входит в синхронайзд секцию,которая охраняется какойто блокировкой(по сути объект [[Lock(concurrency)]])
то у монитора прописывется владелец. ЭТо называется захват монитора.  
**Захват монитора** происходит на уровне JVM и не доступен программисту. Только один поток может находится в синхронайзд секции.![[Pasted image 20230531165251.png]]


МОдификатор синхронайзд можно использовать несколькими способами 
![[Pasted image 20230531165414.png]]

### Рекомендации 
- По возможности сокращаем синхронайзд блоки
- Не синхронизируемся по строкам и оберткам над примитивами. JVM имеет кэши для этих типов. Поэтому возможно может получится так что из разных участков кода смотрим в один объект

Очень важное свойство synchronized это [[Reentrancy(synchronized)]]

### Недостатки
-  Только один поток может работать с синхронизированным блоком
-  Нет таймаута на владения и ожидание монитора. Можно надолго застрять в ожидание 
- Нельзя никак проверить занят ли монитор. Нужно встать в очередь и зависнуть в ожидании
- Нет очередности захвата. Если поток встал первым в ожидании монитора, это не гарантирует что при освобождение монитора кто то другой его не перехватит раньше.
- Поток не реагирует на interrupt()

### Альтернитвы
- [[Lock(concurrency)]] интерефейс и его реализация
- Semaphore, CountDownLatch, CyclicBarrier.....