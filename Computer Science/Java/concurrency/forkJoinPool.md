В отличие от обычных [[Executor]]  не работает с одной блокирующей [[Queue]]. Так как задачи постоянно дробяться исполняющими потоками и снова добавляются в очередь, то идет активная работа с началом и концом очереди. А так как очередь блокирующая, то это становится узким местои и начинает тормозить.

 ForkJoinPool создает отдельную очередь для каждого потокав пулле. Каждыйц поток работает только с концом своей очереди. Если в очереди задачи нет, то поток забирает задачу из начала другой не пустой очереди. Так как начало очереди синхронизировано то работа безопасна.


![[Pasted image 20230613171442.png]]
  
  ![[Pasted image 20230613165100.png]]
  ![[Pasted image 20230613165948.png]]
  ![[Pasted image 20230613170014.png]]

![[Pasted image 20230613170309.png]]


Этот пулл работает с абстрактным классом ForkJoinTask
![[Pasted image 20230613165219.png]]

Так же работает с Runnable and Callable

![[Pasted image 20230613165237.png]]
 
![[Pasted image 20230613165507.png]]

### Параметр asyncMode
По умолчанию false. Поток всегда работает с концом очереди и кладет туда и берет задачи оттуда(по принципу стэка).  Из плюсов синхронизировано только начало очереди и задачи легко объеденяются
Если значение true  то поток кладет задачи в начало , а берет из конца. 
Позволяет добиться равномерного разделения нагрузки. Неизвестно когда и кем выполнится задача. Если элементы разнотипные и не нужен результат , то хорошо подходит.



Рекомендуется использовать один пулл на всю программу. Есть статический метод ForkJoinPool.coomonPool().  

Из плюсов использовать общий пулл, это то что потоки используются по максимуму, 
минусов имена потоков невнятные, может быть неудобно при отладке. Нет кастомной обработки ошибок.

