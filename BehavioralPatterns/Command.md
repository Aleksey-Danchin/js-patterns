[← Назад](/README.md "Вернуться на главную страницу")

# Command / Команда

![Command](https://hsto.org/getpro/habr/post_images/0b9/09b/8fb/0b909b8fbe9fa7d4c02225c2004cb126.jpg)

```javascript
const ADD_PERSON = 0b00
const GET_PERSON = 0b01
const REMOVE_PERSON = 0b10
const CLEAR_LIST = 0b11

class Person {
	constructor (name, family) {
		this.id = Person.idCounter++
		this.name = name
		this.family = family
	}
}

Person.idCounter = 1

class System {
	constructor () {
		this.persons = []
	}

	dispatch (data) {
		if (data.code === ADD_PERSON) {
			this.persons.push(new Person(data.name, data.family))
			return true
		}

		if (data.code === GET_PERSON) {
			for (const person of this.persons) {
				if (person.id === data.id) {
					return person
				}
			}
		}

		if (data.code === REMOVE_PERSON) {
			this.persons = this.persons.filter(x => x.id !== data.id)
			return true
		}

		if (data.code === CLEAR_LIST) {
			this.persons = []
			return true
		}
	}
}

const system = new System

const command1 = {
	code: ADD_PERSON,
	name: 'Алексей',
	family: 'Данчин'
}

system.dispatch(command1)

console.log(system.persons) // [ Person { id: 1, name: 'Алексей', family: 'Данчин' } ]

const person = system.dispatch({
	code: GET_PERSON,
	id: 1
})

console.log(person) // Person { id: 1, name: 'Алексей', family: 'Данчин' }

system.dispatch({
	code: CLEAR_LIST
})

console.log(system.persons) // []
```