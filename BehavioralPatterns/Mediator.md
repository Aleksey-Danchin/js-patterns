[← Назад](/README.md "Вернуться на главную страницу")

# Mediator / Посредник

![Mediator](https://hsto.org/getpro/habr/post_images/9c4/8eb/8ab/9c48eb8ab34d448fc5886c5c965de090.jpg)

```javascript
class EventEmitter {
	constructor () {
		this.handlers = {}
	}

	on (name, handler) {
		if (!this.handlers[name]) {
			this.handlers[name] = []
		}

		this.handlers[name].push(handler)
	}

	emit (name, ...args) {
		if (this.handlers[name]) {
			for (const handler of this.handlers[name]) {
				handler(...args)
			}
		}
	}
}

class Button extends EventEmitter {
	constructor (label) {
		super()

		this.label = label
	}

	click (...args) {
		this.emit('click', ...args)
	}
}

class Modal extends EventEmitter {
	constructor (title) {
		super()

		this.title = title
		this.opened = false
	}

	open () {
		if (this.opened) {
			return
		}

		this.opened = true
		this.emit('open')
	}

	close () {
		if (!this.opened) {
			return
		}

		this.opened = false
		this.emit('close')
	}
}

const openButton = new Button('Открыть модальное окно')
const closeButton = new Button('Закрыть модальное окно')
const sendMessageButton = new Button('Отправить сообщение')
const modal = new Modal('Модальное окно')

openButton.on('click', () => modal.open())
closeButton.on('click', () => modal.close())

modal.on('open', () => console.log('Модальное окно открыли'))
modal.on('close', () => console.log('Модальное окно закрыли'))

openButton.click()
closeButton.click()

sendMessageButton.on('click', (message = '') => {
	if (message.length < 5) {
		closeButton.click()
		modal.open()
	}
})

sendMessageButton.click('Оки')

/*
Модальное окно открыли
Модальное окно закрыли
Модальное окно открыли
*/
```

```javascript
class Mediator {
	constructor () {
		this.modal = new Modal('Модальное окно')

		this.modal.on('open', () => console.log('Модальное окно открыли'))
		this.modal.on('close', () => console.log('Модальное окно закрыли'))
	}

	sendMessage (message) {
		if (message.length < 5) {
			this.modal.close()
			this.modal.open("Сообще не может быть отправленно")
		}
	}

	openWindow () {
		this.modal.open()
	}

	closeWindow () {
		this.modal.close()
	}
}

class EventEmitter {
	constructor () {
		this.handlers = {}
	}

	on (name, handler) {
		if (!this.handlers[name]) {
			this.handlers[name] = []
		}

		this.handlers[name].push(handler)
	}

	emit (name, ...args) {
		if (this.handlers[name]) {
			for (const handler of this.handlers[name]) {
				handler(...args)
			}
		}
	}
}

class Button extends EventEmitter {
	constructor (label) {
		super()

		this.label = label
	}

	click (...args) {
		this.emit('click', ...args)
	}
}

class Modal extends EventEmitter {
	constructor (title) {
		super()

		this.title = title
		this.opened = false
	}

	open () {
		if (this.opened) {
			return
		}

		this.opened = true
		this.emit('open')
	}

	close () {
		if (!this.opened) {
			return
		}

		this.opened = false
		this.emit('close')
	}
}

const openButton = new Button('Открыть модальное окно')
const closeButton = new Button('Закрыть модальное окно')
const sendMessageButton = new Button('Отправить сообщение')

const mediator = new Mediator

openButton.on('click', () => mediator.openWindow())
closeButton.on('click', () => mediator.closeWindow())
sendMessageButton.on('click', message => mediator.sendMessage(message))

openButton.click()
closeButton.click()

sendMessageButton.click('Оки')

/*
Модальное окно открыли
Модальное окно закрыли
Модальное окно открыли
*/
```