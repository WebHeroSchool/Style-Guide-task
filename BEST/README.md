![js](https://i.ytimg.com/vi/HVFhMSbj6RQ/maxresdefault.jpg)
___
#  `JavaScript - 10 Best Practice Guid`
___

## 1. Объявление переменных
### Используйте `const` для объявления переменных; избегайте `var`
> Почему? Это гарантирует, что вы не сможете переопределять значения, т.к. это может привести к ошибкам и к усложнению понимания кода.

    ``` js
    // плохо
    var a = 1;
    var b = 2;

    // хорошо
    const a = 1;
    const b = 2;
    ```
### Если вам необходимо переопределять значения, то используйте `let` вместо `var`
> Почему? Область видимости `let` — блок, у `var` — функция.

    ``` js
    // плохо
    var count = 1;
    if (true) {
      count += 1;
    }

    // хорошо, используйте let.
    let count = 1;
    if (true) {
      count += 1;
    }
    ```
### Помните, что у `let` и `const` блочная область видимости.
    ``` js
    // const и let существуют только в том блоке, в котором они определены.
    {
      let a = 1;
      const b = 1;
    }
    console.log(a); // ReferenceError
    console.log(b); // ReferenceError
    ```
___
## 2.Объекты
### Для создания объекта используйте литеральную нотацию.

    ```js
    // плохо
    const item = new Object();

    // хорошо
    const item = {};
    ```


### Используйте вычисляемые имена свойств, когда создаёте объекты с динамическими именами свойств.
> Почему? Они позволяют вам определить все свойства объекта в одном месте.

    ```js

    function getKey(k) {
      return `a key named ${k}`;
    }

    // плохо
    const obj = {
      id: 5,
      name: 'San Francisco',
    };
    obj[getKey('enabled')] = true;

    // хорошо
    const obj = {
      id: 5,
      name: 'San Francisco',
      [getKey('enabled')]: true,
    };
    ```
### Используйте сокращённую запись метода объекта.
    ```js
    // плохо
    const atom = {
      value: 1,

      addValue: function (value) {
        return atom.value + value;
      },
    };

    // хорошо
    const atom = {
      value: 1,

      addValue(value) {
        return atom.value + value;
      },
    };
    ```

### Используйте сокращённую запись свойств объекта. 
> Почему? Это короче и понятнее.

    ```js
    const lukeSkywalker = 'Luke Skywalker';

    // плохо
    const obj = {
      lukeSkywalker: lukeSkywalker,
    };

    // хорошо
    const obj = {
      lukeSkywalker,
    };
    ```


### Группируйте ваши сокращённые записи свойств в начале объявления объекта.
 > Почему? Так легче сказать, какие свойства используют сокращённую запись.

    ```js
    const anakinSkywalker = 'Anakin Skywalker';
    const lukeSkywalker = 'Luke Skywalker';

    // плохо
    const obj = {
      episodeOne: 1,
      twoJediWalkIntoACantina: 2,
      lukeSkywalker,
      episodeThree: 3,
      mayTheFourth: 4,
      anakinSkywalker,
    };

    // хорошо
    const obj = {
      lukeSkywalker,
      anakinSkywalker,
      episodeOne: 1,
      twoJediWalkIntoACantina: 2,
      episodeThree: 3,
      mayTheFourth: 4,
    };
    ```
### Только недопустимые идентификаторы помещаются в кавычки. 
> Почему? На наш взгляд, такой код легче читать. Это улучшает подсветку синтаксиса, а также облегчает оптимизацию для многих JS-движков.

    ```js
    // плохо
    const bad = {
      'foo': 3,
      'bar': 4,
      'data-blah': 5,
    };

    // хорошо
    const good = {
      foo: 3,
      bar: 4,
      'data-blah': 5,
    };
    ```
## Не вызывайте напрямую методы `Object.prototype`, такие как `hasOwnProperty`, `propertyIsEnumerable`, и `isPrototypeOf`.
> Почему? Эти методы могут быть переопределены в свойствах объекта, который мы проверяем `{ hasOwnProperty: false }`, или этот объект может быть `null` (`Object.create(null)`).

    ```js
    // плохо
    console.log(object.hasOwnProperty(key));

    // хорошо
    console.log(Object.prototype.hasOwnProperty.call(object, key));

    // отлично
    const has = Object.prototype.hasOwnProperty; // Кэшируем запрос в рамках модуля.
    console.log(has.call(object, key));
    /* или */
    import has from 'has'; // https://www.npmjs.com/package/has
    console.log(has(object, key));
    ```

### Используйте оператор расширения вместо [`Object.assign`]

    ```js
    // очень плохо
    const original = { a: 1, b: 2 };
    const copy = Object.assign(original, { c: 3 }); // эта переменная изменяет `original` ಠ_ಠ
    delete copy.a; // если сделать так

    // плохо
    const original = { a: 1, b: 2 };
    const copy = Object.assign({}, original, { c: 3 }); // copy => { a: 1, b: 2, c: 3 }

    // хорошо
    const original = { a: 1, b: 2 };
    const copy = { ...original, c: 3 }; // copy => { a: 1, b: 2, c: 3 }

    const { a, ...noA } = copy; // noA => { b: 2, c: 3 }
    ```
___

## 3.Массивы
### Для создания массива используйте литеральную нотацию.

    ```js
    // плохо
    const items = new Array();

    // хорошо
    const items = [];
    ```

### Для добавления элемента в массив используйте [Array#push]

    ```js
    const someStack = [];

    // плохо
    someStack[someStack.length] = 'abracadabra';

    // хорошо
    someStack.push('abracadabra');
    ```

### Для копирования массивов используйте оператор расширения `...`.

    ```js
    // плохо
    const len = items.length;
    const itemsCopy = [];
    let i;

    for (i = 0; i < len; i += 1) {
      itemsCopy[i] = items[i];
    }

    // хорошо
    const itemsCopy = [...items];
    ```

### Для преобразования итерируемого объекта в массив используйте оператор расширения `...` вместо [`Array.from`]

    ```js
    const foo = document.querySelectorAll('.foo');

    // хорошо
    const nodes = Array.from(foo);

    // отлично
    const nodes = [...foo];
    ```

### Используйте [`Array.from`] для преобразования массивоподобного объекта в массив.

    ```js
    const arrLike = { 0: 'foo', 1: 'bar', 2: 'baz', length: 3 };

    // плохо
    const arr = Array.prototype.slice.call(arrLike);

    // хорошо
    const arr = Array.from(arrLike);
    ```

### Используйте [`Array.from`] вместо оператора расширения `...` для маппинга итерируемых объектов, это позволяет избежать создания промежуточного массива.

    ```js
    // плохо
    const baz = [...foo].map(bar);

    // хорошо
    const baz = Array.from(foo, bar);
    ```

### Используйте операторы `return` внутри функций обратного вызова в методах массива. Можно опустить `return`, когда тело функции состоит из одной инструкции, возвращающей выражение без побочных эффектов.

    ```js
    // хорошо
    [1, 2, 3].map((x) => {
      const y = x + 1;
      return x * y;
    });

    // хорошо
    [1, 2, 3].map((x) => x + 1);

    // плохо - нет возвращаемого значения, следовательно, `acc` становится `undefined` после первой итерации
    [[0, 1], [2, 3], [4, 5]].reduce((acc, item, index) => {
      const flatten = acc.concat(item);
    });

    // хорошо
    [[0, 1], [2, 3], [4, 5]].reduce((acc, item, index) => {
      const flatten = acc.concat(item);
      return flatten;
    });

    // плохо
    inbox.filter((msg) => {
      const { subject, author } = msg;
      if (subject === 'Mockingbird') {
        return author === 'Harper Lee';
      } else {
        return false;
      }
    });

    // хорошо
    inbox.filter((msg) => {
      const { subject, author } = msg;
      if (subject === 'Mockingbird') {
        return author === 'Harper Lee';
      }

      return false;
    });
    ```

### Если массив располагается на нескольких строках, то используйте разрывы строк после открытия и перед закрытием скобок.

    ```js
    // плохо
    const arr = [
      [0, 1], [2, 3], [4, 5],
    ];

    const objectInArray = [{
      id: 1,
    }, {
      id: 2,
    }];

    const numberInArray = [
      1, 2,
    ];

    // хорошо
    const arr = [[0, 1], [2, 3], [4, 5]];

    const objectInArray = [
      {
        id: 1,
      },
      {
        id: 2,
      },
    ];

    const numberInArray = [
      1,
      2,
    ];
    ```
___
## 4. Деструктуризация

### При обращении к нескольким свойствам объекта используйте деструктуризацию объекта. 
> Почему? Деструктуризация избавляет вас от создания временных переменных для этих свойств.

    ```js
    // плохо
    function getFullName(user) {
      const firstName = user.firstName;
      const lastName = user.lastName;

      return `${firstName} ${lastName}`;
    }

    // хорошо
    function getFullName(user) {
      const { firstName, lastName } = user;
      return `${firstName} ${lastName}`;
    }

    // отлично
    function getFullName({ firstName, lastName }) {
      return `${firstName} ${lastName}`;
    }
    ```

### Используйте деструктуризацию массивов. 

    ```js
    const arr = [1, 2, 3, 4];

    // плохо
    const first = arr[0];
    const second = arr[1];

    // хорошо
    const [first, second] = arr;
    ```

### Используйте деструктуризацию объекта для множества возвращаемых значений, но не делайте тоже самое с массивами.
> Почему? Вы сможете добавить новые свойства через некоторое время или изменить порядок без последствий.

    ```js
    // плохо
    function processInput(input) {
      // затем происходит чудо
      return [left, right, top, bottom];
    }

    // при вызове нужно подумать о порядке возвращаемых данных
    const [left, __, top] = processInput(input);

    // хорошо
    function processInput(input) {
      // затем происходит чудо
      return { left, right, top, bottom };
    }

    // при вызове выбираем только необходимые данные
    const { left, top } = processInput(input);
    ```
___
## 5. Стрелочные функции
### Когда вам необходимо использовать анонимную функцию (например, при передаче встроенной функции обратного вызова), используйте стрелочную функцию. 
> Почему? Таким образом создаётся функция, которая выполняется в контексте `this`, который мы обычно хотим, а также это более короткий синтаксис.
> Почему бы и нет? Если у вас есть довольно сложная функция, вы можете переместить эту логику внутрь её собственного именованного функционального выражения.

    ```js
    // плохо
    [1, 2, 3].map(function (x) {
      const y = x + 1;
      return x * y;
    });

    // хорошо
    [1, 2, 3].map((x) => {
      const y = x + 1;
      return x * y;
    });
    ```

### Если тело функции состоит из одного оператора, возвращающего [выражение] без побочных эффектов, то опустите фигурные скобки и используйте неявное возвращение. В противном случае, сохраните фигурные скобки и используйте оператор `return`. 
> Почему? Синтаксический сахар. Когда несколько функций соединены вместе, то это лучше читается.

    ```js
    // плохо
    [1, 2, 3].map((number) => {
      const nextNumber = number + 1;
      `A string containing the ${nextNumber}.`;
    });

    // хорошо
    [1, 2, 3].map((number) => `A string containing the ${number + 1}.`);

    // хорошо
    [1, 2, 3].map((number) => {
      const nextNumber = number + 1;
      return `A string containing the ${nextNumber}.`;
    });

    // хорошо
    [1, 2, 3].map((number, index) => ({
      [index]: number,
    }));

    // Неявный возврат с побочными эффектами
    function foo(callback) {
      const val = callback();
      if (val === true) {
        // Сделать что-то, если функция обратного вызова вернёт true
      }
    }

    let bool = false;

    // плохо
    foo(() => bool = true);

    // хорошо
    foo(() => {
      bool = true;
    });
    ```
### В случае, если выражение располагается на нескольких строках, то необходимо обернуть его в скобки для лучшей читаемости.
> Почему? Это чётко показывает, где функция начинается и где заканчивается.

    ```js
    // плохо
    ['get', 'post', 'put'].map((httpMethod) => Object.prototype.hasOwnProperty.call(
        httpMagicObjectWithAVeryLongName,
        httpMethod,
      )
    );

    // хорошо
    ['get', 'post', 'put'].map((httpMethod) => (
      Object.prototype.hasOwnProperty.call(
        httpMagicObjectWithAVeryLongName,
        httpMethod,
      )
    ));
    ```
### Всегда оборачивайте аргументы круглыми скобками для ясности и согласованности.
> Почему? Минимизирует различия при удалении или добавлении аргументов.

    ```js
    // плохо
    [1, 2, 3].map(x => x * x);

    // хорошо
    [1, 2, 3].map((x) => x * x);

    // плохо
    [1, 2, 3].map(number => (
      `A long string with the ${number}. It’s so long that we don’t want it to take up space on the .map line!`
    ));

    // хорошо
    [1, 2, 3].map((number) => (
      `A long string with the ${number}. It’s so long that we don’t want it to take up space on the .map line!`
    ));

    // плохо
    [1, 2, 3].map(x => {
      const y = x + 1;
      return x * y;
    });

    // хорошо
    [1, 2, 3].map((x) => {
      const y = x + 1;
      return x * y;
    });
    ```
### Избегайте схожести стрелочной функции (`=>`) с операторами сравнения (`<=`, `>=`). 

    ```js
    // плохо
    const itemHeight = (item) => item.height <= 256 ? item.largeSize : item.smallSize;

    // плохо
    const itemHeight = (item) => item.height >= 256 ? item.largeSize : item.smallSize;

    // хорошо
    const itemHeight = (item) => (item.height <= 256 ? item.largeSize : item.smallSize);

    // хорошо
    const itemHeight = (item) => {
      const { height, largeSize, smallSize } = item;
      return height <= 256 ? largeSize : smallSize;
    };
    ```
### Зафиксируйте расположение тела стрелочной функции с неявным возвратом. 

    ```js
    // плохо
    (foo) =>
      bar;
    (foo) =>
      (bar);

    // хорошо
    (foo) => bar;
    (foo) => (bar);
    (foo) => (
       bar
    )
    ```
___

## 6. Итераторы и генераторы
### Не используйте итераторы. Применяйте функции высшего порядка вместо таких циклов как `for-in` или `for-of`.
> Почему? Это обеспечивает соблюдение нашего правила о неизменности переменных. Работать с чистыми функциями, которые возвращают значение, проще, чем с функциями с побочными эффектами.
> Используйте `map()` / `every()` / `filter()` / `find()` / `findIndex()` / `reduce()` / `some()` / ... для итерации по массивам, а `Object.keys()` / `Object.values()` / `Object.entries()` для создания массивов, с помощью которых можно итерироваться по объектам.

    ```js
    const numbers = [1, 2, 3, 4, 5];

    // плохо
    let sum = 0;
    for (let num of numbers) {
      sum += num;
    }
    sum === 15;

    // хорошо
    let sum = 0;
    numbers.forEach((num) => {
      sum += num;
    });
    sum === 15;

    // отлично (используйте силу функций)
    const sum = numbers.reduce((total, num) => total + num, 0);
    sum === 15;

    // плохо
    const increasedByOne = [];
    for (let i = 0; i < numbers.length; i++) {
      increasedByOne.push(numbers[i] + 1);
    }

    // хорошо
    const increasedByOne = [];
    numbers.forEach((num) => {
      increasedByOne.push(num + 1);
    });

    // отлично (продолжайте в том же духе)
    const increasedByOne = numbers.map((num) => num + 1);
    ```

### Не используйте пока генераторы.
> Почему? Они не очень хорошо транспилируются в ES5.
###Если всё-таки необходимо использовать генераторы, убедитесь, что `*` у функции генератора расположена должным образом.
> Почему? `function` и `*` являются частью одного и того же ключевого слова. `*` не является модификатором для `function`, `function*` является уникальной конструкцией, отличной от `function`.

    ```js
    // плохо
    function * foo() {
      // ...
    }

    const bar = function * () {
      // ...
    };

    const baz = function *() {
      // ...
    };

    const quux = function*() {
      // ...
    };

    function*foo() {
      // ...
    }

    function *foo() {
      // ...
    }

    // очень плохо
    function
    *
    foo() {
      // ...
    }

    const wat = function
    *
    () {
      // ...
    };

    // хорошо
    function* foo() {
      // ...
    }

    const foo = function* () {
      // ...
    };
    ```
___
## 7. Свойства
### Используйте точечную нотацию для доступа к свойствам.

    ```js
    const luke = {
      jedi: true,
      age: 28,
    };

    // плохо
    const isJedi = luke['jedi'];

    // хорошо
    const isJedi = luke.jedi;
    ```
### Используйте скобочную нотацию `[]`, когда название свойства хранится в переменной.

    ```js
    const luke = {
      jedi: true,
      age: 28,
    };

    function getProp(prop) {
      return luke[prop];
    }

    const isJedi = getProp('jedi');
    ```

### Используйте оператор `**` для возведения в степень. 

    ```js
    // плохо
    const binary = Math.pow(2, 10);

    // хорошо
    const binary = 2 ** 10;
    ```
___
## 8. Переменные
### Всегда используйте `const` или `let` для объявления переменных. Невыполнение этого требования приведёт к появлению глобальных переменных. Необходимо избегать загрязнения глобального пространства имён.

    ```js
    // плохо
    superPower = new SuperPower();

    // хорошо
    const superPower = new SuperPower();
    ```

### Используйте объявление `const` или `let` для каждой переменной или присвоения.
> Почему? Таким образом проще добавить новые переменные. Также вы никогда не будете беспокоиться о перемещении `;` и `,` и об отображении изменений в пунктуации. Вы также можете пройтись по каждому объявлению с помощью отладчика, вместо того, чтобы прыгать через все сразу.

    ```js
    // плохо
    const items = getItems(),
        goSportsTeam = true,
        dragonball = 'z';

    // плохо
    // (сравните с кодом выше и попытайтесь найти ошибку)
    const items = getItems(),
        goSportsTeam = true;
        dragonball = 'z';

    // хорошо
    const items = getItems();
    const goSportsTeam = true;
    const dragonball = 'z';
    ```
### В первую очередь группируйте `const`, а затем `let`.
> Почему? Это полезно, когда в будущем вам понадобится создать переменную, зависимую от предыдущих.

    ```js
    // плохо
    let i, len, dragonball,
        items = getItems(),
        goSportsTeam = true;

    // плохо
    let i;
    const items = getItems();
    let dragonball;
    const goSportsTeam = true;
    let len;

    // хорошо
    const goSportsTeam = true;
    const items = getItems();
    let dragonball;
    let i;
    let length;
    ```
### Создавайте переменные там, где они вам необходимы, но помещайте их в подходящее место.
> Почему? `let` и `const` имеют блочную область видимости, а не функциональную.

    ```js
    // плохо - вызов ненужной функции
    function checkName(hasName) {
      const name = getName();

      if (hasName === 'test') {
        return false;
      }

      if (name === 'test') {
        this.setName('');
        return false;
      }

      return name;
    }

    // хорошо
    function checkName(hasName) {
      if (hasName === 'test') {
        return false;
      }

      const name = getName();

      if (name === 'test') {
        this.setName('');
        return false;
      }

      return name;
    }
    ```
### Не создавайте цепочки присваивания переменных.
> Почему? Такие цепочки создают неявные глобальные переменные.

    ```js
    // плохо
    (function example() {
      // JavaScript интерпретирует это, как
      // let a = ( b = ( c = 1 ) );
      // Ключевое слово let применится только к переменной a;
      // переменные b и c станут глобальными.
      let a = b = c = 1;
    }());

    console.log(a); // throws ReferenceError
    console.log(b); // 1
    console.log(c); // 1

    // хорошо
    (function example() {
      let a = 1;
      let b = a;
      let c = a;
    }());

    console.log(a); // throws ReferenceError
    console.log(b); // throws ReferenceError
    console.log(c); // throws ReferenceError

    // тоже самое и для `const`
    ```

### Избегайте использования унарных инкрементов и декрементов (`++`, `--`).
> Почему? Согласно документации eslint, унарные инкремент и декремент автоматически вставляют точку с запятой, что может стать причиной трудноуловимых ошибок при инкрементировании и декрементировании значений. Также нагляднее изменять ваши значения таким образом `num += 1` вместо `num++` или `num ++`. Запрет на унарные инкремент и декремент ограждает вас от непреднамеренных преждевременных инкрементаций/декрементаций значений, которые могут привести к непредсказуемому поведению вашей программы.

    ```js
    // плохо

    const array = [1, 2, 3];
    let num = 1;
    num++;
    --num;

    let sum = 0;
    let truthyCount = 0;
    for (let i = 0; i < array.length; i++) {
      let value = array[i];
      sum += value;
      if (value) {
        truthyCount++;
      }
    }

    // хорошо

    const array = [1, 2, 3];
    let num = 1;
    num += 1;
    num -= 1;

    const sum = array.reduce((a, b) => a + b, 0);
    const truthyCount = array.filter(Boolean).length;
    ```
### Запретить неиспользуемые переменные.
> Почему? Переменные, которые объявлены и не используются в коде, скорее всего, являются ошибкой из-за незавершённого рефакторинга. Такие переменные занимают место в коде и могут привести к путанице при чтении.

    ```js
    // плохо

    var some_unused_var = 42;

    // Переменные, которые используются только для записи, не считаются используемыми.
    var y = 10;
    y = 5;

    // Чтение для собственной модификации.
    var z = 0;
    z = z + 1;

    // Неиспользуемые аргументы функции.
    function getX(x, y) {
        return x;
    }

    // хорошо

    function getXPlusY(x, y) {
      return x + y;
    }

    var x = 1;
    var y = a + 2;

    alert(getXPlusY(x, y));

    // Переменная 'type' игнорируется, даже если она не испольуется, потому что рядом есть rest-свойство.
    // Эта форма извлечения объекта, которая опускает указанные ключи.
    var { type, ...coords } = data;
    // 'coords' теперь 'data' объект без свойства 'type'.
    ```
___
## 9. Точка с зяпятой
### **Да.** 

    ```js
    // плохо - выбрасывает исключение
    const luke = {}
    const leia = {}
    [luke, leia].forEach((jedi) => jedi.father = 'vader')

    // плохо - выбрасывает исключение
    const reaction = "No! That’s impossible!"
    (async function meanwhileOnTheFalcon() {
      // переносимся к `leia`, `lando`, `chewie`, `r2`, `c3p0`
      // ...
    }())

    // хорошо
    const luke = {};
    const leia = {};
    [luke, leia].forEach((jedi) => {
      jedi.father = 'vader';
    });

    // хорошо
    const reaction = "No! That’s impossible!";
    (async function meanwhileOnTheFalcon() {
      // переносимся к `leia`, `lando`, `chewie`, `r2`, `c3p0`
      // ...
      }());
    ```
___
## 10. Коментарии
### Используйте конструкцию `/** ... */` для многострочных комментариев.

    ```js
    // плохо
    // make() возвращает новый элемент
    // соответствующий переданному названию тега
    //
    // @param {String} tag
    // @return {Element} element
    function make(tag) {

      // ...

      return element;
    }

    // хорошо
    /**
     * make() возвращает новый элемент
     * соответствующий переданному названию тега
     */
    function make(tag) {

      // ...

      return element;
    }
    ```

### Используйте двойной слеш `//` для однострочных комментариев. Располагайте такие комментарии отдельной строкой над кодом, который хотите пояснить. Если комментарий не является первой строкой блока, добавьте сверху пустую строку.

    ```js
    // плохо
    const active = true;  // это текущая вкладка

    // хорошо
    // это текущая вкладка
    const active = true;

    // плохо
    function getType() {
      console.log('fetching type...');
      // установить по умолчанию тип 'no type'
      const type = this.type || 'no type';

      return type;
    }

    // хорошо
    function getType() {
      console.log('fetching type...');

      // установить по умолчанию тип 'no type'
      const type = this.type || 'no type';

      return type;
    }

    // тоже хорошо
    function getType() {
      // установить по умолчанию тип 'no type'
      const type = this.type || 'no type';

      return type;
    }
    ```

### Начинайте все комментарии с пробела, так их проще читать. 

    ```js
    // плохо
    //это текущая вкладка
    const active = true;

    // хорошо
    // это текущая вкладка
    const active = true;

    // плохо
    /**
     *make() возвращает новый элемент
     *соответствующий переданному названию тега
     */
    function make(tag) {

      // ...

      return element;
    }

    // хорошо
    /**
     * make() возвращает новый элемент
     * соответствующий переданному названию тега
     */
    function make(tag) {

      // ...

      return element;
    }
    ```

### Если комментарий начинается со слов `FIXME` или `TODO`, то это помогает другим разработчикам быстро понять, когда вы хотите указать на проблему, которую надо решить, или когда вы предлагаете решение проблемы, которое надо реализовать. Такие комментарии, в отличие от обычных, побуждают к действию: `FIXME: -- нужно разобраться с этим` или `TODO: -- нужно реализовать`.

### Используйте `// FIXME:`, чтобы описать проблему.

    ```js
    class Calculator extends Abacus {
      constructor() {
        super();

        // FIXME: здесь не должна использоваться глобальная переменная
        total = 0;
      }
    }
    ```

### Используйте `// TODO:`, чтобы описать решение проблемы.

    ```js
    class Calculator extends Abacus {
      constructor() {
        super();

        // TODO: нужна возможность задать total через параметры
        this.total = 0;
      }
    }
    ```
![bye](https://i.dlpng.com/static/png/6788682_preview.png)

