# Factory / Фактория / Фабрика

Порождающий паттерн. Процедура создания экземпляров класса выносится в отдельную функцию или в метод объекта.

```javascript
class Person {
    constructor (name, family, age) {
        this.name = name
        this.family = family
        this.age = age
    }
    
    static create (...args) {
        return new Person(...args)
    }
}

const factory = {
    create (...args) {
        return new Perosn(...args)
    }
}

// Прямой вызов конктруктора
const person1 = new Person("Aleksey", "Danchin", 27)

// Фабричный способ создание объекта
const person2 = Person.create("Aleksey", "Danchin", 27)
const person3 = factory.create("Aleksey", "Danchin", 27)
```

Зачастую используется для цепного стиля программирования без создания лишних переменных.

```javascript
Counter
    .create(0)
    .add(1)
    .log() // 1
    .add(1)
    .multi(5)
    .log() // 10
```

# Factory method / Фабричный метод

![Fatcory Method](https://hsto.org/getpro/habr/post_images/a79/c7d/d5e/a79c7dd5eaba210f19e194f2b97434d0.jpg)

Фабричный метод делегирует логику создания экземпляров дочерним классам.

```javascript
class Human {
    recover () {
        console.log('Теперь я здоров!')
    }
}

class Animal {
    recover () {
        console.log('Гав-гав! "Виляет хвостом"')
    }
}

class Healer {
    treatAPatient () {
        const patient = this.takePatient()
        patient.recover()
    }
    
    takePatient () {
        throw Error('Abstract method "takePatient" was called.')
    }
}

class Doctor extends Healer {
    takePatient () {
        return new Human
    }
}

class Veterinarian extends Healer {
    takePatient () {
        return new Animal
    }
}

const doctor = new Doctor
const veterinarian = new Veterinarian

doctor.treatAPatient() // Теперь я здоров!
veterinarian.treatAPatient() // Гав-гав! "Виляет хвостом"
```

Паттерн "Фабричный метод" используется если логика работы с разыми сущностями одинаковая, но тип сущности будет выявлен только во время работы кода.

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

# Builder / Строитель

![Builder](https://hsto.org/getpro/habr/post_images/16b/2fe/a7f/16b2fea7f7f4dcd14fe2ad0b0bb9bf84.jpg)

```javascript
class Airplane {
	constructor (length, widht, height, carryingCapacity, speed, capacity, type, autopilot, color, streamliningRatio) {
		this.length = length
		this.widht = widht
		this.height = height
		this.carryingCapacity = carryingCapacity
		this.speed = speed
		this.capacity = capacity
		this.type = type
		this.autopilot = autopilot
		this.color = color
		this.streamliningRatio = streamliningRatio
	}
}

class AirplaneBuilder {
	constructor () {
		this.length = 15
		this.widht = 7
		this.height = 5
		this.carryingCapacity = 100
		this.speed = 250
		this.capacity = 44
		this.type = 'passenger'
		this.autopilot = true
		this.color = 'white'
		this.streamliningRatio = 1.1
	}

	setLength (value) {
		this.length = value
		return this
	}

	setWidht (value) {
		this.widht = value
		return this
	}

	setHeight (value) {
		this.height = value
		return this
	}

	setCarryingCapacity (value) {
		this.carryingCapacity = value
		return this
	}

	setSpeed (value) {
		this.speed = value
		return this
	}

	setCapacity (value) {
		this.capacity = value
		return this
	}

	setType (value) {
		this.type = value
		return this
	}

	setAutopilot (value) {
		this.autopilot = value
		return this
	}

	setColor (value) {
		this.color = value
		return this
	}

	setStreamliningRatio (value) {
		this.streamliningRatio = value
		return this
	}

	getAirplane () {
		return new Airplane(
			this.length,
			this.widht,
			this.height,
			this.carryingCapacity,
			this.speed,
			this.capacity,
			this.type,
			this.autopilot,
			this.color,
			this.streamliningRatio
		)
	}
}

// const airplane = new Airplane(120, 35, 10, 230, 500, 12, 'passenger', true, 'black', 1.3)

const airplaneBuilder = new AirplaneBuilder

const airplane = airplaneBuilder
	.setLength(120)
	.setWidht(35)
	.setHeight(10)
	.setCarryingCapacity(230)
	.setSpeed(500)
	.setCapacity(12)
	.setType('passenger')
	.setAutopilot(true)
	.setColor('black')
	.setStreamliningRatio(1.3)
	.getAirplane()

console.log(airplane)
/*
Airplane {
  length: 120,
  widht: 35,
  height: 10,
  carryingCapacity: 230,
  speed: 500,
  capacity: 12,
  type: 'passenger',
  autopilot: true,
  color: 'black',
  streamliningRatio: 1.3 }
*/
```

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
