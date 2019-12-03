[← Назад](/README.md "Вернуться на главную страницу")

# Interpreter / Интерпретатор

![Interpreter](https://hsto.org/getpro/habr/post_images/9f8/d89/5ce/9f8d895cef478a88a9c990802d733f22.jpg)

```javascript
class Interpreter {
	constructor () {
		this.expressions = []
	}

	register (expression) {
		this.expressions.push(expression)
	}

	run (str) {
		const context = {}
		str = str.trim()

		while (str.length) {
			for (const expression of this.expressions) {
				if (str.startsWith(expression.key)) {
					expression.handler(context)
					str = str.substr(expression.key.length).trim()
					break
				}
			}
		}
	}
}

class Expression {
	constructor (key, handler) {
		this.key = key
		this.handler = handler
	}
}

const interpreter = new Interpreter

interpreter.register(new Expression('init', context => context.number = 0))
interpreter.register(new Expression('inc', context => context.number++))
interpreter.register(new Expression('show', context => console.log(context.number)))

interpreter.run(
`
	init
	show
	inc
	inc
	show
`)

/*
0
2
*/
```