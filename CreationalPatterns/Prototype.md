# Prototype / Прототип

![Prototype](https://hsto.org/getpro/habr/post_images/a9d/715/4a9/a9d7154a9b7e321a6330ab0c0337c061.jpg)

```javascript
class Manager {
	constructor () {
		this.counter = 0
	}

	add (n = 0) {
		this.counter += n
	}

	clone () {
		const copy = new Manager
		copy.counter = this.counter
		return copy
	}
}

const manager1 = new Manager

manager1.add(100500)
console.log(manager1)

const manager2 = manager1.clone()
console.log(manager2)
```