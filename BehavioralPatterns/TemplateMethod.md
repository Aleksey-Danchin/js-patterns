[← Назад](/README.md "Вернуться на главную страницу")

# Template Method / Шаблонный метод

![Template Method](https://hsto.org/getpro/habr/post_images/2d6/a9d/443/2d6a9d443800486ed3ba432da9954df8.jpg)

```javascript
class RegisterUser {
	register () {
		const data = this.getData()
		const isCorrect = this.filterData(data)
		
		if (isCorrect) {
			this.sendData(data)
		}

		else {
			this.errorHandler(data)
		}

		this.finish()
	}
}

class FormRegister extends RegisterUser {
	getData () {
		// Обработка формы на странице
		return {
			name: 'Aleksey'
		}
	}

	filterData (data) {
		return data.name.length > 0
	}

	sendData (data) {
		console.log('Данные ушли на сервер', data)
	}

	errorHandler (data) {
		console.warn('Что-то пошло не так!', data)
	}

	finish () {
		console.log('Конец регистрации пользователя.')
	}
}

class ManualRegister extends RegisterUser {
	getData () {
		// Вручную вбиваем
		return {
			name: prompt('Имя пользователя')
		}
	}

	filterData (data) {
		return data.name.length > 0
	}

	sendData (data) {
		console.log('Данные сохранены локально. При повторном соединение с сервером данные будут отправлены.', data)
	}

	errorHandler (data) {
		console.warn('Что-то пошло не так!', data)
	}

	finish () {
		console.log('Пользователь будет скоро зареган.')
	}
}

(new FormRegister).register()
(new ManualRegister).register()
```