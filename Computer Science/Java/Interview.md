 ### Core
- hashmap java8 [[Map]]O
- treemap - [[Map]]
-  strings and memory and differeence between "" and new String("") [[String]]
- [[Почему дабл плохо для денег]]
- Как вызываются конструкторы - от родителя и вниз по иерархии
-  [[Exceptions]] 
- Что такое аспект  [[aspect]]
- будет ли бытрее проход по массиву через parrallel stream - да если на тачке есть доп ядра. [[forkJoinPool]]
- [[GC]]
-  [[Почему пароли нельзя хранить в строках в java]]
- [[транзакции jdbc]]
- treeMap!!!!!!
- [[транзакции jdbc]] - там не хватает еще одного чтения, что то про атомарность

### Concurrency
- Потоки и процессы  разница. [[Threads( Потоки)]] [[Процессы]]
- Сколько памяти поток java - обычно около 1 МБ  [[Threads( Потоки)]] 
- [[JMM]] - java memory model
-  [[Как подбирать количество потоков для задач]]
- ConcurrentHashMap - https://habr.com/ru/articles/132884/
- [[Executor]]
- [[synchronized java]]
- [[forkJoinPool]]
- [[Разница между параллельностью и асинхронностью]]

### KAFKA
- Можно ли из трех партиций вытащить в том порядке в котором они добавлены были - нет
- [[consumer]]
- [[Producer]]
- [[Partition]]
- [[Replication]]
- [[семантика гарантий доставки]]

### Web flux
- [[Reactive Systems]]
- [[cold hot publisher]]
- map vs flatMap
- [[Back pressure]]
- как обработать блокирующий участок -  через специальный метод в котором передать свой трэдпул
- 

### SQL
- Проблема декартова умножения - https://sysout.ru/problema-dekartova-proizvedeniya-cartesian-product-problem/
- Индексы
- [[ACID]]
- [[UNION vs UNION ALL]]  
- 
- [[select for update]]
- [[Реалазации индексов в бд B-tree, Hash, GiST, SP-GiST, GIN, BRIN]]  
- [[Когда мы можешь заюзать для оптимизаци нормализацию/денормолизацию]]  
- [[Какой пул у бд в среднем и почему пул потоков ограничен пулом]]  
- [[Как лучше хранить время в бд? Ответ: timestamptz - путь к успеху]]

### SPRING 
- spring boot starter
-  spring factories

### DevOps
- [[Docker]]
- [[ingress]] kubernetes
- [[Service]]
- [[configMap]]
- 
- _Deployments_ - контроллер, который управляет состоянием развертывания подов, которое описывается в манифесте, следит за удалением и созданием экземпляров подов. Управляет контроллерами ReplicaSet.
- 
-  [[Разница между шардированием и репликацией]]
-  cassandra


### KOTLIN 
- [[coroutines]]
- [[inline function]]
- дженерики котлин!!!!
- 



- [[CAP-теорема]]
- RPC -GRPC 
- **кеш-линии**
- # Когерентность кэша



1. Товары в разных странах разные. Склады тоже. Как лучше разделить?
2. Кафка кластеры как лучше гегорафически распологать
3. гео балансер и гео днс
4. Конфигурация цодов , Active-active
5. блю грин деплой и канарейка
6. типичный кластер кубера