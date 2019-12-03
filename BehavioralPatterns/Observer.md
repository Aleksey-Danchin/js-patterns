[← Назад](/README.md "Вернуться на главную страницу")

# Observer / Слушатель / Наблюдатель

![Observer](https://hsto.org/getpro/habr/post_images/cad/355/d48/cad355d48e3b9a2debcad55bc6504beb.jpg)

```javascript
class Observable {
	constructor () {
		this.listeners = []
	}

	addListener (listener) {
		this.listeners.push(listener)
	}

	emit (...args) {
		for (const listener of this.listeners) {
			listener(...args)
		}
	}
}

const story = new Observable

story.addListener(message => {
	console.log('1-й обработчик', message)
})

story.addListener(message => {
	console.log('2-й обработчик', message)
})

story.emit('Какое-нибудь сообщение')

/*
1-й обработчик Какое-нибудь сообщение
2-й обработчик Какое-нибудь сообщение
*/
```