[← Назад](/README.md "Вернуться на главную страницу")

# Proxy / Прокси / Заместитель

![Proxy](https://hsto.org/getpro/habr/post_images/3c3/c0f/87d/3c3c0f87d7e200b0b383223e547c7f4e.jpg)

```javascript
class User {
	constructor (name, family) {
		this.name = name
		this.family = family
	}

	greeting () {
		console.log(`Всем привет. Я ${this.name} ${this.family}.`)
	}
}

class ProxyUser {
	constructor (user) {
		this.user = user
	}

	get name () {
		console.log('Кто-то запросил имя')
		return this.user.name
	}

	set name (value)  {
		console.log(`Кто-то пытается присовить имя "${value}" пользователю.`)
		return this.user.name = value
	}

	greeting () {
		console.log('Кто-то пытается вызвать метод "greeting"')
		return this.user.greeting()
	}
}

const user = new ProxyUser(new User('Алексей', 'Данчин'))

user.greeting()
```

```javascript
class User {
	constructor (name, family) {
		this.name = name
		this.family = family
	}

	greeting () {
		console.log(`Всем привет. Я ${this.name} ${this.family}.`)
	}
}

const user = new Proxy(
	new User('Алексей', 'Данчин'),
	{
		get (target, name, receiver) {
			console.log(`Кто-то запросил ${name}`)
			
			return target[name] !== undefined ? target[name] : `"${name}" value doesn't exist. Hi! I'm proxy )))`
		}
	}
)

console.log(
	user.name,
	user.family,
	user.age
)
/*
Кто-то запросил name
Кто-то запросил family
Кто-то запросил age
Алексей Данчин "age" value doesn't exist. Hi! I'm proxy )))
*/
```