[← Назад](/README.md "Вернуться на главную страницу")

# Visitor / Посетитель

![Visitor](https://hsto.org/getpro/habr/post_images/ecb/a77/f17/ecba77f1762c4e96b5b468810bf9edb0.jpg)

```javascript
class Visitorable {
	accept (actions) {
		if (actions[this.type]) {
			actions[this.type](this)
		}
	}
}

class Apple extends Visitorable {
	constructor () {
		super()

		this.type = 'apple'
	}

	eat () {
		return 'Хрусь'
	}
}

class Watermelon extends Visitorable {
	constructor () {
		super()

		this.type = 'watermelon'
	}

	eat () {
		return 'Много косточек'
	}
}

const eating = {
	apple (apple) {
		console.log(apple.eat())
	},

	watermelon (watermelon) {
		console.log(watermelon.eat())
	}
}

const apple = new Apple
const watermelon = new Watermelon

apple.accept(eating)
watermelon.accept(eating)
```