[← Назад](/README.md "Вернуться на главную страницу")

# Abstract Factory / Абстрактная фабрика

![Abstract Factory](https://hsto.org/getpro/habr/post_images/710/505/d1a/710505d1aff5667c97fcb06215faee31.jpg)

```javascript
class Candy {}
class Soda {}
class Barbecue {}
class Wine {}

class ChildrensHolidayFactory {
	makeFood () {
		return Candy
	}

	makeDrink () {
		return Soda
	}
}

class AdultsHolidayFactory {
	makeFood () {
		return Barbecue
	}

	makeDrink () {
		return Wine
	}
}

function makeHolidayFun (food, drink) {}

{
	// Детский праздник
	const factory = new ChildrensHolidayFactory

	const food = factory.makeFood()
	const drink = factory.makeDrink()

	makeHolidayFun(food, drink)
}


{
	// Детский праздник
	const factory = new AdultsHolidayFactory

	const food = factory.makeFood()
	const drink = factory.makeDrink()

	makeHolidayFun(food, drink)
}
```