[← Назад](/README.md "Вернуться на главную страницу")

# Chain Of Responsibility / Цепочка Обязанностей

![Chain Of Responsibility](https://hsto.org/getpro/habr/post_images/ec8/3ef/362/ec83ef3621e0dbe56bed96e3c272e3ff.jpg)

```javascript
processing('Алексей') // Алексей handler fired
processing('Серьгей') // Серьгей handler fired
processing('Ольга') // Error

function processing (name) {
	step1(name)
}

function step1 (name) {
	if (name === 'Алексей') {
		console.log('Алексей handler fired')
	}

	else {
		step2(name)
	}
}

function step2 (name) {
	if (name === 'Дмитрий') {
		console.log('Дмитрий handler fired')
	}

	else {
		step3(name)
	}
}

function step3 (name) {
	if (name === 'Серьгей') {
		console.log('Серьгей handler fired')
	}

	else {
		console.log('Error')
	}
}
```

```javascript
class Middlewarer {
	constructor () {
		this.handlers = []
	}

	dispatch (arg) {
		for (const handler of this.handlers) {
			const result = handler(arg)

			if (result === false) {
				break
			}
		}
	}

	use (handler) {
		this.handlers.push(handler)
	}
}

const server = new Middlewarer

server.use(name => {
	if (name === "Алексей") {
		console.log("Алексей handler fired")
		return false
	}
})

server.use(name => {
	if (name === "Дмитрий") {
		console.log("Дмитрий handler fired")
		return false
	}
})

server.use(name => {
	if (name === "Серьгей") {
		console.log("Серьгей handler fired")
		return false
	}
})

server.use(name => {
	console.log("Error")
})

server.dispatch("Алексей") // Алексей handler fired
server.dispatch("Серьгей") // Серьгей handler fired
server.dispatch("Ольга") // Error
```

```javascript
class Country {
	constructor (label, cost, buff) {
		this.label = label
		this.cost = cost
		this.buff = buff
	}
}

class TravelAgency {
	constructor () {
		this.countries = []
	}

	add (country) {
		this.countries.push(country)
	}

	buyTicket (money) {
		for (const country of this.countries) {
			if (country.cost <= money && country.buff > 0) {
				country.buff--
				return country
			}
		}
	}
}

const ta = new TravelAgency

ta.add(new Country('Австралия', 15000, 3))
ta.add(new Country('Норвегия', 10000, 2))
ta.add(new Country('Польша', 18000, 4))

ta.buyTicket(15000) // Country { label: 'Австралия', cost: 15000, buff: 2 }
ta.buyTicket(11000) // Country { label: 'Норвегия', cost: 10000, buff: 1 }
```

```javascript
class Country {
	constructor (label, cost, buff) {
		this.label = label
		this.cost = cost
		this.buff = buff
		this.next = null
	}
}

class TravelAgency {
	constructor () {
		this.first = null
	}

	get last () {
		if (this.first === null) {
			return null
		}

		let current = this.first

		while (current.next !== null) {
			current = current.next
		}

		return current
	}

	add (country) {
		if (this.first === null) {
			this.first = country
		}

		else {
			this.last.next = country
		}
	}

	buyTicket (money) {
		let current = this.first

		while (current) {
			if (current.cost <= money && current.buff > 0) {
				current.buff--
				return current
			}

			current = current.next
		}
	}
}

const ta = new TravelAgency

ta.add(new Country('Австралия', 15000, 3))
ta.add(new Country('Норвегия', 10000, 2))
ta.add(new Country('Польша', 18000, 4))

console.log(ta.buyTicket(15000)) // Country { label: 'Австралия', cost: 15000, buff: 2 }
console.log(ta.buyTicket(11000)) // Country { label: 'Норвегия', cost: 10000, buff: 1 } 
```