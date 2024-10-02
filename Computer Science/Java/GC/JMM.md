Возможности адресации памяти, предоставляемые архитектурой ОС, зависят от разрядности процессора, определяющего общий диапазон емкости памяти. Так, например, 32-х разрядный процессор обеспечивает диапазон адресации 2 в 32, то есть 4 ГБ. Диапазон адресации для 64-разрядного процессора (2 в 64) составляет 16 экзабайт.

Вся память jvm  состоит из двух областей
- [[heap]]
- [[Metaspace]](до java 8 назывался PermGen)
- [[Code Cache]]
- [[stack mem]]
![[Pasted image 20230608203833.png]]


Дополнительные источники информации о коммуникациях, могут быть найдены в следующих источниках:

- ["Презентация Troubleshooting Memory Issues in Java Applications"](https://www.oracle.com/webfolder/technetwork/tutorials/mooc/JVM_Troubleshooting/week1/lesson1.pdf) - краткое пояснение имеющихся областей памяти JVM.
- ["Гайд по флагам JVM"](https://www.baeldung.com/jvm-parameters) - список полезных флагов JVM с пояснением.
- ["HotSpot Virtual Machine Garbage Collection Tuning Guide"](https://docs.oracle.com/en/java/javase/11/gctuning/preface.html#GUID-5650179B-DC2A-4F25-B2C6-F3961C93FD07) - официальный документ от Oracle о настройке сборщиков мусора. В нём можно найти описания областей памяти и их сегментов.