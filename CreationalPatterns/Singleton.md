[← Назад](/README.md "Вернуться на главную страницу")

# Singleton / Одиночка

![Singleton](https://hsto.org/getpro/habr/post_images/f80/871/aaf/f80871aaf46238adcc0cd2468f19a4c5.jpg)


```javascript
// ES6
let instance = null

export default class Singleton {
	constructor () {
		if (instance) {
			return instance
		}

		instance = this
	}
}
```

```javascript
// Браузер
;(function () {
	let instance = null

	class Singleton {
		constructor () {
			if (instance) {
				return instance
			}

			instance = this
		}
	}

	window.Singleton = Singleton
})();
```