### 1. Функции и стрелочные функции

#### Обычные функции:

Обычная функция объявляется с помощью ключевого слова `function`.

```js
function sum(a, b) {
    return a + b;
}
console.log(sum(3, 5)); // 8
```

#### Функции как переменные:

Функции можно присваивать переменным.

```js
const multiply = function(a, b) {
    return a * b;
};
console.log(multiply(3, 5)); // 15
```

#### Стрелочные функции:

Стрелочные функции записываются короче и не имеют своего `this`.

```js
const sum = (a, b) => a + b;
console.log(sum(3, 5)); // 8
```

Если у стрелочной функции один параметр, можно опустить скобки:

```js
const square = n => n * n;
console.log(square(4)); // 16
```

Если тело функции состоит из нескольких строк, требуется `return`:

```js
const difference = (a, b) => {
    const result = a - b;
    return result;
};
console.log(difference(10, 3)); // 7
```

### 2. Методы работы с массивами

#### `map` – преобразование массива:

Создает новый массив, применяя функцию ко всем элементам.

```js
const nums = [1, 2, 3, 4];
const doubled = nums.map(n => n * 2);
console.log(doubled); // [2, 4, 6, 8]
```

#### `filter` – фильтрация массива:

Создает новый массив, оставляя только элементы, удовлетворяющие условию.

```js
const nums = [1, 2, 3, 4, 5, 6];
const evens = nums.filter(n => n % 2 === 0);
console.log(evens); // [2, 4, 6]
```

#### `reduce` – сведение массива к одному значению:

Складывает все элементы массива.

```js
const nums = [1, 2, 3, 4];
const sum = nums.reduce((acc, n) => acc + n, 0);
console.log(sum); // 10
```

#### `forEach` – перебор массива:

Применяет функцию к каждому элементу массива.

```js
const nums = [1, 2, 3];
nums.forEach(n => console.log(n * 2));
// Выведет: 2, 4, 6
```

### 3. Работа с объектами

#### Деструктуризация объектов:

Позволяет извлекать свойства из объекта в переменные.

```js
const user = { name: "Alice", age: 25 };
const { name, age } = user;
console.log(name, age); // Alice 25
```

#### Добавление новых свойств в объект:

```js
const user = { name: "Alice", age: 25 };
user.city = "New York";
console.log(user);
// { name: "Alice", age: 25, city: "New York" }
```

### 4. Колбэки и асинхронность

#### Колбэк-функции:

Функция передается как аргумент другой функции и вызывается внутри неё.

```js
function greet(name, callback) {
    console.log(`Hello, ${name}`);
    callback();
}

greet("Alice", function() {
    console.log("Callback выполнен");
});
// Выведет:
// Hello, Alice
// Callback выполнен
```

#### `setTimeout` – задержка выполнения кода:

```js
setTimeout(() => console.log("Сообщение через 2 секунды"), 2000);
```

#### `setInterval` – выполнение кода через равные промежутки времени:

```js
setInterval(() => console.log("Прошла секунда"), 1000);
// Выведет "Прошла секунда" каждую секунду
```

### 5. Замыкания

Замыкание – это функция, которая запоминает переменные из своего внешнего окружения.

```js
function counter() {
    let count = 0;
    return function() {
        return count++;
    };
}

const increment = counter();
console.log(increment()); // 0
console.log(increment()); // 1
console.log(increment()); // 2
```

Этот код создаёт `increment`, который увеличивает `count`, но `count` остаётся недоступен извне.


Разбираем твои вопросы по порядку:

---

## **1. Function Expression (Функциональное выражение)**

Функциональное выражение — это объявление функции и присваивание её переменной.

🔹 **Пример:**

```js
const sayHello = function(name) {
    return `Привет, ${name}`;
};
console.log(sayHello("Нуртас")); // Привет, Нуртас
```

✅ Функцию можно передавать как аргумент и использовать анонимно.  
❌ В отличие от `function declaration`, её нельзя вызвать **до** объявления.

---

## **2. Callback (Колбэк-функция)**

Колбэк — это функция, переданная в другую функцию **как аргумент** и вызываемая внутри неё.

🔹 **Пример:**

```js
function processUser(name, callback) {
    console.log(`Обрабатываем пользователя: ${name}`);
    callback(); // Вызываем переданную функцию
}

processUser("Нуртас", function() {
    console.log("Завершено!");
});
// Обрабатываем пользователя: Нуртас
// Завершено!
```

✅ Колбэки используются в асинхронных операциях (таймеры, API-запросы).

---

## **3. find()**

Метод `find()` ищет **первый элемент** массива, который удовлетворяет условию.

🔹 **Пример:**

```js
const numbers = [10, 20, 30, 40];
const found = numbers.find(num => num > 25);
console.log(found); // 30 (Первый подходящий элемент)
```

✅ Возвращает **первый найденный элемент**.  
❌ Если элемент не найден, возвращает **undefined**.

---

## **4. findIndex()**

Метод `findIndex()` возвращает **индекс первого подходящего элемента**.

🔹 **Пример:**

```js
const numbers = [10, 20, 30, 40];
const index = numbers.findIndex(num => num > 25);
console.log(index); // 2 (индекс числа 30)
```

✅ Если ничего не найдено, возвращает **-1**.

---

## **5. findLastIndex()**

Метод `findLastIndex()` ищет **последний** подходящий элемент и возвращает его индекс.

🔹 **Пример:**

```js
const numbers = [10, 20, 30, 40, 30];
const lastIndex = numbers.findLastIndex(num => num > 25);
console.log(lastIndex); // 4 (индекс последней 30)
```

✅ Работает как `findIndex()`, но **справа налево**.

---

## **6. shift()**

Метод `shift()` удаляет **первый элемент** массива и возвращает его.

🔹 **Пример:**

```js
const queue = ["Alice", "Bob", "Charlie"];
const first = queue.shift();
console.log(first); // Alice
console.log(queue); // ["Bob", "Charlie"]
```

✅ Изменяет массив.  
❌ Если массив пустой, вернёт **undefined**.

---

## **7. unshift()**

Метод `unshift()` добавляет элементы **в начало массива** и возвращает **новую длину** массива.

🔹 **Пример:**

```js
const queue = ["Bob", "Charlie"];
const newLength = queue.unshift("Alice");
console.log(queue); // ["Alice", "Bob", "Charlie"]
console.log(newLength); // 3
```

✅ Изменяет массив.

---

## **8. slice()**

Метод `slice()` **копирует** часть массива и создаёт **новый массив**.

🔹 **Пример:**

```js
const numbers = [10, 20, 30, 40, 50];
const part = numbers.slice(1, 4); 
console.log(part); // [20, 30, 40]
console.log(numbers); // Исходный массив не изменился
```

✅ `slice(start, end)` копирует элементы от **start** до **end (не включая)**.  
✅ Не изменяет исходный массив.

---

### **Итог:**

- **Function Expression** – функция как переменная.
- **Callback** – передача функции как аргумента.
- **find()** – первый элемент, подходящий под условие.
- **findIndex()** – индекс первого подходящего элемента.
- **findLastIndex()** – индекс последнего подходящего элемента.
- **shift()** – удаляет первый элемент.
- **unshift()** – добавляет в начало.
- **slice()** – копирует часть массива без изменения оригинала.

🚀 Готов к quiz? 😎