### Создание объекта

  

```java
Map<String, String> hashmap = new HashMap<String, String>();
```

  
`Footprint{Objects=2, References=20, Primitives=[int x 3, float]}   Object size: 120 bytes`  
  
Новоявленный объект hashmap, содержит ряд свойств:  

- **table** — Массив типа **Entry[]**, который является хранилищем ссылок на списки (цепочки) значений;
- **loadFactor** — Коэффициент загрузки. Значение по умолчанию 0.75 является хорошим компромиссом между временем доступа и объемом хранимых данных;
- **threshold** — Предельное количество элементов, при достижении которого, размер хэш-таблицы увеличивается вдвое. Рассчитывается по формуле **(capacity * loadFactor)**;
- **size** — Количество элементов HashMap-а;

В [[ConcurrentHashMap]], ключи не могут быть null. При попытке добавления или использования null в качестве ключа, ConcurrentHashMap выбросит исключение `NullPointerException`.

Это отличается от обычной HashMap, которая позволяет использовать один null-ключ. Однако в ConcurrentHashMap такое поведение было изменено для обеспечения безопасности и предотвращения проблем с многопоточностью.

![[Pasted image 20230609124316.png]]

### Добавление элементов

  

```
hashmap.put("0", "zero");
```

  
`Footprint{Objects=7, References=25, Primitives=[int x 10, char x 5, float]}   Object size: 232 bytes`  
  
При добавлении элемента, последовательность шагов следующая:  

1. Сначала ключ проверяется на равенство null. Если это проверка вернула true, будет вызван метод **putForNullKey(value)** (вариант с добавлением null-ключа рассмотрим чуть [позже](https://habr.com/ru/articles/128017/#putForNullKey)).  
      
    
2. Далее генерируется хэш на основе ключа. Для генерации используется метод **hash(hashCode)**, в который передается **key.hashCode()**.  
      
    
    ```
    
    static int hash(int h)
    {
        h ^= (h >>> 20) ^ (h >>> 12);
        return h ^ (h >>> 7) ^ (h >>> 4);
    }
    ```
    
      
    Комментарий из исходников объясняет, каких результатов стоит ожидать — _метод **hash(key)** гарантирует что полученные хэш-коды, будут иметь только ограниченное количество коллизий (примерно 8, при дефолтном значении коэффициента загрузки)._  
      
    В моем случае, для ключа со значением **''0''** метод **hashCode()** вернул значение 48, в итоге:  
      
    
    ```
    
    h ^ (h >>> 20) ^ (h >>> 12) = 48
    
    h ^ (h >>>  7) ^ (h >>>  4) = 51
    ```
    
      
      
    
3. С помощью метода **indexFor(hash, tableLength)**, определяется позиция в массиве, куда будет помещен элемент.  
      
    
    ```
    
    static int indexFor(int h, int length)
    {
        return h & (length - 1);
    }
    ```
    
      
    При значении хэша **51** и размере таблице **16**, мы получаем индекс в массиве:  
      
    
    ```
    
    h & (length - 1) = 3
    ```
    
      
      
    
4. Теперь, зная индекс в массиве, мы получаем список (цепочку) элементов, привязанных к этой ячейке. Хэш и ключ нового элемента поочередно сравниваются с хэшами и ключами элементов из списка и, при совпадении этих параметров, значение элемента перезаписывается.  
      
    
    ```
    
    if (e.hash == hash && (e.key == key || key.equals(e.key)))
    {
        V oldValue = e.value;
        e.value = value;
                    
        return oldValue;
    }
    ```
    
      
      
    
5. Если же предыдущий шаг не выявил совпадений, будет вызван метод **addEntry(hash, key, value, index)** для добавления нового элемента.  
      
    
    ```
    
    void addEntry(int hash, K key, V value, int index)
    {
        Entry<K, V> e = table[index];
        table[index] = new Entry<K, V>(hash, key, value, e);
        ...
    }
    ```
    
      
    [![](https://habrastorage.org/r/w1560/storage1/25a37cf3/19b73e6b/efade65a/7981f807.png)  
    ](http://habrastorage.org/storage1/517a990b/09fe6798/cf03fb7f/88d6f1dd.png)

```java
// 1.
for (Map.Entry<String, String> entry: hashmap.entrySet())
    System.out.println(entry.getKey() + " = " + entry.getValue());

// 2.
for (String key: hashmap.keySet())
    System.out.println(hashmap.get(key));

// 3.
Iterator<Map.Entry<String, String>> itr = hashmap.entrySet().iterator();
while (itr.hasNext())
    System.out.println(itr.next());
```