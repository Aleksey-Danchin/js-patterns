[← Назад](/README.md "Вернуться на главную страницу")

# State / Состояние

![State](https://hsto.org/getpro/habr/post_images/4fb/72c/31e/4fb72c31e718d89f6cefb97af767b6ee.jpg)

```javascript
class Person {
	constructor (name, family, age) {
		this.name = name
		this.family = family
		this.age = age

		this.state = 'normal'
	}

	greeting () {
		if (this.state === 'normal') {
			console.log(`Добрый день. Я ${this.name} ${this.family}.`)
		}

		else if (this.state === 'angry') {
			console.log(`ЧТО ${this.name.toUpperCase()}?! Я УЖЕ ${this.age} ${this.name.toUpperCase()}!`)
		}
	}
}

const person = new Person('Алексей', 'Данчин', 27)

person.greeting() // Добрый день. Я Алексей Данчин.

person.state = 'angry'

person.greeting() // ЧТО АЛЕКСЕЙ?! Я УЖЕ 27 АЛЕКСЕЙ!
```