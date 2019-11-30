# Adapter / Адаптер

![Adapter](https://hsto.org/getpro/habr/post_images/4ae/931/8d8/4ae9318d8b986cf4efada94b9de54761.jpg)

```javascript
class Database {
	constructor () {
		this.users = []
	}

	static create (...args) {
		return new Database(...args)
	}

	saveNewtUserData (user) {
		this.users.push(
			JSON.parse(JSON.stringify(user))
		)
	}

	findOneUserByOwnId (userId) {
		for (const user of this.users) {
			if (user.id === userId) {
				return user
			}
		}
	}
}

class Adapter {
	constructor (db) {
		this.db = db
	}

	add (user) {
		this.db.saveNewtUserData(user)
	}

	find (userId) {
		return this.db.findOneUserByOwnId(userId)
	}
}

const db = new Adapter(Database.create())

db.add({
	id: 1,
	name: "Алексей",
	family: "Данчин"
})

console.log(
	db.find(1) // { id: 1, name: 'Алексей', family: 'Данчин' }
)
```

---

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

---

# Composite / Компоновщик

![Composite](https://hsto.org/getpro/habr/post_images/3c0/75e/525/3c075e525a7ed4aa2f13495a3289e348.jpg)

```javascript
class Good {
	constructor (label, price) {
		this.label = label
		this.price = price
	}
}

class Composite {
	constructor (label) {
		this.label = label

		this.items = []
	}

	add (item) {
		this.items.push(item)
	}

	get price () {
		return this.items.reduce((p, e) => p + e.price, 0)
	}
}

const products = new Composite('Продукты')

products.add(new Good('Яблоко', 12))
products.add(new Good('Малина', 20))
products.add(new Good('Молоко', 24))

console.log(`Общая цена категории "${products.label}" составила ${products.price}"`)

const basket = new Composite('Карзина')
basket.add(products)
basket.add(new Good('Машинное масло', 100))

console.log(`Общая цена категории "${basket.label}" составила ${basket.price}"`)
```

---

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

---

# Facade / Фасад

![Facade](https://hsto.org/getpro/habr/post_images/049/2df/3bf/0492df3bf1fc55c520276c618815298a.jpg)

```javascript
class Good {
	constructor (label, price) {
		this.label = label
		this.price = price
	}
}

class Box {
	constructor (address) {
		this.address = address
		this.items = []
	}

	addItem (item) {
		if (!this.items.includes(item)) {
			this.items.push(item)
		}
	}
}

class Order {
	constructor () {
		this.id = ++Order.idCounter

		this.boxes = []
	}

	addBox (box) {
		if (!this.boxes.includes(box)) {
			this.boxes.push(box)
		}
	}

	get price () {
		let price = 0

		for (const box of this.boxes) {
			for (const item of box.items) {
				price += item.price
			}
		}

		return price
	}
}

Order.idCounter = 0

class Notification {
	constructor (user, order) {
		this.user = user
		this.order = order
	}

	send () {
		console.log(`Письмо на почту: ${this.user.name} ${this.user.family}, ваш заказ был отправлен вам почтой по адресу ${this.user.address}. Вы можете отслеживать его id ${this.order.id}. Общая стоимость ${this.order.price}`)

		for (let i = 0; i < this.order.boxes.length; i++) {
			const box = this.order.boxes[i]

			console.log(`\n${i + 1} коробка:`)

			for (const good of box.items) {
				console.log(`${good.label}: ${good.label.length * 1.5}`)
			}
		}
	}
}

class Amazon {
	constructor (user) {
		this.user = user
		this.goods = []
		this.order = null
		this.notification = null
	}

	add (label) {
		this.goods.push(new Good(label, label.length * 1.5))
	}

	buy () {
		if (this.order) {
			return false
		}

		this.order = new Order

		const goods = this.goods.slice()
		while (goods.length) {
			const box = new Box(this.user.address)

			goods.splice(-3).forEach(
				x => box.addItem(x)
			)

			this.order.addBox(box)
		}

		this.notification = new Notification(this.user, this.order)
		this.notification.send()

		return true
	}
}

const webshop = new Amazon({
	name: 'Алексей',
	family: 'Данчин',
	address: 'Москва, переулок Чукчи.'
})

webshop.add('Маска')
webshop.add('Книга')
webshop.add('Тетрадка')
webshop.add('Масло')

webshop.buy()
/*
Письмо на почту: Алексей Данчин, ваш заказ был отправлен вам почтой по адресу Москва, переулок Чукчи.. Вы можете отслеживать его id 1. Общая стоимость 34.5
1 коробка:Книга: 7.5
Тетрадка: 12
Масло: 7.5

2 коробка:
Маска: 7.5
*/
```

---

# Flyweight / Приспособленец / Легковес

![Flyweight](https://hsto.org/getpro/habr/post_images/120/481/32c/12048132c537c1ad342f3fe17647de0d.jpg)

```javascript
class Apple {
	constructor (type, color, weight) {
		this.type = type
		this.color = color
		this.weight = weight
	}
}

const apples = []
for (let i = 0; i < 10**6; i++) {
	apples.push(
		new Apple('Ginger gold', 'yellow', parseInt(Math.random() * 400))
	)
}

/* НО! */
class Apple {
	constructor (type, color, weight) {
		this.type = type
		this.color = color
		this.weight = weight
	}
}

class FlyweightFactory {
	constructor (...categories) {
		this.categories = categories
		this.items = []
	}

	add (...params) {
		this.items.push(params.map((v, i) => this.categories[i].indexOf(v)))
	}

	get apples () {
		const instance = this

		return (function * () {
			for (const item of instance.items) {
				yield new Apple(
					...item.map((v, i) => instance.categories[i][v])
				)
			}
		})()
	}
}

const appleFactory = new FlyweightFactory(
	["Абориген", "Антоновка", "Голден", "Кребы", "Ренет", "Чара"],
	["зеленый", "красный", "желтый", "белый"],
	[100, 200, 300, 400]
)

appleFactory.add("Абориген", "зеленый", 200)
appleFactory.add("Голден", "желтый", 100)

console.log(appleFactory)
/*
FlyweightFactory {
  categories:
   [ [ 'Абориген', 'Антоновка', 'Голден', 'Кребы', 'Ренет', 'Чара' ],
     [ 'зеленый', 'красный', 'желтый', 'белый' ],
     [ 100, 200, 300, 400 ] ],
  items: [ [ 0, 0, 1 ], [ 2, 2, 0 ] ] }
*/

for (const apple of appleFactory.apples) {
	console.log(apple)
}
/*
Apple { type: 'Абориген', color: 'зеленый', weight: 200 }
Apple { type: 'Голден', color: 'желтый', weight: 100 }
*/
```

---

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


# Adapter / Адаптер

![Adapter](https://hsto.org/getpro/habr/post_images/4ae/931/8d8/4ae9318d8b986cf4efada94b9de54761.jpg)

```javascript
class Database {
	constructor () {
		this.users = []
	}

	static create (...args) {
		return new Database(...args)
	}

	saveNewtUserData (user) {
		this.users.push(
			JSON.parse(JSON.stringify(user))
		)
	}

	findOneUserByOwnId (userId) {
		for (const user of this.users) {
			if (user.id === userId) {
				return user
			}
		}
	}
}

class Adapter {
	constructor (db) {
		this.db = db
	}

	add (user) {
		this.db.saveNewtUserData(user)
	}

	find (userId) {
		return this.db.findOneUserByOwnId(userId)
	}
}

const db = new Adapter(Database.create())

db.add({
	id: 1,
	name: "Алексей",
	family: "Данчин"
})

console.log(
	db.find(1) // { id: 1, name: 'Алексей', family: 'Данчин' }
)
```

---

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

---

# Composite / Компоновщик

![Composite](https://hsto.org/getpro/habr/post_images/3c0/75e/525/3c075e525a7ed4aa2f13495a3289e348.jpg)

```javascript
class Good {
	constructor (label, price) {
		this.label = label
		this.price = price
	}
}

class Composite {
	constructor (label) {
		this.label = label

		this.items = []
	}

	add (item) {
		this.items.push(item)
	}

	get price () {
		return this.items.reduce((p, e) => p + e.price, 0)
	}
}

const products = new Composite('Продукты')

products.add(new Good('Яблоко', 12))
products.add(new Good('Малина', 20))
products.add(new Good('Молоко', 24))

console.log(`Общая цена категории "${products.label}" составила ${products.price}"`)

const basket = new Composite('Карзина')
basket.add(products)
basket.add(new Good('Машинное масло', 100))

console.log(`Общая цена категории "${basket.label}" составила ${basket.price}"`)
```

---

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

---

# Facade / Фасад

![Facade](https://hsto.org/getpro/habr/post_images/049/2df/3bf/0492df3bf1fc55c520276c618815298a.jpg)

```javascript
class Good {
	constructor (label, price) {
		this.label = label
		this.price = price
	}
}

class Box {
	constructor (address) {
		this.address = address
		this.items = []
	}

	addItem (item) {
		if (!this.items.includes(item)) {
			this.items.push(item)
		}
	}
}

class Order {
	constructor () {
		this.id = ++Order.idCounter

		this.boxes = []
	}

	addBox (box) {
		if (!this.boxes.includes(box)) {
			this.boxes.push(box)
		}
	}

	get price () {
		let price = 0

		for (const box of this.boxes) {
			for (const item of box.items) {
				price += item.price
			}
		}

		return price
	}
}

Order.idCounter = 0

class Notification {
	constructor (user, order) {
		this.user = user
		this.order = order
	}

	send () {
		console.log(`Письмо на почту: ${this.user.name} ${this.user.family}, ваш заказ был отправлен вам почтой по адресу ${this.user.address}. Вы можете отслеживать его id ${this.order.id}. Общая стоимость ${this.order.price}`)

		for (let i = 0; i < this.order.boxes.length; i++) {
			const box = this.order.boxes[i]

			console.log(`\n${i + 1} коробка:`)

			for (const good of box.items) {
				console.log(`${good.label}: ${good.label.length * 1.5}`)
			}
		}
	}
}

class Amazon {
	constructor (user) {
		this.user = user
		this.goods = []
		this.order = null
		this.notification = null
	}

	add (label) {
		this.goods.push(new Good(label, label.length * 1.5))
	}

	buy () {
		if (this.order) {
			return false
		}

		this.order = new Order

		const goods = this.goods.slice()
		while (goods.length) {
			const box = new Box(this.user.address)

			goods.splice(-3).forEach(
				x => box.addItem(x)
			)

			this.order.addBox(box)
		}

		this.notification = new Notification(this.user, this.order)
		this.notification.send()

		return true
	}
}

const webshop = new Amazon({
	name: 'Алексей',
	family: 'Данчин',
	address: 'Москва, переулок Чукчи.'
})

webshop.add('Маска')
webshop.add('Книга')
webshop.add('Тетрадка')
webshop.add('Масло')

webshop.buy()
/*
Письмо на почту: Алексей Данчин, ваш заказ был отправлен вам почтой по адресу Москва, переулок Чукчи.. Вы можете отслеживать его id 1. Общая стоимость 34.5
1 коробка:Книга: 7.5
Тетрадка: 12
Масло: 7.5

2 коробка:
Маска: 7.5
*/
```

---

# Flyweight / Приспособленец / Легковес

![Flyweight](https://hsto.org/getpro/habr/post_images/120/481/32c/12048132c537c1ad342f3fe17647de0d.jpg)

```javascript
class Apple {
	constructor (type, color, weight) {
		this.type = type
		this.color = color
		this.weight = weight
	}
}

const apples = []
for (let i = 0; i < 10**6; i++) {
	apples.push(
		new Apple('Ginger gold', 'yellow', parseInt(Math.random() * 400))
	)
}

/* НО! */
class Apple {
	constructor (type, color, weight) {
		this.type = type
		this.color = color
		this.weight = weight
	}
}

class FlyweightFactory {
	constructor (...categories) {
		this.categories = categories
		this.items = []
	}

	add (...params) {
		this.items.push(params.map((v, i) => this.categories[i].indexOf(v)))
	}

	get apples () {
		const instance = this

		return (function * () {
			for (const item of instance.items) {
				yield new Apple(
					...item.map((v, i) => instance.categories[i][v])
				)
			}
		})()
	}
}

const appleFactory = new FlyweightFactory(
	["Абориген", "Антоновка", "Голден", "Кребы", "Ренет", "Чара"],
	["зеленый", "красный", "желтый", "белый"],
	[100, 200, 300, 400]
)

appleFactory.add("Абориген", "зеленый", 200)
appleFactory.add("Голден", "желтый", 100)

console.log(appleFactory)
/*
FlyweightFactory {
  categories:
   [ [ 'Абориген', 'Антоновка', 'Голден', 'Кребы', 'Ренет', 'Чара' ],
     [ 'зеленый', 'красный', 'желтый', 'белый' ],
     [ 100, 200, 300, 400 ] ],
  items: [ [ 0, 0, 1 ], [ 2, 2, 0 ] ] }
*/

for (const apple of appleFactory.apples) {
	console.log(apple)
}
/*
Apple { type: 'Абориген', color: 'зеленый', weight: 200 }
Apple { type: 'Голден', color: 'желтый', weight: 100 }
*/
```

---

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