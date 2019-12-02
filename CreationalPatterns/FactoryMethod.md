[← Назад](/README.md "Вернуться на главную страницу")

# Factory Method / Фабричный метод

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