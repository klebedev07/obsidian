Еще один способ организовать блокировку кода, альтернатива [[synchronized java]] Гарантии те же что и у  [[synchronized java]], по сути более функциональная версия.




![[Pasted image 20230531171455.png]] 

![[Pasted image 20230601135734.png]]

Что бы поддержать методы wait(), notify(), notifyAll(), Существуют интерфейс Condition. Он неразрывно связан с ReentrantLock. Синтаксис:
![[Pasted image 20230601142026.png]]

### Пример
![[Pasted image 20230601142246.png]]