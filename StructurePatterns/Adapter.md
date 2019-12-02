[← Назад](/README.md "Вернуться на главную страницу")

![Adapter](https://hsto.org/getpro/habr/post_images/4ae/931/8d8/4ae9318d8b986cf4efada94b9de54761.jpg)

```javascript
class Database {
	constructor () {
		this.users = []
	}

	static create (...args) {
		return new Database(...args)
	}

	saveNewUserData (user) {
		this.users.push(
			JSON.parse(JSON.stringify(user))
		)
	}

	findOneUserByOwnId (userId) {
		for (const user of this.users) {
			if (user.id === userId) {
				return user
			}
		}
	}
}

class Adapter {
	constructor (db) {
		this.db = db
	}

	add (user) {
		this.db.saveNewUserData(user)
	}

	find (userId) {
		return this.db.findOneUserByOwnId(userId)
	}
}

const db = new Adapter(Database.create())

db.add({
	id: 1,
	name: "Алексей",
	family: "Данчин"
})

console.log(
	db.find(1) // { id: 1, name: 'Алексей', family: 'Данчин' }
)
```