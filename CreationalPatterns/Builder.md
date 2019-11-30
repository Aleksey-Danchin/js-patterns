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