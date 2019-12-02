[← Назад](/README.md "Вернуться на главную страницу")

# Bridge / Мост

![Bridge](https://hsto.org/getpro/habr/post_images/a37/91c/32c/a3791c32c219678bc6549b012747497d.jpg)

```javascript
class Person {
	constructor (name, family) {
		this.name = name
		this.family = family

		this.speaker = new Speaker(this)
		this.walker = new Walker(this)
	}
}

class Speaker {
	constructor (person) {
		this.person = person
	}

	greeting () {
		console.log(`${this.person.name} приветствует вас!`)
	}
}

class Walker {
	constructor (person) {
		this.person = person
	}

	run () {
		console.log(`${this.person.name} побежал за мороженым!`)
	}
}

const person = new Person('Алексей', 'Данчин')

person.walker.run()
person.speaker.greeting()
```