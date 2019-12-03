[← Назад](/README.md "Вернуться на главную страницу")

# Strategy / Стратегия

![Strategy](https://hsto.org/getpro/habr/post_images/8d8/303/cdb/8d8303cdbc70de33f376454c2eb6934a.jpg)

```javascript
const ASC = (a, b) => a - b
const DESC = (a, b) => b - a
const ABSOLUTE_ASC = (a, b) => Math.abs(a) - Math.abs(b)
const ABSOLUTE_DESC = (a, b) => Math.abs(b) - Math.abs(a)

const numbers = (new Int16Array(10)).map(() => Math.floor(Math.random() * 100) - 50)

console.log(numbers) // Int16Array [ 29, 21, -5, -50, -22, 39, 22, 40, -39, 17 ]

console.log("ASC", numbers.sort(ASC)) // ASC Int16Array [ -50, -39, -22, -5, 17, 21, 22, 29, 39, 40 ]
console.log("DESC", numbers.sort(DESC)) // DESC Int16Array [ 40, 39, 29, 22, 21, 17, -5, -22, -39, -50 ]
console.log("ABSOLUTE_ASC", numbers.sort(ABSOLUTE_ASC)) // ABSOLUTE_ASC Int16Array [ -5, 17, 21, 22, -22, 29, 39, -39, 40, -50 ]
console.log("ABSOLUTE_DESC", numbers.sort(ABSOLUTE_DESC)) // ABSOLUTE_DESC Int16Array [ -50, 40, 39, -39, 29, 22, -22, 21, 17, -5 ]
```