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