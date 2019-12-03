[← Назад](/README.md "Вернуться на главную страницу")

# Iterator / Итератор

![Iterator](https://hsto.org/getpro/habr/post_images/af1/992/b68/af1992b6809a68dd7743f8e7756ad635.jpg)

```javascript
class OwnArray extends Array {
	[Symbol.iterator] () {
		let i = this.length

		return {
			next: () => {
				return {
					value: this[--i],
					done: i === -1
				}
			}
		}
	}
}

const array = new OwnArray(0, 1, 2, 3, 4, 5)

console.log(array)

for (const item of array) {
	console.log(item)
}
```

```javascript
class Person {
	constructor (name, family) {
		this.name = name
		this.family = family
	}
}

class PersonsList {
	constructor () {
		this.persons = []
	}

	add (person) {
		if (!this.persons.includes(person)) {
			this.persons.push(person)
		}
	}
}

class Iterator {
	constructor (list) {
		this.list = list
	}

	forEach (handler) {
		for (const [index, person] of this.list.persons.entries()) {
			handler(person, index)
		}
	}
}

const list = new PersonsList

list.add(new Person('Алексей', 'Данчин'))
list.add(new Person('Серьгей', 'Лак'))
list.add(new Person('Мария', 'Чипсинка'))

const iterator = new Iterator(list)
iterator.forEach((person, index) => {
	console.log(person, index)
})
/*
Person { name: 'Алексей', family: 'Данчин' } 0
Person { name: 'Серьгей', family: 'Лак' } 1
Person { name: 'Мария', family: 'Чипсинка' } 2
*/
```