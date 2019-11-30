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