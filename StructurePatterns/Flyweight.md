[← Назад](/README.md "Вернуться на главную страницу")

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