Html внутри javaScript'а. [[ReactJs]] интерпретирует ее как особые инструкции по созданию [[Virtual DOM]]

В браузер попадает js, babel занимается преобразованием jsx в js


Если внутри jsx разметки нужно вызвать javascript то выделяем его в фигурные скобки. Например в следующем участке кода props.message является кодом внутри разметки:
```javascript
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
