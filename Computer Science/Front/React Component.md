Компонента кусок какого то HTML. Используется в [[ReactJs]] КАждый раз когда мы создаем новую **функцию** возвращающую JSX,у нас по сути появляется новый тэг, который мы можем в дальнейшем использовать.

По сути это функция принимающа [[props (ReactJS)]] возвращающая [[JSX разметка]]  Вней всегда только один главный тэг в котором содержаться все остальные.

![[Pasted image 20230509235833.png]]


Компонента может быть классовой. Выглядит примерно так: 
![[Pasted image 20230510000225.png]]

Укаждой компоненты есть жизненый цикл компоненты [[Component LifeCycle]]


Компонента вызывается как тэг.

Компоненты могут быть:
- Контейнерные - в них зашита бизнес логика и работа с [[Context]] и [[React Store]]
- Презентационная - она отвечает только за отрисовку, никакой логики работы со стэйтом там не должно быть

Это позволяет нам разделить ответсвенности [[Single Responsibility]] из [[SOLID]]
 
Компонента имеет [[props (ReactJS)]] .  Это набор значений позволяющий делать наши компоненты динамическими. Свойства конкретной компоненты.

Компонента стилизуется [[CSS modules]]



Пример  cамой компоненты
```javaScript
const Post = (props) => {  
return (
		<div className={classes.item}>  
			{props.message}  
			<div>  
				<span>Like</span>  
			</div>  
		</div>  
	)
}
```

Пример как она далее вызовется в другой компоненте. Параметр messgae и есть наш props

```JavaScript
const MyPosts = () => {  
return (
	<div >
	my posts   
		<div>  
			<Post message = "First post"/>   
			<Post message = "Second post"/>  
		</div>  
	</div>  
  )
}
```



