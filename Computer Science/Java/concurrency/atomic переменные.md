Работают на основе CAS операций(Compare and set).

Условно можно разделить подходы реализации большинства atomic-методов на две группы: **compare-and-set** и **set-and-get**.  
  
Методы категории **compare-and-set** принимают старое значение и новое. _Если переданное старое значение совпало с текущим, устанавливается новое._ Обычно делегируют вызов в методы класса `Unsafe`, которые заменяются нативными реализациями виртуальной машины. Виртуальная машина в большинстве случаев использует атомарную операцию процессора [compare-and-swap](https://ru.wikipedia.org/wiki/%D0%A1%D1%80%D0%B0%D0%B2%D0%BD%D0%B5%D0%BD%D0%B8%D0%B5_%D1%81_%D0%BE%D0%B1%D0%BC%D0%B5%D0%BD%D0%BE%D0%BC) (CAS). Поэтому атомики обычно более эффективны чем стандартная дорогостоящая блокировка.

В случае **set-and-get** старое значение неизвестно. Поэтому нужен небольшой трюк: программа сначала считывает текущее значение, а затем записывает новое, тоже с помощью CAS, потому что запись могла успеть поменяться даже за этот шаг. Эта попытка чтения+записи повторяется в цикле, пока старое значение не совпадет и переменная не будет успешно записана.  
  
Этот трюк называется [double-checked или optimistic locking](https://ru.wikipedia.org/wiki/%D0%91%D0%BB%D0%BE%D0%BA%D0%B8%D1%80%D0%BE%D0%B2%D0%BA%D0%B0_%D1%81_%D0%B4%D0%B2%D0%BE%D0%B9%D0%BD%D0%BE%D0%B9_%D0%BF%D1%80%D0%BE%D0%B2%D0%B5%D1%80%D0%BA%D0%BE%D0%B9), и может быть использован и в пользовательском коде с любым способом синхронизации. Оптимистичность заключается в том, что мы надеемся что [состояния гонки](https://ru.wikipedia.org/wiki/%D0%A1%D0%BE%D1%81%D1%82%D0%BE%D1%8F%D0%BD%D0%B8%D0%B5_%D0%B3%D0%BE%D0%BD%D0%BA%D0%B8) нет, прибегая к синхронизации только если гонка всё же случилась. Реализация оптимистичной блокировки может быть дана как отдельная задача.

