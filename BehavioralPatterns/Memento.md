[← Назад](/README.md "Вернуться на главную страницу")

# Memento / Хранитель / Снимок

![Memento](https://hsto.org/getpro/habr/post_images/c08/bf1/7ee/c08bf17ee80d42272441cafbcce1a2dd.jpg)

```javascript
class Memento {
	constructor () {
		this.history = {}
	}

	getSum () {
		const key = JSON.stringify(arguments)

		if (this.history[key]) {
			return this.history[key]
		}

		return this.history[key] = [].reduce.call(arguments, (a, b) => a + b)
	}
}

const memento = new Memento
const array = (new Int16Array(1000)).map((e, i) => i + 1)

console.time('First')
memento.getSum(...array)
console.timeEnd('First')

console.time('Second')
memento.getSum(...array)
console.timeEnd('Second')

/*
First: 1.406ms
Second: 0.490ms
*/
```