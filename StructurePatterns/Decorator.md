[← Назад](/README.md "Вернуться на главную страницу")

# Decorator / Декоратор

![Decorator](https://hsto.org/getpro/habr/post_images/15c/27c/26e/15c27c26e08f1936e3f73089ecac3d05.jpg)

```javascript
class Person {
	constructor (name, family) {
		this.name = name
		this.family = family
	}

	greeting () {
		return `Всем привет. Я ${this.name} ${this.family}.`
	}
}

class Arab {
	constructor (person) {
		this.person = person
	}

	greeting () {
		return this.person.greeting().split('').reverse().join('')
	}
}

class Evil {
	constructor (person) {
		this.person = person
	}

	greeting () {
		return this.person.greeting().toUpperCase().replace(/\./g, '!')
	}
}

let person = new Person('Алексей', 'Данчин')
console.log(person.greeting()) // Всем привет. Я Алексей Данчин.

person = new Evil(person)
console.log(person.greeting()) // ВСЕМ ПРИВЕТ! Я АЛЕКСЕЙ ДАНЧИН!

person = new Arab(person)
console.log(person.greeting()) // !НИЧНАД ЙЕСКЕЛА Я !ТЕВИРП МЕСВ
```