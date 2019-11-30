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