В [[Java]]  механизм позволяющий использовать множество потоков [[Threads( Потоки)]] без накладных расходов. 

 На схеме ниже очередь блокирущая. Блокируется начало и конец очереди. В один момент времени только один поток может работать с началом или концом. Это отличаетcя по принципу от  [[forkJoinPool]]
![[Pasted image 20230530154327.png]]


Иерархия executor-ов 
![[Pasted image 20230530155232.png]]

В зависимости от потребностей нужно выбирать нужный тип. Мы можем гибко настраивать размер очереди, время жизни, максимальное и минимальное количество потоков. 


С помощью размера очереди мы можем мониторить нагрузку на трэдпулл. и в случае чего его тюнить. 

![[Pasted image 20230530155851.png]]


На графике ниже показана зависимости загрузки ЦПУ от времени нахождения задачи в очереди 
![[Pasted image 20230530160154.png]]

![[Pasted image 20230530160216.png]]