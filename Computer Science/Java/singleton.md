###### 1 Static field

```java
public class Singleton {
	public static final Singleton INSTANCE = new Singleton();
}
```

  
**+** Простая и прозрачная реализация  
**+** Потокобезопасность  
**-** Не ленивая инициализация


###### 1 Synchronized Accessor

```java
public class Singleton {
	private static Singleton instance;
	
	public static synchronized Singleton getInstance() {
		if (instance == null) {
			instance = new Singleton();
		}
		return instance;
	}
}
```

  
**+** Ленивая инициализация  
**-** Низкая производительность (критическая секция) в наиболее типичном доступе

###### 2 Double Checked Locking & volatile

```java
public class Singleton {
        private static volatile Singleton instance;
	
        public static Singleton getInstance() {
		Singleton localInstance = instance;
		if (localInstance == null) {
			synchronized (Singleton.class) {
				localInstance = instance;
				if (localInstance == null) {
					instance = localInstance = new Singleton();
				}
			}
		}
		return localInstance;
	}
}
```
**+** Ленивая инициализация  
**+** Высокая производительность  
**-** Поддерживается только с JDK 1.5 [5]

Проблема идиомы Double Checked Lock заключается в модели памяти Java, точнее в порядке создания объектов. Можно условно представить этот порядок следующими этапами [2, 3]:  
  
Пусть мы создаем нового студента: Student s = new Student(), тогда  
  
1) local_ptr = malloc(sizeof(Student)) // выделение памяти под сам объект;  
2) s = local_ptr // инициализация указателя;  
3) Student::ctor(s); // конструирование объекта (инициализация полей);  
  
Таким образом, между вторым и третьим этапом возможна ситуация, при которой другой поток может получить и начать использовать (на основании условия, что указатель не нулевой) не полностью сконструированный объект. На самом деле, эта проблема была частично решена в JDK 1.5 [5], однако авторы JSR-133 [5] рекомендуют использовать voloatile для Double Cheсked Lock. Более того, их отношение к подобным вещам легко прослеживается из коментария к спецификации:

###### 3 On Demand Holder idiom
```java
public class Singleton {
		
	public static class SingletonHolder {
		public static final Singleton HOLDER_INSTANCE = new Singleton();
	}
		
	public static Singleton getInstance() {
		return SingletonHolder.HOLDER_INSTANCE;
	}
}
```
**+** Ленивая инициализация  
**+** Высокая производительность  
**-** Невозможно использовать для не статических полей класса