- **Согласованность.  
    **Каждый процесс чтения получает последнюю запись или ошибку, соответственно, когда в системе запущено несколько параллельных процессов записи и чтения, то каждое чтение всегда возвращает последнюю запись, сделанную в системе.
- **Доступность.  
    **Принцип доступности в распределенной системе гарантирует, что система всегда остается работоспособной. Каждый запрос получает ответ без ошибок, независимо от индивидуального состояния узла. Впрочем, принцип не гарантирует, что ответ содержит самую последнюю запись (смотрите предыдущий пункт “Согласованность”).
- **Устойчивость к разделению.  
    **Распределенная система продолжает работу, даже когда отдельный узел не отвечает. Вышедший из строя узел подкрепляется вторичным узлом, поэтому вторичный узел заменяет первичный во время сбоев, а система становится отказоустойчивой. Хотя некоторые сообщения все-таки могут выходить из строя.![[Снимок экрана 2024-03-03 в 20.49.26.png]]

![[Pasted image 20240303205123.png]]