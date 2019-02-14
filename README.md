# Airbnb JavaScript 代码规范() {

*一种写JavaScript更合理的代码风格。*

> **Note**: 本指南假设你使用了 [Babel](https://babeljs.io), 并且要求你使用 [babel-preset-airbnb](https://npmjs.com/babel-preset-airbnb) 或者其他同等资源。 并且假设你在你的应用中安装了 shims/polyfills ，使用[airbnb-browser-shims](https://npmjs.com/airbnb-browser-shims) 或者相同功能。

[![Downloads](https://img.shields.io/npm/dm/eslint-config-airbnb.svg)](https://www.npmjs.com/package/eslint-config-airbnb)
[![Downloads](https://img.shields.io/npm/dm/eslint-config-airbnb-base.svg)](https://www.npmjs.com/package/eslint-config-airbnb-base)
[![Gitter](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/airbnb/javascript?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge)

<!-- 本指南也提供了其他语言的版本。 查看 [翻译](#translation) -->

其他代码风格指南

  - [ES5 (Deprecated)](https://github.com/airbnb/javascript/tree/es5-deprecated/es5)
  - [React](react/)
  - [CSS-in-JavaScript](css-in-javascript/)
  - [CSS & Sass](https://github.com/airbnb/css)
  - [Ruby](https://github.com/airbnb/ruby)

## <a id="table-of-contents">目录</a>

  1. [类型](#types)
  1. [引用](#references)
  1. [对象](#objects)
  1. [数组](#arrays)
  1. [解构](#destructuring)
  1. [字符](#strings)
  1. [方法](#functions)
  1. [箭头函数](#arrow-functions)
  1. [类和构造器](#classes--constructors)
  1. [模块](#modules)
  1. [迭代器和发生器](#iterators-and-generators)
  1. [属性](#properties)
  1. [变量](#variables)
  1. [提升](#hoisting)
  1. [比较运算符和等号](#comparison-operators--equality)
  1. [块](#blocks)
  1. [控制语句](#control-statements)
  1. [注释](#comments)
  1. [空白](#whitespace)
  1. [逗号](#commas)
  1. [分号](#semicolons)
  1. [类型转换和强制类型转换](#type-casting--coercion)
  1. [命名规范](#naming-conventions)
  1. [存取器](#accessors)
  1. [事件](#events)
  1. [jQuery](#jquery)
  1. [ECMAScript 5 兼容性](#ecmascript-5-compatibility)
  1. [ECMAScript 6+ (ES 2015+) 风格](#ecmascript-6-es-2015-styles)
  1. [标准库](#standard-library)
  1. [测试](#testing)
  1. [性能](#performance)
  1. [资源](#resources)
  1. [JavaScript风格指南的指南](#the-javascript-style-guide-guide)
  1. [许可证](#license)
  1. [修正案](#amendments)

## <a id="types">类型</a>

  <a name="types--primitives"></a><a name="1.1"></a>
  - [1.1](#types--primitives) **原始值**: 当你访问一个原始类型的时候，你可以直接使用它的值。

    - `string`
    - `number`
    - `boolean`
    - `null`
    - `undefined`
    - `symbol`

    ```javascript
    const foo = 1;
    let bar = foo;

    bar = 9;

    console.log(foo, bar); // => 1, 9
    ```

    - 标识符不能完全被支持，因此在针对不支持的浏览器或者环境时不应该使用它们。

  <a name="types--complex"></a><a name="1.2"></a>
  - [1.2](#types--complex)  **复杂类型**: 当你访问一个复杂类型的时候，你需要一个值得引用。

    - `object`
    - `array`
    - `function`

    ```javascript
    const foo = [1, 2];
    const bar = foo;

    bar[0] = 9;

    console.log(foo[0], bar[0]); // => 9, 9
    ```

**[⬆ 返回目录](#table-of-contents)**

## <a id="references">引用</a>

  <a name="references--prefer-const"></a><a name="2.1"></a>
  - [2.1](#references--prefer-const) 使用 `const` 定义你的所有引用；避免使用 `var`。 eslint: [`prefer-const`](https://eslint.org/docs/rules/prefer-const.html), [`no-const-assign`](https://eslint.org/docs/rules/no-const-assign.html)

    > 为什么? 这样能够确保你不能重新赋值你的引用，否则可能导致错误或者产生难以理解的代码。.

    ```javascript
    // bad
    var a = 1;
    var b = 2;

    // good
    const a = 1;
    const b = 2;
    ```

  <a name="references--disallow-var"></a><a name="2.2"></a>
  - [2.2](#references--disallow-var) 如果你必须重新赋值你的引用， 使用 `let` 代替 `var`。 eslint: [`no-var`](https://eslint.org/docs/rules/no-var.html)

    > 为什么? `let` 是块级作用域，而不像 `var` 是函数作用域.

    ```javascript
    // bad
    var count = 1;
    if (true) {
      count += 1;
    }

    // good, use the let.
    let count = 1;
    if (true) {
      count += 1;
    }
    ```

  <a name="references--block-scope"></a><a name="2.3"></a>
  - [2.3](#references--block-scope) 注意，let 和 const 都是块级范围的。

    ```javascript
    // const 和 let 只存在于他们定义的块中。
    {
      let a = 1;
      const b = 1;
    }
    console.log(a); // ReferenceError
    console.log(b); // ReferenceError
    ```

**[⬆ 返回目录](#table-of-contents)**

## <a id="object">对象</a>

  <a name="objects--no-new"></a><a name="3.1"></a>
  - [3.1](#objects--no-new) 使用字面语法来创建对象。 eslint: [`no-new-object`](https://eslint.org/docs/rules/no-new-object.html)

    ```javascript
    // bad
    const item = new Object();

    // good
    const item = {};
    ```

  <a name="es6-computed-properties"></a><a name="3.4"></a>
  - [3.2](#es6-computed-properties) 在创建具有动态属性名称的对象时使用计算属性名。

    > 为什么? 它允许你在一个地方定义对象的所有属性。

    ```javascript

    function getKey(k) {
      return `a key named ${k}`;
    }

    // bad
    const obj = {
      id: 5,
      name: 'San Francisco',
    };
    obj[getKey('enabled')] = true;

    // good
    const obj = {
      id: 5,
      name: 'San Francisco',
      [getKey('enabled')]: true,
    };
    ```

  <a name="es6-object-shorthand"></a><a name="3.5"></a>
  - [3.3](#es6-object-shorthand) 使用对象方法的缩写。 eslint: [`object-shorthand`](https://eslint.org/docs/rules/object-shorthand.html)

    ```javascript
    // bad
    const atom = {
      value: 1,

      addValue: function (value) {
        return atom.value + value;
      },
    };

    // good
    const atom = {
      value: 1,

      addValue(value) {
        return atom.value + value;
      },
    };
    ```

  <a name="es6-object-concise"></a><a name="3.6"></a>
  - [3.4](#es6-object-concise) 使用属性值的缩写。 eslint: [`object-shorthand`](https://eslint.org/docs/rules/object-shorthand.html)

    > 为什么? 它的写法和描述较短。

    ```javascript
    const lukeSkywalker = 'Luke Skywalker';

    // bad
    const obj = {
      lukeSkywalker: lukeSkywalker,
    };

    // good
    const obj = {
      lukeSkywalker,
    };
    ```

  <a name="objects--grouped-shorthand"></a><a name="3.7"></a>
  - [3.5](#objects--grouped-shorthand) 在对象声明的时候将简写的属性进行分组。

    > 为什么? 这样更容易的判断哪些属性使用的简写。

    ```javascript
    const anakinSkywalker = 'Anakin Skywalker';
    const lukeSkywalker = 'Luke Skywalker';

    // bad
    const obj = {
      episodeOne: 1,
      twoJediWalkIntoACantina: 2,
      lukeSkywalker,
      episodeThree: 3,
      mayTheFourth: 4,
      anakinSkywalker,
    };

    // good
    const obj = {
      lukeSkywalker,
      anakinSkywalker,
      episodeOne: 1,
      twoJediWalkIntoACantina: 2,
      episodeThree: 3,
      mayTheFourth: 4,
    };
    ```

  <a name="objects--quoted-props"></a><a name="3.8"></a>
  - [3.6](#objects--quoted-props) 只使用引号标注无效标识符的属性。 eslint: [`quote-props`](https://eslint.org/docs/rules/quote-props.html)

    > 为什么? 总的来说，我们认为这样更容易阅读。 它提升了语法高亮显示，并且更容易通过许多 JS 引擎优化。
    ```javascript
    // bad
    const bad = {
      'foo': 3,
      'bar': 4,
      'data-blah': 5,
    };

    // good
    const good = {
      foo: 3,
      bar: 4,
      'data-blah': 5,
    };
    ```

  <a name="objects--prototype-builtins"></a>
  - [3.7](#objects--prototype-builtins) 不能直接调用 `Object.prototype` 的方法，如： `hasOwnProperty` 、 `propertyIsEnumerable` 和 `isPrototypeOf`。

    > 为什么? 这些方法可能被一下问题对象的属性追踪 - 相应的有 `{ hasOwnProperty: false }` - 或者，对象是一个空对象 (`Object.create(null)`)。

    ```javascript
    // bad
    console.log(object.hasOwnProperty(key));

    // good
    console.log(Object.prototype.hasOwnProperty.call(object, key));

    // best
    const has = Object.prototype.hasOwnProperty; // 在模块范围内的缓存中查找一次
    /* or */
    import has from 'has'; // https://www.npmjs.com/package/has
    // ...
    console.log(has.call(object, key));
    ```

  <a name="objects--rest-spread"></a>
  - [3.8](#objects--rest-spread) 更喜欢对象扩展操作符，而不是用 [`Object.assign`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Object/assign) 浅拷贝一个对象。 使用对象的 rest 操作符来获得一个具有某些属性的新对象。

    ```javascript
    // very bad
    const original = { a: 1, b: 2 };
    const copy = Object.assign(original, { c: 3 }); // 变异的 `original` ಠ_ಠ
    delete copy.a; // 这....

    // bad
    const original = { a: 1, b: 2 };
    const copy = Object.assign({}, original, { c: 3 }); // copy => { a: 1, b: 2, c: 3 }

    // good
    const original = { a: 1, b: 2 };
    const copy = { ...original, c: 3 }; // copy => { a: 1, b: 2, c: 3 }

    const { a, ...noA } = copy; // noA => { b: 2, c: 3 }
    ```

**[⬆ 返回目录](#table-of-contents)**

## <a id="arrays">数组</a>

  <a name="arrays--literals"></a><a name="4.1"></a>
  - [4.1](#arrays--literals) 使用字面语法创建数组。 eslint: [`no-array-constructor`](https://eslint.org/docs/rules/no-array-constructor.html)

    ```javascript
    // bad
    const items = new Array();

    // good
    const items = [];
    ```

  <a name="arrays--push"></a><a name="4.2"></a>
  - [4.2](#arrays--push) 使用 [Array#push](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Array/push) 取代直接赋值来给数组添加项。

    ```javascript
    const someStack = [];

    // bad
    someStack[someStack.length] = 'abracadabra';

    // good
    someStack.push('abracadabra');
    ```

  <a name="es6-array-spreads"></a><a name="4.3"></a>
  - [4.3](#es6-array-spreads) 使用数组展开方法 `...` 来拷贝数组。

    ```javascript
    // bad
    const len = items.length;
    const itemsCopy = [];
    let i;

    for (i = 0; i < len; i += 1) {
      itemsCopy[i] = items[i];
    }

    // good
    const itemsCopy = [...items];
    ```

  <a name="arrays--from"></a><a name="4.4"></a>
  - [4.4](#arrays--from) 将一个类数组对象转换成一个数组， 使用展开方法 `...` 代替 [`Array.from`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Array/from)。

    ```javascript
    const foo = document.querySelectorAll('.foo');

    // good
    const nodes = Array.from(foo);

    // best
    const nodes = [...foo];
    ```

  <a name="arrays--mapping"></a>
  - [4.5](#arrays--mapping) 对于对迭代器的映射，使用 [Array.from](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Array/from) 替代展开方法 `...` ， 因为它避免了创建中间数组。

    ```javascript
    // bad
    const baz = [...foo].map(bar);

    // good
    const baz = Array.from(foo, bar);
    ```

  <a name="arrays--callback-return"></a><a name="4.5"></a>
  - [4.6](#arrays--callback-return) 在数组回调方法中使用 return 语句。 如果函数体由一个返回无副作用的表达式的单个语句组成，那么可以省略返回值， 具体查看 [8.2](#arrows--implicit-return)。 eslint: [`array-callback-return`](https://eslint.org/docs/rules/array-callback-return)

    ```javascript
    // good
    [1, 2, 3].map((x) => {
      const y = x + 1;
      return x * y;
    });

    // good
    [1, 2, 3].map(x => x + 1);

    // bad - 没有返回值，意味着在第一次迭代后 `acc` 没有被定义
    [[0, 1], [2, 3], [4, 5]].reduce((acc, item, index) => {
      const flatten = acc.concat(item);
      acc[index] = flatten;
    });

    // good
    [[0, 1], [2, 3], [4, 5]].reduce((acc, item, index) => {
      const flatten = acc.concat(item);
      acc[index] = flatten;
      return flatten;
    });

    // bad
    inbox.filter((msg) => {
      const { subject, author } = msg;
      if (subject === 'Mockingbird') {
        return author === 'Harper Lee';
      } else {
        return false;
      }
    });

    // good
    inbox.filter((msg) => {
      const { subject, author } = msg;
      if (subject === 'Mockingbird') {
        return author === 'Harper Lee';
      }

      return false;
    });
    ```

  <a name="arrays--bracket-newline"></a>
  - [4.7](#arrays--bracket-newline) 如果数组有多行，则在开始的时候换行，然后在结束的时候换行。

    ```javascript
    // bad
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

    // good
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

**[⬆ 返回目录](#table-of-contents)**

## <a id="destructuring">解构</a>

  <a name="destructuring--object"></a><a name="5.1"></a>
  - [5.1](#destructuring--object) 在访问和使用对象的多个属性的时候使用对象的解构。 eslint: [`prefer-destructuring`](https://eslint.org/docs/rules/prefer-destructuring)

    > 为什么? 解构可以避免为这些属性创建临时引用。

    ```javascript
    // bad
    function getFullName(user) {
      const firstName = user.firstName;
      const lastName = user.lastName;

      return `${firstName} ${lastName}`;
    }

    // good
    function getFullName(user) {
      const { firstName, lastName } = user;
      return `${firstName} ${lastName}`;
    }

    // best
    function getFullName({ firstName, lastName }) {
      return `${firstName} ${lastName}`;
    }
    ```

  <a name="destructuring--array"></a><a name="5.2"></a>
  - [5.2](#destructuring--array) 使用数组解构。 eslint: [`prefer-destructuring`](https://eslint.org/docs/rules/prefer-destructuring)

    ```javascript
    const arr = [1, 2, 3, 4];

    // bad
    const first = arr[0];
    const second = arr[1];

    // good
    const [first, second] = arr;
    ```

  <a name="destructuring--object-over-array"></a><a name="5.3"></a>
  - [5.3](#destructuring--object-over-array) 对于多个返回值使用对象解构，而不是数组解构。

    > 为什么? 你可以随时添加新的属性或者改变属性的顺序，而不用修改调用方。

    ```javascript
    // bad
    function processInput(input) {
      // 处理代码...
      return [left, right, top, bottom];
    }

    // 调用者需要考虑返回数据的顺序。
    const [left, __, top] = processInput(input);

    // good
    function processInput(input) {
      // 处理代码...
      return { left, right, top, bottom };
    }

    // 调用者只选择他们需要的数据。
    const { left, top } = processInput(input);
    ```

**[⬆ 返回目录](#table-of-contents)**

## <a id="strings">字符</a>

  <a name="strings--quotes"></a><a name="6.1"></a>
  - [6.1](#strings--quotes) 使用单引号 `''` 定义字符串。 eslint: [`quotes`](https://eslint.org/docs/rules/quotes.html)

    ```javascript
    // bad
    const name = "Capt. Janeway";

    // bad - 模板文字应该包含插值或换行。
    const name = `Capt. Janeway`;

    // good
    const name = 'Capt. Janeway';
    ```

  <a name="strings--line-length"></a><a name="6.2"></a>
  - [6.2](#strings--line-length) 使行超过100个字符的字符串不应使用字符串连接跨多行写入。

    > 为什么? 断开的字符串更加难以工作，并且使代码搜索更加困难。

    ```javascript
    // bad
    const errorMessage = 'This is a super long error that was thrown because \
    of Batman. When you stop to think about how Batman had anything to do \
    with this, you would get nowhere \
    fast.';

    // bad
    const errorMessage = 'This is a super long error that was thrown because ' +
      'of Batman. When you stop to think about how Batman had anything to do ' +
      'with this, you would get nowhere fast.';

    // good
    const errorMessage = 'This is a super long error that was thrown because of Batman. When you stop to think about how Batman had anything to do with this, you would get nowhere fast.';
    ```

  <a name="es6-template-literals"></a><a name="6.4"></a>
  - [6.3](#es6-template-literals) 当以编程模式构建字符串时，使用字符串模板代替字符串拼接。 eslint: [`prefer-template`](https://eslint.org/docs/rules/prefer-template.html) [`template-curly-spacing`](https://eslint.org/docs/rules/template-curly-spacing)

    > 为什么? 字符串模板为您提供了一种可读的、简洁的语法，具有正确的换行和字符串插值特性。

    ```javascript
    // bad
    function sayHi(name) {
      return 'How are you, ' + name + '?';
    }

    // bad
    function sayHi(name) {
      return ['How are you, ', name, '?'].join();
    }

    // bad
    function sayHi(name) {
      return `How are you, ${ name }?`;
    }

    // good
    function sayHi(name) {
      return `How are you, ${name}?`;
    }
    ```

  <a name="strings--eval"></a><a name="6.5"></a>
  - [6.4](#strings--eval) 不要在字符串上使用 `eval()` ，它打开了太多漏洞。 eslint: [`no-eval`](https://eslint.org/docs/rules/no-eval)

  <a name="strings--escaping"></a>
  - [6.5](#strings--escaping) 不要转义字符串中不必要的字符。 eslint: [`no-useless-escape`](https://eslint.org/docs/rules/no-useless-escape)

    > 为什么? 反斜杠损害了可读性，因此只有在必要的时候才会出现。

    ```javascript
    // bad
    const foo = '\'this\' \i\s \"quoted\"';

    // good
    const foo = '\'this\' is "quoted"';
    const foo = `my name is '${name}'`;
    ```

**[⬆ 返回目录](#table-of-contents)**

## <a id="functions">方法</a>

  <a name="functions--declarations"></a><a name="7.1"></a>
  - [7.1](#functions--declarations) 使用命名的函数表达式代替函数声明。 eslint: [`func-style`](https://eslint.org/docs/rules/func-style)

    > 为什么? 函数声明是挂起的，这意味着在它在文件中定义之前，很容易引用函数。这会损害可读性和可维护性。如果您发现函数的定义是大的或复杂的，以至于它干扰了对文件的其余部分的理解，那么也许是时候将它提取到它自己的模块中了!不要忘记显式地命名这个表达式，不管它的名称是否从包含变量(在现代浏览器中经常是这样，或者在使用诸如Babel之类的编译器时)。这消除了对错误的调用堆栈的任何假设。 ([Discussion](https://github.com/airbnb/javascript/issues/794))

    ```javascript
    // bad
    function foo() {
      // ...
    }

    // bad
    const foo = function () {
      // ...
    };

    // good
    // 从变量引用调用中区分的词汇名称
    const short = function longUniqueMoreDescriptiveLexicalFoo() {
      // ...
    };
    ```

  <a name="functions--iife"></a><a name="7.2"></a>
  - [7.2](#functions--iife) Wrap立即调用函数表达式。 eslint: [`wrap-iife`](https://eslint.org/docs/rules/wrap-iife.html)

    > 为什么? 立即调用的函数表达式是单个单元 - 包装， 并且拥有括号调用, 在括号内, 清晰的表达式。 请注意，在一个到处都是模块的世界中，您几乎不需要一个 IIFE 。

    ```javascript
    // immediately-invoked function expression (IIFE) 立即调用的函数表达式
    (function () {
      console.log('Welcome to the Internet. Please follow me.');
    }());
    ```

  <a name="functions--in-blocks"></a><a name="7.3"></a>
  - [7.3](#functions--in-blocks) 切记不要在非功能块中声明函数 (`if`, `while`, 等)。 将函数赋值给变量。 浏览器允许你这样做，但是他们都有不同的解释，这是个坏消息。 eslint: [`no-loop-func`](https://eslint.org/docs/rules/no-loop-func.html)

  <a name="functions--note-on-blocks"></a><a name="7.4"></a>
  - [7.4](#functions--note-on-blocks) **注意:** ECMA-262 将 `block` 定义为语句列表。 函数声明不是语句。

    ```javascript
    // bad
    if (currentUser) {
      function test() {
        console.log('Nope.');
      }
    }

    // good
    let test;
    if (currentUser) {
      test = () => {
        console.log('Yup.');
      };
    }
    ```

  <a name="functions--arguments-shadow"></a><a name="7.5"></a>
  - [7.5](#functions--arguments-shadow) 永远不要定义一个参数为 `arguments`。 这将会优先于每个函数给定范围的 `arguments` 对象。

    ```javascript
    // bad
    function foo(name, options, arguments) {
      // ...
    }

    // good
    function foo(name, options, args) {
      // ...
    }
    ```

  <a name="es6-rest"></a><a name="7.6"></a>
  - [7.6](#es6-rest) 不要使用 `arguments`, 选择使用 rest 语法 `...` 代替。 eslint: [`prefer-rest-params`](https://eslint.org/docs/rules/prefer-rest-params)

    > 为什么? `...` 明确了你想要拉取什么参数。 更甚, rest 参数是一个真正的数组，而不仅仅是类数组的 `arguments` 。

    ```javascript
    // bad
    function concatenateAll() {
      const args = Array.prototype.slice.call(arguments);
      return args.join('');
    }

    // good
    function concatenateAll(...args) {
      return args.join('');
    }
    ```

  <a name="es6-default-parameters"></a><a name="7.7"></a>
  - [7.7](#es6-default-parameters) 使用默认的参数语法，而不是改变函数参数。

    ```javascript
    // really bad
    function handleThings(opts) {
      // No! We shouldn’t mutate function arguments.
      // Double bad: if opts is falsy it'll be set to an object which may
      // be what you want but it can introduce subtle bugs.
      opts = opts || {};
      // ...
    }

    // still bad
    function handleThings(opts) {
      if (opts === void 0) {
        opts = {};
      }
      // ...
    }

    // good
    function handleThings(opts = {}) {
      // ...
    }
    ```

  <a name="functions--default-side-effects"></a><a name="7.8"></a>
  - [7.8](#functions--default-side-effects) 避免使用默认参数的副作用。

    > 为什么? 他们很容易混淆。

    ```javascript
    var b = 1;
    // bad
    function count(a = b++) {
      console.log(a);
    }
    count();  // 1
    count();  // 2
    count(3); // 3
    count();  // 3
    ```

  <a name="functions--defaults-last"></a><a name="7.9"></a>
  - [7.9](#functions--defaults-last) 总是把默认参数放在最后。

    ```javascript
    // bad
    function handleThings(opts = {}, name) {
      // ...
    }

    // good
    function handleThings(name, opts = {}) {
      // ...
    }
    ```

  <a name="functions--constructor"></a><a name="7.10"></a>
  - [7.10](#functions--constructor) 永远不要使用函数构造器来创建一个新函数。 eslint: [`no-new-func`](https://eslint.org/docs/rules/no-new-func)

    > 为什么? 以这种方式创建一个函数将对一个类似于 `eval()` 的字符串进行计算，这将打开漏洞。

    ```javascript
    // bad
    var add = new Function('a', 'b', 'return a + b');

    // still bad
    var subtract = Function('a', 'b', 'return a - b');
    ```

  <a name="functions--signature-spacing"></a><a name="7.11"></a>
  - [7.11](#functions--signature-spacing) 函数签名中的间距。 eslint: [`space-before-function-paren`](https://eslint.org/docs/rules/space-before-function-paren) [`space-before-blocks`](https://eslint.org/docs/rules/space-before-blocks)

    > 为什么? 一致性很好，在删除或添加名称时不需要添加或删除空格。

    ```javascript
    // bad
    const f = function(){};
    const g = function (){};
    const h = function() {};

    // good
    const x = function () {};
    const y = function a() {};
    ```

  <a name="functions--mutate-params"></a><a name="7.12"></a>
  - [7.12](#functions--mutate-params) 没用变异参数。 eslint: [`no-param-reassign`](https://eslint.org/docs/rules/no-param-reassign.html)

    > 为什么? 将传入的对象作为参数进行操作可能会在原始调用程序中造成不必要的变量副作用。

    ```javascript
    // bad
    function f1(obj) {
      obj.key = 1;
    }

    // good
    function f2(obj) {
      const key = Object.prototype.hasOwnProperty.call(obj, 'key') ? obj.key : 1;
    }
    ```

  <a name="functions--reassign-params"></a><a name="7.13"></a>
  - [7.13](#functions--reassign-params) 不要再赋值参数。 eslint: [`no-param-reassign`](https://eslint.org/docs/rules/no-param-reassign.html)

    > 为什么? 重新赋值参数会导致意外的行为，尤其是在访问 `arguments` 对象的时候。 它还可能导致性能优化问题，尤其是在 V8 中。

    ```javascript
    // bad
    function f1(a) {
      a = 1;
      // ...
    }

    function f2(a) {
      if (!a) { a = 1; }
      // ...
    }

    // good
    function f3(a) {
      const b = a || 1;
      // ...
    }

    function f4(a = 1) {
      // ...
    }
    ```

  <a name="functions--spread-vs-apply"></a><a name="7.14"></a>
  - [7.14](#functions--spread-vs-apply) 优先使用扩展运算符 `...` 来调用可变参数函数。 eslint: [`prefer-spread`](https://eslint.org/docs/rules/prefer-spread)

    > 为什么? 它更加干净，你不需要提供上下文，并且你不能轻易的使用 `apply` 来 `new` 。

    ```javascript
    // bad
    const x = [1, 2, 3, 4, 5];
    console.log.apply(console, x);

    // good
    const x = [1, 2, 3, 4, 5];
    console.log(...x);

    // bad
    new (Function.prototype.bind.apply(Date, [null, 2016, 8, 5]));

    // good
    new Date(...[2016, 8, 5]);
    ```

  <a name="functions--signature-invocation-indentation"></a>
  - [7.15](#functions--signature-invocation-indentation) 具有多行签名或者调用的函数应该像本指南中的其他多行列表一样缩进：在一行上只有一个条目，并且每个条目最后加上逗号。 eslint: [`function-paren-newline`](https://eslint.org/docs/rules/function-paren-newline)

    ```javascript
    // bad
    function foo(bar,
                 baz,
                 quux) {
      // ...
    }

    // good
    function foo(
      bar,
      baz,
      quux,
    ) {
      // ...
    }

    // bad
    console.log(foo,
      bar,
      baz);

    // good
    console.log(
      foo,
      bar,
      baz,
    );
    ```

**[⬆ 返回目录](#table-of-contents)**

## <a id="arrow-functions">箭头函数</a>

  <a name="arrows--use-them"></a><a name="8.1"></a>
  - [8.1](#arrows--use-them) 当你必须使用匿名函数时 (当传递内联函数时)， 使用箭头函数。 eslint: [`prefer-arrow-callback`](https://eslint.org/docs/rules/prefer-arrow-callback.html), [`arrow-spacing`](https://eslint.org/docs/rules/arrow-spacing.html)

    > 为什么? 它创建了一个在 `this` 上下文中执行的函数版本，它通常是你想要的，并且是一个更简洁的语法。

    > 为什么不? 如果你有一个相当复杂的函数，你可以把这个逻辑转移到它自己的命名函数表达式中。

    ```javascript
    // bad
    [1, 2, 3].map(function (x) {
      const y = x + 1;
      return x * y;
    });

    // good
    [1, 2, 3].map((x) => {
      const y = x + 1;
      return x * y;
    });
    ```

  <a name="arrows--implicit-return"></a><a name="8.2"></a>
  - [8.2](#arrows--implicit-return) 如果函数体包含一个单独的语句，返回一个没有副作用的 [expression](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Expressions_and_Operators#Expressions) ， 省略括号并使用隐式返回。否则，保留括号并使用 `return` 语句。 eslint: [`arrow-parens`](https://eslint.org/docs/rules/arrow-parens.html), [`arrow-body-style`](https://eslint.org/docs/rules/arrow-body-style.html)

    > 为什么? 语法糖。 多个函数被链接在一起时，提高可读性。

    ```javascript
    // bad
    [1, 2, 3].map(number => {
      const nextNumber = number + 1;
      `A string containing the ${nextNumber}.`;
    });

    // good
    [1, 2, 3].map(number => `A string containing the ${number}.`);

    // good
    [1, 2, 3].map((number) => {
      const nextNumber = number + 1;
      return `A string containing the ${nextNumber}.`;
    });

    // good
    [1, 2, 3].map((number, index) => ({
      [index]: number,
    }));

    // 没有副作用的隐式返回
    function foo(callback) {
      const val = callback();
      if (val === true) {
        // 如果回调返回 true 执行
      }
    }

    let bool = false;

    // bad
    foo(() => bool = true);

    // good
    foo(() => {
      bool = true;
    });
    ```

  <a name="arrows--paren-wrap"></a><a name="8.3"></a>
  - [8.3](#arrows--paren-wrap) 如果表达式跨越多个行，用括号将其括起来，以获得更好的可读性。

    > 为什么? 它清楚地显示了函数的起点和终点。

    ```javascript
    // bad
    ['get', 'post', 'put'].map(httpMethod => Object.prototype.hasOwnProperty.call(
        httpMagicObjectWithAVeryLongName,
        httpMethod,
      )
    );

    // good
    ['get', 'post', 'put'].map(httpMethod => (
      Object.prototype.hasOwnProperty.call(
        httpMagicObjectWithAVeryLongName,
        httpMethod,
      )
    ));
    ```

  <a name="arrows--one-arg-parens"></a><a name="8.4"></a>
  - [8.4](#arrows--one-arg-parens) 如果你的函数接收一个参数，则可以不用括号，省略括号。 否则，为了保证清晰和一致性，需要在参数周围加上括号。 注意：总是使用括号是可以接受的，在这种情况下，我们使用 [“always” option](https://eslint.org/docs/rules/arrow-parens#always) 来配置 eslint. eslint: [`arrow-parens`](https://eslint.org/docs/rules/arrow-parens.html)

    > 为什么? 减少视觉上的混乱。

    ```javascript
    // bad
    [1, 2, 3].map((x) => x * x);

    // good
    [1, 2, 3].map(x => x * x);

    // good
    [1, 2, 3].map(number => (
      `A long string with the ${number}. It’s so long that we don’t want it to take up space on the .map line!`
    ));

    // bad
    [1, 2, 3].map(x => {
      const y = x + 1;
      return x * y;
    });

    // good
    [1, 2, 3].map((x) => {
      const y = x + 1;
      return x * y;
    });
    ```

  <a name="arrows--confusing"></a><a name="8.5"></a>
  - [8.5](#arrows--confusing) 避免箭头函数符号 (`=>`) 和比较运算符 (`<=`, `>=`) 的混淆。 eslint: [`no-confusing-arrow`](https://eslint.org/docs/rules/no-confusing-arrow)

    ```javascript
    // bad
    const itemHeight = item => item.height > 256 ? item.largeSize : item.smallSize;

    // bad
    const itemHeight = (item) => item.height > 256 ? item.largeSize : item.smallSize;

    // good
    const itemHeight = item => (item.height > 256 ? item.largeSize : item.smallSize);

    // good
    const itemHeight = (item) => {
      const { height, largeSize, smallSize } = item;
      return height > 256 ? largeSize : smallSize;
    };
    ```

  <a name="whitespace--implicit-arrow-linebreak"></a>
  - [8.6](#whitespace--implicit-arrow-linebreak) 注意带有隐式返回的箭头函数函数体的位置。 eslint: [`implicit-arrow-linebreak`](https://eslint.org/docs/rules/implicit-arrow-linebreak)

    ```javascript
    // bad
    (foo) =>
      bar;

    (foo) =>
      (bar);

    // good
    (foo) => bar;
    (foo) => (bar);
    (foo) => (
       bar
    )
    ```

**[⬆ 返回目录](#table-of-contents)**

## <a id="classes--constructors">类和构造器</a>

  <a name="constructors--use-class"></a><a name="9.1"></a>
  - [9.1](#constructors--use-class) 尽量使用 `class`. 避免直接操作 `prototype` .

    > 为什么? `class` 语法更简洁，更容易推理。

    ```javascript
    // bad
    function Queue(contents = []) {
      this.queue = [...contents];
    }
    Queue.prototype.pop = function () {
      const value = this.queue[0];
      this.queue.splice(0, 1);
      return value;
    };

    // good
    class Queue {
      constructor(contents = []) {
        this.queue = [...contents];
      }
      pop() {
        const value = this.queue[0];
        this.queue.splice(0, 1);
        return value;
      }
    }
    ```

  <a name="constructors--extends"></a><a name="9.2"></a>
  - [9.2](#constructors--extends) 使用 `extends` 来扩展继承。

    > 为什么? 它是一个内置的方法，可以在不破坏 `instanceof` 的情况下继承原型功能。

    ```javascript
    // bad
    const inherits = require('inherits');
    function PeekableQueue(contents) {
      Queue.apply(this, contents);
    }
    inherits(PeekableQueue, Queue);
    PeekableQueue.prototype.peek = function () {
      return this.queue[0];
    };

    // good
    class PeekableQueue extends Queue {
      peek() {
        return this.queue[0];
      }
    }
    ```

  <a name="constructors--chaining"></a><a name="9.3"></a>
  - [9.3](#constructors--chaining) 方法返回了 `this` 来供其内部方法调用。

    ```javascript
    // bad
    Jedi.prototype.jump = function () {
      this.jumping = true;
      return true;
    };

    Jedi.prototype.setHeight = function (height) {
      this.height = height;
    };

    const luke = new Jedi();
    luke.jump(); // => true
    luke.setHeight(20); // => undefined

    // good
    class Jedi {
      jump() {
        this.jumping = true;
        return this;
      }

      setHeight(height) {
        this.height = height;
        return this;
      }
    }

    const luke = new Jedi();

    luke.jump()
      .setHeight(20);
    ```

  <a name="constructors--tostring"></a><a name="9.4"></a>
  - [9.4](#constructors--tostring) 只要在确保能正常工作并且不产生任何副作用的情况下，编写一个自定义的 `toString()` 方法也是可以的。

    ```javascript
    class Jedi {
      constructor(options = {}) {
        this.name = options.name || 'no name';
      }

      getName() {
        return this.name;
      }

      toString() {
        return `Jedi - ${this.getName()}`;
      }
    }
    ```

  <a name="constructors--no-useless"></a><a name="9.5"></a>
  - [9.5](#constructors--no-useless) 如果没有指定类，则类具有默认的构造器。 一个空的构造器或是一个代表父类的函数是没有必要的。 eslint: [`no-useless-constructor`](https://eslint.org/docs/rules/no-useless-constructor)

    ```javascript
    // bad
    class Jedi {
      constructor() {}

      getName() {
        return this.name;
      }
    }

    // bad
    class Rey extends Jedi {
      constructor(...args) {
        super(...args);
      }
    }

    // good
    class Rey extends Jedi {
      constructor(...args) {
        super(...args);
        this.name = 'Rey';
      }
    }
    ```

  <a name="classes--no-duplicate-members"></a>
  - [9.6](#classes--no-duplicate-members) 避免定义重复的类成员。 eslint: [`no-dupe-class-members`](https://eslint.org/docs/rules/no-dupe-class-members)

    > 为什么? 重复的类成员声明将会默认倾向于最后一个 - 具有重复的类成员可以说是一个错误。

    ```javascript
    // bad
    class Foo {
      bar() { return 1; }
      bar() { return 2; }
    }

    // good
    class Foo {
      bar() { return 1; }
    }

    // good
    class Foo {
      bar() { return 2; }
    }
    ```

**[⬆ 返回目录](#table-of-contents)**

## <a id="modules">模块</a>

  <a name="modules--use-them"></a><a name="10.1"></a>
  - [10.1](#modules--use-them) 你可能经常使用模块 (`import`/`export`) 在一些非标准模块的系统上。 你也可以在你喜欢的模块系统上相互转换。

    > 为什么? 模块是未来的趋势，让我们拥抱未来。

    ```javascript
    // bad
    const AirbnbStyleGuide = require('./AirbnbStyleGuide');
    module.exports = AirbnbStyleGuide.es6;

    // ok
    import AirbnbStyleGuide from './AirbnbStyleGuide';
    export default AirbnbStyleGuide.es6;

    // best
    import { es6 } from './AirbnbStyleGuide';
    export default es6;
    ```

  <a name="modules--no-wildcard"></a><a name="10.2"></a>
  - [10.2](#modules--no-wildcard) 不要使用通配符导入。

    > 为什么? 这确定你有一个单独的默认导出。

    ```javascript
    // bad
    import * as AirbnbStyleGuide from './AirbnbStyleGuide';

    // good
    import AirbnbStyleGuide from './AirbnbStyleGuide';
    ```

  <a name="modules--no-export-from-import"></a><a name="10.3"></a>
  - [10.3](#modules--no-export-from-import) 不要直接从导入导出。

    > 为什么? 虽然写在一行很简洁，但是有一个明确的导入和一个明确的导出能够保证一致性。

    ```javascript
    // bad
    // filename es6.js
    export { es6 as default } from './AirbnbStyleGuide';

    // good
    // filename es6.js
    import { es6 } from './AirbnbStyleGuide';
    export default es6;
    ```

  <a name="modules--no-duplicate-imports"></a>
  - [10.4](#modules--no-duplicate-imports) 只从一个路径导入所有需要的东西。
 eslint: [`no-duplicate-imports`](https://eslint.org/docs/rules/no-duplicate-imports)
    > 为什么? 从同一个路径导入多个行，使代码更难以维护。

    ```javascript
    // bad
    import foo from 'foo';
    // … 其他导入 … //
    import { named1, named2 } from 'foo';

    // good
    import foo, { named1, named2 } from 'foo';

    // good
    import foo, {
      named1,
      named2,
    } from 'foo';
    ```

  <a name="modules--no-mutable-exports"></a>
  - [10.5](#modules--no-mutable-exports) 不要导出可变的引用。
 eslint: [`import/no-mutable-exports`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/no-mutable-exports.md)
    > 为什么? 在一般情况下，应该避免发生突变，但是在导出可变引用时及其容易发生突变。虽然在某些特殊情况下，可能需要这样，但是一般情况下只需要导出常量引用。

    ```javascript
    // bad
    let foo = 3;
    export { foo };

    // good
    const foo = 3;
    export { foo };
    ```

  <a name="modules--prefer-default-export"></a>
  - [10.6](#modules--prefer-default-export) 在单个导出的模块中，选择默认模块而不是指定的导出。
 eslint: [`import/prefer-default-export`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/prefer-default-export.md)
    > 为什么? 为了鼓励更多的文件只导出一件东西，这样可读性和可维护性更好。

    ```javascript
    // bad
    export function foo() {}

    // good
    export default function foo() {}
    ```

  <a name="modules--imports-first"></a>
  - [10.7](#modules--imports-first) 将所有的 `import`s 语句放在所有非导入语句的上边。
 eslint: [`import/first`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/first.md)
    > 为什么? 由于所有的 `import`s 都被提前，保持他们在顶部是为了防止意外发生。

    ```javascript
    // bad
    import foo from 'foo';
    foo.init();

    import bar from 'bar';

    // good
    import foo from 'foo';
    import bar from 'bar';

    foo.init();
    ```

  <a name="modules--multiline-imports-over-newlines"></a>
  - [10.8](#modules--multiline-imports-over-newlines) 多行导入应该像多行数组和对象一样缩进。

    > 为什么? 花括号和其他规范一样，遵循相同的缩进规则，后边的都好一样。

    ```javascript
    // bad
    import {longNameA, longNameB, longNameC, longNameD, longNameE} from 'path';

    // good
    import {
      longNameA,
      longNameB,
      longNameC,
      longNameD,
      longNameE,
    } from 'path';
    ```

  <a name="modules--no-webpack-loader-syntax"></a>
  - [10.9](#modules--no-webpack-loader-syntax) 在模块导入语句中禁止使用 Webpack 加载器语法。
 eslint: [`import/no-webpack-loader-syntax`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/no-webpack-loader-syntax.md)
    > 为什么? 因为在导入语句中使用 webpack 语法，将代码和模块绑定在一起。应该在 `webpack.config.js` 中使用加载器语法。

    ```javascript
    // bad
    import fooSass from 'css!sass!foo.scss';
    import barCss from 'style!css!bar.css';

    // good
    import fooSass from 'foo.scss';
    import barCss from 'bar.css';
    ```

**[⬆ 返回目录](#table-of-contents)**

## <a id="iterators-and-generators">迭代器和发生器</a>

  <a name="iterators--nope"></a><a name="11.1"></a>
  - [11.1](#iterators--nope) 不要使用迭代器。 你应该使用 JavaScript 的高阶函数代替 `for-in` 或者 `for-of`。 eslint: [`no-iterator`](https://eslint.org/docs/rules/no-iterator.html) [`no-restricted-syntax`](https://eslint.org/docs/rules/no-restricted-syntax)

    > 为什么? 这是我们强制的规则。 拥有返回值得纯函数比这个更容易解释。

    > 使用 `map()` / `every()` / `filter()` / `find()` / `findIndex()` / `reduce()` / `some()` / ... 遍历数组， 和使用 `Object.keys()` / `Object.values()` / `Object.entries()` 迭代你的对象生成数组。

    ```javascript
    const numbers = [1, 2, 3, 4, 5];

    // bad
    let sum = 0;
    for (let num of numbers) {
      sum += num;
    }
    sum === 15;

    // good
    let sum = 0;
    numbers.forEach((num) => {
      sum += num;
    });
    sum === 15;

    // best (use the functional force)
    const sum = numbers.reduce((total, num) => total + num, 0);
    sum === 15;

    // bad
    const increasedByOne = [];
    for (let i = 0; i < numbers.length; i++) {
      increasedByOne.push(numbers[i] + 1);
    }

    // good
    const increasedByOne = [];
    numbers.forEach((num) => {
      increasedByOne.push(num + 1);
    });

    // best (keeping it functional)
    const increasedByOne = numbers.map(num => num + 1);
    ```

  <a name="generators--nope"></a><a name="11.2"></a>
  - [11.2](#generators--nope) 不要使用发生器。

    > 为什么? 它们不能很好的适应 ES5。

  <a name="generators--spacing"></a>
  - [11.3](#generators--spacing) 如果你必须使用发生器或者无视 [我们的建议](#generators--nope)，请确保他们的函数签名是正常的间隔。 eslint: [`generator-star-spacing`](https://eslint.org/docs/rules/generator-star-spacing)

    > 为什么? `function` 和 `*` 是同一个概念关键字的一部分 - `*` 不是 `function` 的修饰符， `function*` 是一个不同于 `function` 的构造器。

    ```javascript
    // bad
    function * foo() {
      // ...
    }

    // bad
    const bar = function * () {
      // ...
    };

    // bad
    const baz = function *() {
      // ...
    };

    // bad
    const quux = function*() {
      // ...
    };

    // bad
    function*foo() {
      // ...
    }

    // bad
    function *foo() {
      // ...
    }

    // very bad
    function
    *
    foo() {
      // ...
    }

    // very bad
    const wat = function
    *
    () {
      // ...
    };

    // good
    function* foo() {
      // ...
    }

    // good
    const foo = function* () {
      // ...
    };
    ```

**[⬆ 返回目录](#table-of-contents)**

## <a id="properties">属性</a>

  <a name="properties--dot"></a><a name="12.1"></a>
  - [12.1](#properties--dot) 访问属性时使用点符号。 eslint: [`dot-notation`](https://eslint.org/docs/rules/dot-notation.html)

    ```javascript
    const luke = {
      jedi: true,
      age: 28,
    };

    // bad
    const isJedi = luke['jedi'];

    // good
    const isJedi = luke.jedi;
    ```

  <a name="properties--bracket"></a><a name="12.2"></a>
  - [12.2](#properties--bracket) 使用变量访问属性时，使用 `[]`表示法。

    ```javascript
    const luke = {
      jedi: true,
      age: 28,
    };

    function getProp(prop) {
      return luke[prop];
    }

    const isJedi = getProp('jedi');
    ```
  <a name="es2016-properties--exponentiation-operator"></a>
  - [12.3](#es2016-properties--exponentiation-operator) 计算指数时，可以使用 `**` 运算符。 eslint: [`no-restricted-properties`](https://eslint.org/docs/rules/no-restricted-properties).

    ```javascript
    // bad
    const binary = Math.pow(2, 10);

    // good
    const binary = 2 ** 10;
    ```

**[⬆ 返回目录](#table-of-contents)**

## <a id="variables">变量</a>

  <a name="variables--const"></a><a name="13.1"></a>
  - [13.1](#variables--const) 使用 `const` 或者 `let` 来定义变量。 不这样做将创建一个全局变量。 我们希望避免污染全局命名空间。 Captain Planet 警告过我们。 eslint: [`no-undef`](https://eslint.org/docs/rules/no-undef) [`prefer-const`](https://eslint.org/docs/rules/prefer-const)

    ```javascript
    // bad
    superPower = new SuperPower();

    // good
    const superPower = new SuperPower();
    ```

  <a name="variables--one-const"></a><a name="13.2"></a>
  - [13.2](#variables--one-const) 使用 `const` 或者 `let` 声明每一个变量。 eslint: [`one-var`](https://eslint.org/docs/rules/one-var.html)

    > 为什么? 这样更容易添加新的变量声明，而且你不必担心是使用 `;` 还是使用 `,` 或引入标点符号的差别。 你可以通过 debugger 逐步查看每个声明，而不是立即跳过所有声明。

    ```javascript
    // bad
    const items = getItems(),
        goSportsTeam = true,
        dragonball = 'z';

    // bad
    // (compare to above, and try to spot the mistake)
    const items = getItems(),
        goSportsTeam = true;
        dragonball = 'z';

    // good
    const items = getItems();
    const goSportsTeam = true;
    const dragonball = 'z';
    ```

  <a name="variables--const-let-group"></a><a name="13.3"></a>
  - [13.3](#variables--const-let-group) 把 `const` 声明的放在一起，把 `let` 声明的放在一起。.

    > 为什么? 这在后边如果需要根据前边的赋值变量指定一个变量时很有用。

    ```javascript
    // bad
    let i, len, dragonball,
        items = getItems(),
        goSportsTeam = true;

    // bad
    let i;
    const items = getItems();
    let dragonball;
    const goSportsTeam = true;
    let len;

    // good
    const goSportsTeam = true;
    const items = getItems();
    let dragonball;
    let i;
    let length;
    ```

  <a name="variables--define-where-used"></a><a name="13.4"></a>
  - [13.4](#variables--define-where-used) 在你需要的使用定义变量，但是要把它们放在一个合理的地方。

    > 为什么? `let` 和 `const` 是块级作用域而不是函数作用域。

    ```javascript
    // bad - 不必要的函数调用
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

    // good
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
  <a name="variables--no-chain-assignment"></a><a name="13.5"></a>
  - [13.5](#variables--no-chain-assignment) 不要链式变量赋值。 eslint: [`no-multi-assign`](https://eslint.org/docs/rules/no-multi-assign)

    > 为什么? 链式变量赋值会创建隐式全局变量。

    ```javascript
    // bad
    (function example() {
      // JavaScript 把它解释为
      // let a = ( b = ( c = 1 ) );
      // let 关键词只适用于变量 a ；变量 b 和变量 c 则变成了全局变量。
      let a = b = c = 1;
    }());

    console.log(a); // throws ReferenceError
    console.log(b); // 1
    console.log(c); // 1

    // good
    (function example() {
      let a = 1;
      let b = a;
      let c = a;
    }());

    console.log(a); // throws ReferenceError
    console.log(b); // throws ReferenceError
    console.log(c); // throws ReferenceError

    // 对于 `const` 也一样
    ```

  <a name="variables--unary-increment-decrement"></a><a name="13.6"></a>
  - [13.6](#variables--unary-increment-decrement) 避免使用不必要的递增和递减 (`++`, `--`)。 eslint [`no-plusplus`](https://eslint.org/docs/rules/no-plusplus)

    > 为什么? 在eslint文档中，一元递增和递减语句以自动分号插入为主题，并且在应用程序中可能会导致默认值的递增或递减。它还可以用像 `num += 1` 这样的语句来改变您的值，而不是使用 `num++` 或 `num ++` 。不允许不必要的增量和减量语句也会使您无法预先递增/预递减值，这也会导致程序中的意外行为。

    ```javascript
    // bad

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

    // good

    const array = [1, 2, 3];
    let num = 1;
    num += 1;
    num -= 1;

    const sum = array.reduce((a, b) => a + b, 0);
    const truthyCount = array.filter(Boolean).length;
    ```

<a name="variables--linebreak"></a>
  - [13.7](#variables--linebreak) 避免在赋值语句 `=` 前后换行。如果你的代码违反了 [`max-len`](https://eslint.org/docs/rules/max-len.html)， 使用括号包裹。 eslint [`operator-linebreak`](https://eslint.org/docs/rules/operator-linebreak.html).

    > 为什么? 在 `=` 前后换行，可能混淆赋的值。

    ```javascript
    // bad
    const foo =
      superLongLongLongLongLongLongLongLongFunctionName();

    // bad
    const foo
      = 'superLongLongLongLongLongLongLongLongString';

    // good
    const foo = (
      superLongLongLongLongLongLongLongLongFunctionName()
    );

    // good
    const foo = 'superLongLongLongLongLongLongLongLongString';
    ```

**[⬆ 返回目录](#table-of-contents)**

## <a id="hoisting">提升</a>

  <a name="hoisting--about"></a><a name="14.1"></a>
  - [14.1](#hoisting--about) `var` 定义的变量会被提升到函数范围的最顶部，但是它的赋值不会。`const` 和 `let` 声明的变量受到一个称之为 [Temporal Dead Zones (TDZ)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let#Temporal_Dead_Zone_and_errors_with_let) 的新概念保护。 知道为什么 [typeof 不再安全](http://es-discourse.com/t/why-typeof-is-no-longer-safe/15) 是很重要的。

    ```javascript
    // 我们知道这个行不通 (假设没有未定义的全局变量)
    function example() {
      console.log(notDefined); // => throws a ReferenceError
    }

    // 在引用变量后创建变量声明将会因变量提升而起作用。
    // 注意: 真正的值 `true` 不会被提升。
    function example() {
      console.log(declaredButNotAssigned); // => undefined
      var declaredButNotAssigned = true;
    }

    // 解释器将变量提升到函数的顶部
    // 这意味着我们可以将上边的例子重写为：
    function example() {
      let declaredButNotAssigned;
      console.log(declaredButNotAssigned); // => undefined
      declaredButNotAssigned = true;
    }

    // 使用 const 和 let
    function example() {
      console.log(declaredButNotAssigned); // => throws a ReferenceError
      console.log(typeof declaredButNotAssigned); // => throws a ReferenceError
      const declaredButNotAssigned = true;
    }
    ```

  <a name="hoisting--anon-expressions"></a><a name="14.2"></a>
  - [14.2](#hoisting--anon-expressions) 匿名函数表达式提升变量名，而不是函数赋值。

    ```javascript
    function example() {
      console.log(anonymous); // => undefined

      anonymous(); // => TypeError anonymous is not a function

      var anonymous = function () {
        console.log('anonymous function expression');
      };
    }
    ```

  <a name="hoisting--named-expresions"></a><a name="hoisting--named-expressions"></a><a name="14.3"></a>
  - [14.3](#hoisting--named-expressions) 命名函数表达式提升的是变量名，而不是函数名或者函数体。

    ```javascript
    function example() {
      console.log(named); // => undefined

      named(); // => TypeError named is not a function

      superPower(); // => ReferenceError superPower is not defined

      var named = function superPower() {
        console.log('Flying');
      };
    }

    // 当函数名和变量名相同时也是如此。
    function example() {
      console.log(named); // => undefined

      named(); // => TypeError named is not a function

      var named = function named() {
        console.log('named');
      };
    }
    ```

  <a name="hoisting--declarations"></a><a name="14.4"></a>
  - [14.4](#hoisting--declarations) 函数声明提升其名称和函数体。

    ```javascript
    function example() {
      superPower(); // => Flying

      function superPower() {
        console.log('Flying');
      }
    }
    ```

  - 更多信息请参考 [Ben Cherry](http://www.adequatelygood.com/) 的 [JavaScript Scoping & Hoisting](http://www.adequatelygood.com/2010/2/JavaScript-Scoping-and-Hoisting/)。

**[⬆ 返回目录](#table-of-contents)**

## <a id="comparison-operators--equality">比较运算符和等号</a>

  <a name="comparison--eqeqeq"></a><a name="15.1"></a>
  - [15.1](#comparison--eqeqeq) 使用 `===` 和 `!==` 而不是 `==` 和 `!=`。 eslint: [`eqeqeq`](https://eslint.org/docs/rules/eqeqeq.html)

  <a name="comparison--if"></a><a name="15.2"></a>
  - [15.2](#comparison--if) 条件语句，例如 `if` 语句使用 `ToBoolean` 的抽象方法来计算表达式的结果，并始终遵循以下简单的规则：

    - **Objects** 的取值为： **true**
    - **Undefined** 的取值为： **false**
    - **Null** 的取值为： **false**
    - **Booleans** 的取值为： **布尔值的取值**
    - **Numbers** 的取值为：如果为 **+0, -0, or NaN** 值为 **false** 否则为 **true**
    - **Strings** 的取值为: 如果是一个空字符串 `''` 值为 **false** 否则为 **true**

    ```javascript
    if ([0] && []) {
      // true
      // 一个数组（即使是空的）是一个对象，对象的取值为 true
    }
    ```

  <a name="comparison--shortcuts"></a><a name="15.3"></a>
  - [15.3](#comparison--shortcuts) 对于布尔值使用简写，但是对于字符串和数字进行显式比较。

    ```javascript
    // bad
    if (isValid === true) {
      // ...
    }

    // good
    if (isValid) {
      // ...
    }

    // bad
    if (name) {
      // ...
    }

    // good
    if (name !== '') {
      // ...
    }

    // bad
    if (collection.length) {
      // ...
    }

    // good
    if (collection.length > 0) {
      // ...
    }
    ```

  <a name="comparison--moreinfo"></a><a name="15.4"></a>
  - [15.4](#comparison--moreinfo) 获取更多信息请查看 Angus Croll 的 [Truth Equality and JavaScript](https://javascriptweblog.wordpress.com/2011/02/07/truth-equality-and-javascript/#more-2108) 。

  <a name="comparison--switch-blocks"></a><a name="15.5"></a>
  - [15.5](#comparison--switch-blocks) 在 `case` 和 `default` 的子句中，如果存在声明 (例如. `let`, `const`, `function`, 和 `class`)，使用大括号来创建块 。 eslint: [`no-case-declarations`](https://eslint.org/docs/rules/no-case-declarations.html)

    > 为什么? 语法声明在整个 `switch` 块中都是可见的，但是只有在赋值的时候才会被初始化，这种情况只有在 `case` 条件达到才会发生。 当多个 `case` 语句定义相同的东西是，这会导致问题问题。

    ```javascript
    // bad
    switch (foo) {
      case 1:
        let x = 1;
        break;
      case 2:
        const y = 2;
        break;
      case 3:
        function f() {
          // ...
        }
        break;
      default:
        class C {}
    }

    // good
    switch (foo) {
      case 1: {
        let x = 1;
        break;
      }
      case 2: {
        const y = 2;
        break;
      }
      case 3: {
        function f() {
          // ...
        }
        break;
      }
      case 4:
        bar();
        break;
      default: {
        class C {}
      }
    }
    ```

  <a name="comparison--nested-ternaries"></a><a name="15.6"></a>
  - [15.6](#comparison--nested-ternaries) 三目表达式不应该嵌套，通常是单行表达式。 eslint: [`no-nested-ternary`](https://eslint.org/docs/rules/no-nested-ternary.html)

    ```javascript
    // bad
    const foo = maybe1 > maybe2
      ? "bar"
      : value1 > value2 ? "baz" : null;

    // 分离为两个三目表达式
    const maybeNull = value1 > value2 ? 'baz' : null;

    // better
    const foo = maybe1 > maybe2
      ? 'bar'
      : maybeNull;

    // best
    const foo = maybe1 > maybe2 ? 'bar' : maybeNull;
    ```

  <a name="comparison--unneeded-ternary"></a><a name="15.7"></a>
  - [15.7](#comparison--unneeded-ternary) 避免不必要的三目表达式。 eslint: [`no-unneeded-ternary`](https://eslint.org/docs/rules/no-unneeded-ternary.html)

    ```javascript
    // bad
    const foo = a ? a : b;
    const bar = c ? true : false;
    const baz = c ? false : true;

    // good
    const foo = a || b;
    const bar = !!c;
    const baz = !c;
    ```

  <a name="comparison--no-mixed-operators"></a>
  - [15.8](#comparison--no-mixed-operators) 使用该混合运算符时，使用括号括起来。 唯一例外的是标准算数运算符 (`+`, `-`, `*`, & `/`) 因为他们的优先级被广泛理解。 eslint: [`no-mixed-operators`](https://eslint.org/docs/rules/no-mixed-operators.html)

    > 为什么? 这能提高可读性并且表明开发人员的意图。

    ```javascript
    // bad
    const foo = a && b < 0 || c > 0 || d + 1 === 0;

    // bad
    const bar = a ** b - 5 % d;

    // bad
    // 可能陷入一种 (a || b) && c 的思考
    if (a || b && c) {
      return d;
    }

    // good
    const foo = (a && b < 0) || c > 0 || (d + 1 === 0);

    // good
    const bar = (a ** b) - (5 % d);

    // good
    if (a || (b && c)) {
      return d;
    }

    // good
    const bar = a + b / c * d;
    ```

**[⬆ 返回目录](#table-of-contents)**

## <a id="blocks">块</a>

  <a name="blocks--braces"></a><a name="16.1"></a>
  - [16.1](#blocks--braces) 当有多行代码块的时候，使用大括号包裹。 eslint: [`nonblock-statement-body-position`](https://eslint.org/docs/rules/nonblock-statement-body-position)

    ```javascript
    // bad
    if (test)
      return false;

    // good
    if (test) return false;

    // good
    if (test) {
      return false;
    }

    // bad
    function foo() { return false; }

    // good
    function bar() {
      return false;
    }
    ```

  <a name="blocks--cuddled-elses"></a><a name="16.2"></a>
  - [16.2](#blocks--cuddled-elses) 如果你使用的是 `if` 和 `else` 的多行代码块，则将 `else` 语句放在 `if` 块闭括号同一行的位置。 eslint: [`brace-style`](https://eslint.org/docs/rules/brace-style.html)

    ```javascript
    // bad
    if (test) {
      thing1();
      thing2();
    }
    else {
      thing3();
    }

    // good
    if (test) {
      thing1();
      thing2();
    } else {
      thing3();
    }
    ```

  <a name="blocks--no-else-return"></a><a name="16.3"></a>
  - [16.3](#blocks--no-else-return) 如果一个 `if` 块总是执行一个 `return` 语句，那么接下来的 `else` 块就没有必要了。 如果一个包含 `return` 语句的 `else if` 块，在一个包含了 `return` 语句的 `if` 块之后，那么可以拆成多个 `if` 块。 eslint: [`no-else-return`](https://eslint.org/docs/rules/no-else-return)

    ```javascript
    // bad
    function foo() {
      if (x) {
        return x;
      } else {
        return y;
      }
    }

    // bad
    function cats() {
      if (x) {
        return x;
      } else if (y) {
        return y;
      }
    }

    // bad
    function dogs() {
      if (x) {
        return x;
      } else {
        if (y) {
          return y;
        }
      }
    }

    // good
    function foo() {
      if (x) {
        return x;
      }

      return y;
    }

    // good
    function cats() {
      if (x) {
        return x;
      }

      if (y) {
        return y;
      }
    }

    // good
    function dogs(x) {
      if (x) {
        if (z) {
          return y;
        }
      } else {
        return z;
      }
    }
    ```

**[⬆ 返回目录](#table-of-contents)**

## <a id="control-statements">控制语句</a>

  <a name="control-statements"></a>
  - [17.1](#control-statements) 如果你的控制语句 (`if`, `while` 等) 太长或者超过了一行最大长度的限制，则可以将每个条件（或组）放入一个新的行。 逻辑运算符应该在行的开始。

    > 为什么? 要求操作符在行的开始保持对齐并遵循类似方法衔接的模式。 这提高了可读性，并且使更复杂的逻辑更容易直观的被理解。

    ```javascript
    // bad
    if ((foo === 123 || bar === 'abc') && doesItLookGoodWhenItBecomesThatLong() && isThisReallyHappening()) {
      thing1();
    }

    // bad
    if (foo === 123 &&
      bar === 'abc') {
      thing1();
    }

    // bad
    if (foo === 123
      && bar === 'abc') {
      thing1();
    }

    // bad
    if (
      foo === 123 &&
      bar === 'abc'
    ) {
      thing1();
    }

    // good
    if (
      foo === 123
      && bar === 'abc'
    ) {
      thing1();
    }

    // good
    if (
      (foo === 123 || bar === 'abc')
      && doesItLookGoodWhenItBecomesThatLong()
      && isThisReallyHappening()
    ) {
      thing1();
    }

    // good
    if (foo === 123 && bar === 'abc') {
      thing1();
    }
    ```

  <a name="control-statement--value-selection"></a>
  - [17.2](#control-statements--value-selection) 不要使用选择操作符代替控制语句。

    ```javascript
    // bad
    !isRunning && startRunning();

    // good
    if (!isRunning) {
      startRunning();
    }
    ```

**[⬆ 返回目录](#table-of-contents)**

## <a id="comments">注释</a>

  <a name="comments--multiline"></a><a name="17.1"></a>
  - [18.1](#comments--multiline) 使用 `/** ... */` 来进行多行注释。

    ```javascript
    // bad
    // make() returns a new element
    // based on the passed in tag name
    //
    // @param {String} tag
    // @return {Element} element
    function make(tag) {

      // ...

      return element;
    }

    // good
    /**
     * make() returns a new element
     * based on the passed-in tag name
     */
    function make(tag) {

      // ...

      return element;
    }
    ```

  <a name="comments--singleline"></a><a name="17.2"></a>
  - [18.2](#comments--singleline) 使用 `//` 进行单行注释。 将单行注释放在需要注释的行的上方新行。 在注释之前放一个空行，除非它在块的第一行。

    ```javascript
    // bad
    const active = true;  // is current tab

    // good
    // is current tab
    const active = true;

    // bad
    function getType() {
      console.log('fetching type...');
      // set the default type to 'no type'
      const type = this.type || 'no type';

      return type;
    }

    // good
    function getType() {
      console.log('fetching type...');

      // set the default type to 'no type'
      const type = this.type || 'no type';

      return type;
    }

    // also good
    function getType() {
      // set the default type to 'no type'
      const type = this.type || 'no type';

      return type;
    }
    ```

  <a name="comments--spaces"></a>
  - [18.3](#comments--spaces) 用一个空格开始所有的注释，使它更容易阅读。 eslint: [`spaced-comment`](https://eslint.org/docs/rules/spaced-comment)

    ```javascript
    // bad
    //is current tab
    const active = true;

    // good
    // is current tab
    const active = true;

    // bad
    /**
     *make() returns a new element
     *based on the passed-in tag name
     */
    function make(tag) {

      // ...

      return element;
    }

    // good
    /**
     * make() returns a new element
     * based on the passed-in tag name
     */
    function make(tag) {

      // ...

      return element;
    }
    ```

  <a name="comments--actionitems"></a><a name="17.3"></a>
  - [18.4](#comments--actionitems) 使用 `FIXME` 或者 `TODO` 开始你的注释可以帮助其他开发人员快速了解，如果你提出了一个需要重新审视的问题，或者你对需要实现的问题提出的解决方案。 这些不同于其他评论，因为他们是可操作的。 这些行为是 `FIXME: -- 需要解决这个问题` 或者 `TODO: -- 需要被实现`。

  <a name="comments--fixme"></a><a name="17.4"></a>
  - [18.5](#comments--fixme) 使用 `// FIXME:` 注释一个问题。

    ```javascript
    class Calculator extends Abacus {
      constructor() {
        super();

        // FIXME: 这里不应该使用全局变量
        total = 0;
      }
    }
    ```

  <a name="comments--todo"></a><a name="17.5"></a>
  - [18.6](#comments--todo) 使用 `// TODO:` 注释解决问题的方法。

    ```javascript
    class Calculator extends Abacus {
      constructor() {
        super();

        // TODO: total 应该由一个 param 的选项配置
        this.total = 0;
      }
    }
    ```

**[⬆ 返回目录](#table-of-contents)**

## <a id="whitespace">空白</a>

  <a name="whitespace--spaces"></a><a name="18.1"></a>
  - [19.1](#whitespace--spaces) 使用 tabs (空格字符) 设置为 2 个空格。 eslint: [`indent`](https://eslint.org/docs/rules/indent.html)

    ```javascript
    // bad
    function foo() {
    ∙∙∙∙let name;
    }

    // bad
    function bar() {
    ∙let name;
    }

    // good
    function baz() {
    ∙∙let name;
    }
    ```

  <a name="whitespace--before-blocks"></a><a name="18.2"></a>
  - [19.2](#whitespace--before-blocks) 在主体前放置一个空格。 eslint: [`space-before-blocks`](https://eslint.org/docs/rules/space-before-blocks.html)

    ```javascript
    // bad
    function test(){
      console.log('test');
    }

    // good
    function test() {
      console.log('test');
    }

    // bad
    dog.set('attr',{
      age: '1 year',
      breed: 'Bernese Mountain Dog',
    });

    // good
    dog.set('attr', {
      age: '1 year',
      breed: 'Bernese Mountain Dog',
    });
    ```

  <a name="whitespace--around-keywords"></a><a name="18.3"></a>
  - [19.3](#whitespace--around-keywords) 在控制语句（`if`, `while` 等）开始括号之前放置一个空格。 在函数调用和是声明中，在参数列表和函数名之间没有空格。 eslint: [`keyword-spacing`](https://eslint.org/docs/rules/keyword-spacing.html)

    ```javascript
    // bad
    if(isJedi) {
      fight ();
    }

    // good
    if (isJedi) {
      fight();
    }

    // bad
    function fight () {
      console.log ('Swooosh!');
    }

    // good
    function fight() {
      console.log('Swooosh!');
    }
    ```

  <a name="whitespace--infix-ops"></a><a name="18.4"></a>
  - [19.4](#whitespace--infix-ops) 用空格分离操作符。 eslint: [`space-infix-ops`](https://eslint.org/docs/rules/space-infix-ops.html)

    ```javascript
    // bad
    const x=y+5;

    // good
    const x = y + 5;
    ```

  <a name="whitespace--newline-at-end"></a><a name="18.5"></a>
  - [19.5](#whitespace--newline-at-end) 使用单个换行符结束文件。 eslint: [`eol-last`](https://github.com/eslint/eslint/blob/master/docs/rules/eol-last.md)

    ```javascript
    // bad
    import { es6 } from './AirbnbStyleGuide';
      // ...
    export default es6;
    ```

    ```javascript
    // bad
    import { es6 } from './AirbnbStyleGuide';
      // ...
    export default es6;↵
    ↵
    ```

    ```javascript
    // good
    import { es6 } from './AirbnbStyleGuide';
      // ...
    export default es6;↵
    ```

  <a name="whitespace--chains"></a><a name="18.6"></a>
  - [19.6](#whitespace--chains) 在使用链式方法调用的时候使用缩进(超过两个方法链)。 使用一个引导点，强调该行是方法调用，而不是新的语句。 eslint: [`newline-per-chained-call`](https://eslint.org/docs/rules/newline-per-chained-call) [`no-whitespace-before-property`](https://eslint.org/docs/rules/no-whitespace-before-property)

    ```javascript
    // bad
    $('#items').find('.selected').highlight().end().find('.open').updateCount();

    // bad
    $('#items').
      find('.selected').
        highlight().
        end().
      find('.open').
        updateCount();

    // good
    $('#items')
      .find('.selected')
        .highlight()
        .end()
      .find('.open')
        .updateCount();

    // bad
    const leds = stage.selectAll('.led').data(data).enter().append('svg:svg').classed('led', true)
        .attr('width', (radius + margin) * 2).append('svg:g')
        .attr('transform', `translate(${radius + margin},${radius + margin})`)
        .call(tron.led);

    // good
    const leds = stage.selectAll('.led')
        .data(data)
      .enter().append('svg:svg')
        .classed('led', true)
        .attr('width', (radius + margin) * 2)
      .append('svg:g')
        .attr('transform', `translate(${radius + margin},${radius + margin})`)
        .call(tron.led);

    // good
    const leds = stage.selectAll('.led').data(data);
    ```

  <a name="whitespace--after-blocks"></a><a name="18.7"></a>
  - [19.7](#whitespace--after-blocks) 在块和下一个语句之前留下一空白行。

    ```javascript
    // bad
    if (foo) {
      return bar;
    }
    return baz;

    // good
    if (foo) {
      return bar;
    }

    return baz;

    // bad
    const obj = {
      foo() {
      },
      bar() {
      },
    };
    return obj;

    // good
    const obj = {
      foo() {
      },

      bar() {
      },
    };

    return obj;

    // bad
    const arr = [
      function foo() {
      },
      function bar() {
      },
    ];
    return arr;

    // good
    const arr = [
      function foo() {
      },

      function bar() {
      },
    ];

    return arr;
    ```

  <a name="whitespace--padded-blocks"></a><a name="18.8"></a>
  - [19.8](#whitespace--padded-blocks) 不要在块的开头使用空白行。 eslint: [`padded-blocks`](https://eslint.org/docs/rules/padded-blocks.html)

    ```javascript
    // bad
    function bar() {

      console.log(foo);

    }

    // bad
    if (baz) {

      console.log(qux);
    } else {
      console.log(foo);

    }

    // bad
    class Foo {

      constructor(bar) {
        this.bar = bar;
      }
    }

    // good
    function bar() {
      console.log(foo);
    }

    // good
    if (baz) {
      console.log(qux);
    } else {
      console.log(foo);
    }
    ```

  <a name="whitespace--in-parens"></a><a name="18.9"></a>
  - [19.9](#whitespace--in-parens) 不要在括号内添加空格。 eslint: [`space-in-parens`](https://eslint.org/docs/rules/space-in-parens.html)

    ```javascript
    // bad
    function bar( foo ) {
      return foo;
    }

    // good
    function bar(foo) {
      return foo;
    }

    // bad
    if ( foo ) {
      console.log(foo);
    }

    // good
    if (foo) {
      console.log(foo);
    }
    ```

  <a name="whitespace--in-brackets"></a><a name="18.10"></a>
  - [19.10](#whitespace--in-brackets) 不要在中括号中添加空格。 eslint: [`array-bracket-spacing`](https://eslint.org/docs/rules/array-bracket-spacing.html)

    ```javascript
    // bad
    const foo = [ 1, 2, 3 ];
    console.log(foo[ 0 ]);

    // good
    const foo = [1, 2, 3];
    console.log(foo[0]);
    ```

  <a name="whitespace--in-braces"></a><a name="18.11"></a>
  - [19.11](#whitespace--in-braces) 在花括号内添加空格。 eslint: [`object-curly-spacing`](https://eslint.org/docs/rules/object-curly-spacing.html)

    ```javascript
    // bad
    const foo = {clark: 'kent'};

    // good
    const foo = { clark: 'kent' };
    ```

  <a name="whitespace--max-len"></a><a name="18.12"></a>
  - [19.12](#whitespace--max-len) 避免让你的代码行超过100个字符（包括空格）。 注意：根据上边的 [约束](#strings--line-length)，长字符串可免除此规定，不应分解。 eslint: [`max-len`](https://eslint.org/docs/rules/max-len.html)

    > 为什么? 这样能够确保可读性和可维护性。

    ```javascript
    // bad
    const foo = jsonData && jsonData.foo && jsonData.foo.bar && jsonData.foo.bar.baz && jsonData.foo.bar.baz.quux && jsonData.foo.bar.baz.quux.xyzzy;

    // bad
    $.ajax({ method: 'POST', url: 'https://airbnb.com/', data: { name: 'John' } }).done(() => console.log('Congratulations!')).fail(() => console.log('You have failed this city.'));

    // good
    const foo = jsonData
      && jsonData.foo
      && jsonData.foo.bar
      && jsonData.foo.bar.baz
      && jsonData.foo.bar.baz.quux
      && jsonData.foo.bar.baz.quux.xyzzy;

    // good
    $.ajax({
      method: 'POST',
      url: 'https://airbnb.com/',
      data: { name: 'John' },
    })
      .done(() => console.log('Congratulations!'))
      .fail(() => console.log('You have failed this city.'));
    ```

  <a name="whitespace--block-spacing"></a>
  - [19.13](#whitespace--block-spacing) 要求打开的块标志和同一行上的标志拥有一致的间距。此规则还会在同一行关闭的块标记和前边的标记强制实施一致的间距。 eslint: [`block-spacing`](https://eslint.org/docs/rules/block-spacing)

    ```javascript
    // bad
    function foo() {return true;}
    if (foo) { bar = 0;}

    // good
    function foo() { return true; }
    if (foo) { bar = 0; }
    ```

  <a name="whitespace--comma-spacing"></a>
  - [19.14](#whitespace--comma-spacing) 逗号之前避免使用空格，逗号之后需要使用空格。eslint: [`comma-spacing`](https://eslint.org/docs/rules/comma-spacing)

    ```javascript
    // bad
    var foo = 1,bar = 2;
    var arr = [1 , 2];

    // good
    var foo = 1, bar = 2;
    var arr = [1, 2];
    ```

  <a name="whitespace--computed-property-spacing"></a>
  - [19.15](#whitespace--computed-property-spacing) 在计算属性之间强化间距。eslint: [`computed-property-spacing`](https://eslint.org/docs/rules/computed-property-spacing)

    ```javascript
    // bad
    obj[foo ]
    obj[ 'foo']
    var x = {[ b ]: a}
    obj[foo[ bar ]]

    // good
    obj[foo]
    obj['foo']
    var x = { [b]: a }
    obj[foo[bar]]
    ```

  <a name="whitespace--func-call-spacing"></a>
  - [19.16](#whitespace--func-call-spacing) 在函数和它的调用之间强化间距。 eslint: [`func-call-spacing`](https://eslint.org/docs/rules/func-call-spacing)

    ```javascript
    // bad
    func ();

    func
    ();

    // good
    func();
    ```

  <a name="whitespace--key-spacing"></a>
  - [19.17](#whitespace--key-spacing) 在对象的属性和值之间强化间距。 eslint: [`key-spacing`](https://eslint.org/docs/rules/key-spacing)

    ```javascript
    // bad
    var obj = { "foo" : 42 };
    var obj2 = { "foo":42 };

    // good
    var obj = { "foo": 42 };
    ```

  <a name="whitespace--no-trailing-spaces"></a>
  - [19.18](#whitespace--no-trailing-spaces) 在行的末尾避免使用空格。 eslint: [`no-trailing-spaces`](https://eslint.org/docs/rules/no-trailing-spaces)

  <a name="whitespace--no-multiple-empty-lines"></a>
  - [19.19](#whitespace--no-multiple-empty-lines) 避免多个空行，并且只允许在文件末尾添加一个换行符。 eslint: [`no-multiple-empty-lines`](https://eslint.org/docs/rules/no-multiple-empty-lines)

    <!-- markdownlint-disable MD012 -->
    ```javascript
    // bad
    var x = 1;



    var y = 2;

    // good
    var x = 1;

    var y = 2;
    ```
    <!-- markdownlint-enable MD012 -->

**[⬆ 返回目录](#table-of-contents)**

## <a id="commas">逗号</a>

  <a name="commas--leading-trailing"></a><a name="19.1"></a>
  - [20.1](#commas--leading-trailing) 逗号前置： **不行** eslint: [`comma-style`](https://eslint.org/docs/rules/comma-style.html)

    ```javascript
    // bad
    const story = [
        once
      , upon
      , aTime
    ];

    // good
    const story = [
      once,
      upon,
      aTime,
    ];

    // bad
    const hero = {
        firstName: 'Ada'
      , lastName: 'Lovelace'
      , birthYear: 1815
      , superPower: 'computers'
    };

    // good
    const hero = {
      firstName: 'Ada',
      lastName: 'Lovelace',
      birthYear: 1815,
      superPower: 'computers',
    };
    ```

  <a name="commas--dangling"></a><a name="19.2"></a>
  - [20.2](#commas--dangling) 添加尾随逗号： **可以** eslint: [`comma-dangle`](https://eslint.org/docs/rules/comma-dangle.html)

    > 为什么? 这个将造成更清洁的 git 扩展差异。 另外，像 Babel 这样的编译器，会在转换后的代码中删除额外的尾随逗号，这意味着你不必担心在浏览器中后面的 [尾随逗号问题](https://github.com/airbnb/javascript/blob/es5-deprecated/es5/README.md#commas) 。

    ```diff
    // bad - 没有尾随逗号的 git 差异
    const hero = {
         firstName: 'Florence',
    -    lastName: 'Nightingale'
    +    lastName: 'Nightingale',
    +    inventorOf: ['coxcomb chart', 'modern nursing']
    };

    // good - 有尾随逗号的 git 差异
    const hero = {
         firstName: 'Florence',
         lastName: 'Nightingale',
    +    inventorOf: ['coxcomb chart', 'modern nursing'],
    };
    ```

    ```javascript
    // bad
    const hero = {
      firstName: 'Dana',
      lastName: 'Scully'
    };

    const heroes = [
      'Batman',
      'Superman'
    ];

    // good
    const hero = {
      firstName: 'Dana',
      lastName: 'Scully',
    };

    const heroes = [
      'Batman',
      'Superman',
    ];

    // bad
    function createHero(
      firstName,
      lastName,
      inventorOf
    ) {
      // does nothing
    }

    // good
    function createHero(
      firstName,
      lastName,
      inventorOf,
    ) {
      // does nothing
    }

    // good (注意逗号不能出现在 "rest" 元素后边)
    function createHero(
      firstName,
      lastName,
      inventorOf,
      ...heroArgs
    ) {
      // does nothing
    }

    // bad
    createHero(
      firstName,
      lastName,
      inventorOf
    );

    // good
    createHero(
      firstName,
      lastName,
      inventorOf,
    );

    // good (注意逗号不能出现在 "rest" 元素后边)
    createHero(
      firstName,
      lastName,
      inventorOf,
      ...heroArgs
    );
    ```

**[⬆ 返回目录](#table-of-contents)**

## <a id="semicolons">分号</a>

  <a name="semicolons--required"></a><a name="20.1"></a>
  - [21.1](#semicolons--required) **对** eslint: [`semi`](https://eslint.org/docs/rules/semi.html)

    > 为什么? 当 JavaScript 遇见一个没有分号的换行符时，它会使用一个叫做 [Automatic Semicolon Insertion](https://tc39.github.io/ecma262/#sec-automatic-semicolon-insertion) 的规则来确定是否应该以换行符视为语句的结束，并且如果认为如此，会在代码中断前插入一个分号到代码中。 但是，ASI 包含了一些奇怪的行为，如果 JavaScript 错误的解释了你的换行符，你的代码将会中断。 随着新特性成为 JavaScript 的一部分，这些规则将变得更加复杂。 明确地终止你的语句，并配置你的 linter 以捕获缺少的分号将有助于防止你遇到的问题。

    ```javascript
    // bad - 可能异常
    const luke = {}
    const leia = {}
    [luke, leia].forEach(jedi => jedi.father = 'vader')

    // bad - 可能异常
    const reaction = "No! That's impossible!"
    (async function meanwhileOnTheFalcon() {
      // handle `leia`, `lando`, `chewie`, `r2`, `c3p0`
      // ...
    }())

    // bad - 返回 `undefined` 而不是下一行的值 - 当 `return` 单独一行的时候 ASI 总是会发生
    function foo() {
      return
        'search your feelings, you know it to be foo'
    }

    // good
    const luke = {};
    const leia = {};
    [luke, leia].forEach((jedi) => {
      jedi.father = 'vader';
    });

    // good
    const reaction = "No! That's impossible!";
    (async function meanwhileOnTheFalcon() {
      // handle `leia`, `lando`, `chewie`, `r2`, `c3p0`
      // ...
    }());

    // good
    function foo() {
      return 'search your feelings, you know it to be foo';
    }
    ```

    [更多信息](https://stackoverflow.com/questions/7365172/semicolon-before-self-invoking-function/7365214#7365214).

**[⬆ 返回目录](#table-of-contents)**

## <a id="type-casting--coercion">类型转换和强制类型转换</a>

  <a name="coercion--explicit"></a><a name="21.1"></a>
  - [22.1](#coercion--explicit) 在语句开始前进行类型转换。

  <a name="coercion--strings"></a><a name="21.2"></a>
  - [22.2](#coercion--strings)  字符类型： eslint: [`no-new-wrappers`](https://eslint.org/docs/rules/no-new-wrappers)

    ```javascript
    // => this.reviewScore = 9;

    // bad
    const totalScore = new String(this.reviewScore); // typeof totalScore is "object" not "string"

    // bad
    const totalScore = this.reviewScore + ''; // invokes this.reviewScore.valueOf()

    // bad
    const totalScore = this.reviewScore.toString(); // isn’t guaranteed to return a string

    // good
    const totalScore = String(this.reviewScore);
    ```

  <a name="coercion--numbers"></a><a name="21.3"></a>
  - [22.3](#coercion--numbers) 数字类型：使用 `Number` 进行类型铸造和 `parseInt` 总是通过一个基数来解析一个字符串。 eslint: [`radix`](https://eslint.org/docs/rules/radix) [`no-new-wrappers`](https://eslint.org/docs/rules/no-new-wrappers)

    ```javascript
    const inputValue = '4';

    // bad
    const val = new Number(inputValue);

    // bad
    const val = +inputValue;

    // bad
    const val = inputValue >> 0;

    // bad
    const val = parseInt(inputValue);

    // good
    const val = Number(inputValue);

    // good
    const val = parseInt(inputValue, 10);
    ```

  <a name="coercion--comment-deviations"></a><a name="21.4"></a>
  - [22.4](#coercion--comment-deviations) 如果出于某种原因，你正在做一些疯狂的事情，而 `parseInt` 是你的瓶颈，并且出于 [性能问题](https://jsperf.com/coercion-vs-casting/3) 需要使用位运算， 请写下注释，说明为什么这样做和你做了什么。

    ```javascript
    // good
    /**
     * parseInt 使我的代码变慢。
     * 位运算将一个字符串转换成数字更快。
     */
    const val = inputValue >> 0;
    ```

  <a name="coercion--bitwise"></a><a name="21.5"></a>
  - [22.5](#coercion--bitwise) **注意：** 当你使用位运算的时候要小心。 数字总是被以 [64-bit 值](https://es5.github.io/#x4.3.19) 的形式表示，但是位运算总是返回一个 32-bit 的整数 ([来源](https://es5.github.io/#x11.7))。 对于大于 32 位的整数值，位运算可能会导致意外行为。[讨论](https://github.com/airbnb/javascript/issues/109)。 最大的 32 位整数是： 2,147,483,647。

    ```javascript
    2147483647 >> 0; // => 2147483647
    2147483648 >> 0; // => -2147483648
    2147483649 >> 0; // => -2147483647
    ```

  <a name="coercion--booleans"></a><a name="21.6"></a>
  - [22.6](#coercion--booleans) 布尔类型： eslint: [`no-new-wrappers`](https://eslint.org/docs/rules/no-new-wrappers)

    ```javascript
    const age = 0;

    // bad
    const hasAge = new Boolean(age);

    // good
    const hasAge = Boolean(age);

    // best
    const hasAge = !!age;
    ```

**[⬆ 返回目录](#table-of-contents)**

## <a id="naming-conventions">命名规范</a>

  <a name="naming--descriptive"></a><a name="22.1"></a>
  - [23.1](#naming--descriptive) 避免单字母的名字。用你的命名来描述功能。 eslint: [`id-length`](https://eslint.org/docs/rules/id-length)

    ```javascript
    // bad
    function q() {
      // ...
    }

    // good
    function query() {
      // ...
    }
    ```

  <a name="naming--camelCase"></a><a name="22.2"></a>
  - [23.2](#naming--camelCase) 在命名对象、函数和实例时使用驼峰命名法（camelCase）。 eslint: [`camelcase`](https://eslint.org/docs/rules/camelcase.html)

    ```javascript
    // bad
    const OBJEcttsssss = {};
    const this_is_my_object = {};
    function c() {}

    // good
    const thisIsMyObject = {};
    function thisIsMyFunction() {}
    ```

  <a name="naming--PascalCase"></a><a name="22.3"></a>
  - [23.3](#naming--PascalCase) 只有在命名构造器或者类的时候才用帕斯卡拼命名法（PascalCase）。 eslint: [`new-cap`](https://eslint.org/docs/rules/new-cap.html)

    ```javascript
    // bad
    function user(options) {
      this.name = options.name;
    }

    const bad = new user({
      name: 'nope',
    });

    // good
    class User {
      constructor(options) {
        this.name = options.name;
      }
    }

    const good = new User({
      name: 'yup',
    });
    ```

  <a name="naming--leading-underscore"></a><a name="22.4"></a>
  - [23.4](#naming--leading-underscore) 不要使用前置或者后置下划线。 eslint: [`no-underscore-dangle`](https://eslint.org/docs/rules/no-underscore-dangle.html)

    > 为什么? JavaScript 在属性和方法方面没有隐私设置。 虽然前置的下划线是一种常见的惯例，意思是 “private” ，事实上，这些属性时公开的，因此，它们也是你公共 API 的一部分。 这种约定可能导致开发人员错误的认为更改不会被视为中断，或者不需要测试。建议：如果你想要什么东西是 “private” ， 那就一定不能有明显的表现。

    ```javascript
    // bad
    this.__firstName__ = 'Panda';
    this.firstName_ = 'Panda';
    this._firstName = 'Panda';

    // good
    this.firstName = 'Panda';

    // 好，在 WeakMapx 可用的环境中
    // see https://kangax.github.io/compat-table/es6/#test-WeakMap
    const firstNames = new WeakMap();
    firstNames.set(this, 'Panda');
    ```

  <a name="naming--self-this"></a><a name="22.5"></a>
  - [23.5](#naming--self-this) 不要保存 `this` 的引用。 使用箭头函数或者 [函数#bind](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/bind)。

    ```javascript
    // bad
    function foo() {
      const self = this;
      return function () {
        console.log(self);
      };
    }

    // bad
    function foo() {
      const that = this;
      return function () {
        console.log(that);
      };
    }

    // good
    function foo() {
      return () => {
        console.log(this);
      };
    }
    ```

  <a name="naming--filename-matches-export"></a><a name="22.6"></a>
  - [23.6](#naming--filename-matches-export) 文件名应该和默认导出的名称完全匹配。

    ```javascript
    // file 1 contents
    class CheckBox {
      // ...
    }
    export default CheckBox;

    // file 2 contents
    export default function fortyTwo() { return 42; }

    // file 3 contents
    export default function insideDirectory() {}

    // in some other file
    // bad
    import CheckBox from './checkBox'; // PascalCase import/export, camelCase filename
    import FortyTwo from './FortyTwo'; // PascalCase import/filename, camelCase export
    import InsideDirectory from './InsideDirectory'; // PascalCase import/filename, camelCase export

    // bad
    import CheckBox from './check_box'; // PascalCase import/export, snake_case filename
    import forty_two from './forty_two'; // snake_case import/filename, camelCase export
    import inside_directory from './inside_directory'; // snake_case import, camelCase export
    import index from './inside_directory/index'; // requiring the index file explicitly
    import insideDirectory from './insideDirectory/index'; // requiring the index file explicitly

    // good
    import CheckBox from './CheckBox'; // PascalCase export/import/filename
    import fortyTwo from './fortyTwo'; // camelCase export/import/filename
    import insideDirectory from './insideDirectory'; // camelCase export/import/directory name/implicit "index"
    // ^ supports both insideDirectory.js and insideDirectory/index.js
    ```

  <a name="naming--camelCase-default-export"></a><a name="22.7"></a>
  - [23.7](#naming--camelCase-default-export) 当你导出默认函数时使用驼峰命名法。 你的文件名应该和方法名相同。

    ```javascript
    function makeStyleGuide() {
      // ...
    }

    export default makeStyleGuide;
    ```

  <a name="naming--PascalCase-singleton"></a><a name="22.8"></a>
  - [23.8](#naming--PascalCase-singleton) 当你导出一个构造器 / 类 / 单例 / 函数库 / 暴露的对象时应该使用帕斯卡命名法。

    ```javascript
    const AirbnbStyleGuide = {
      es6: {
      },
    };

    export default AirbnbStyleGuide;
    ```

  <a name="naming--Acronyms-and-Initialisms"></a>
  - [23.9](#naming--Acronyms-and-Initialisms) 缩略词和缩写都必须是全部大写或者全部小写。

    > 为什么? 名字是为了可读性，不是为了满足计算机算法。

    ```javascript
    // bad
    import SmsContainer from './containers/SmsContainer';

    // bad
    const HttpRequests = [
      // ...
    ];

    // good
    import SMSContainer from './containers/SMSContainer';

    // good
    const HTTPRequests = [
      // ...
    ];

    // also good
    const httpRequests = [
      // ...
    ];

    // best
    import TextMessageContainer from './containers/TextMessageContainer';

    // best
    const requests = [
      // ...
    ];
    ```

  <a name="naming--uppercase"></a>
  - [23.10](#naming--uppercase) 你可以大写一个常量，如果它：（1）被导出，（2）使用 `const` 定义（不能被重新赋值），（3）程序员可以信任它（以及其嵌套的属性）是不变的。

    > 为什么? 这是一个可以帮助程序员确定变量是否会发生变化的辅助工具。UPPERCASE_VARIABLES 可以让程序员知道他们可以相信变量（及其属性）不会改变。
    - 是否是对所有的 `const` 定义的变量？ - 这个是没有必要的，不应该在文件中使用大写。但是，它应该用于导出常量。
    - 导出对象呢？ - 在顶级导出属性 (e.g. `EXPORTED_OBJECT.key`) 并且保持所有嵌套属性不变。

    ```javascript
    // bad
    const PRIVATE_VARIABLE = 'should not be unnecessarily uppercased within a file';

    // bad
    export const THING_TO_BE_CHANGED = 'should obviously not be uppercased';

    // bad
    export let REASSIGNABLE_VARIABLE = 'do not use let with uppercase variables';

    // ---

    // 允许，但是不提供语义值
    export const apiKey = 'SOMEKEY';

    // 多数情况下，很好
    export const API_KEY = 'SOMEKEY';

    // ---

    // bad - 不必要大写 key 没有增加语义值
    export const MAPPING = {
      KEY: 'value'
    };

    // good
    export const MAPPING = {
      key: 'value'
    };
    ```

**[⬆ 返回目录](#table-of-contents)**

## <a id="accessors">存取器</a>

  <a name="accessors--not-required"></a><a name="23.1"></a>
  - [24.1](#accessors--not-required) 对于属性的的存取函数不是必须的。

  <a name="accessors--no-getters-setters"></a><a name="23.2"></a>
  - [24.2](#accessors--no-getters-setters) 不要使用 JavaScript 的 getters/setters 方法，因为它们会导致意外的副作用，并且更加难以测试、维护和推敲。 相应的，如果你需要存取函数的时候使用 `getVal()` 和 `setVal('hello')`。

    ```javascript
    // bad
    class Dragon {
      get age() {
        // ...
      }

      set age(value) {
        // ...
      }
    }

    // good
    class Dragon {
      getAge() {
        // ...
      }

      setAge(value) {
        // ...
      }
    }
    ```

  <a name="accessors--boolean-prefix"></a><a name="23.3"></a>
  - [24.3](#accessors--boolean-prefix) 如果属性/方法是一个 `boolean` 值，使用 `isVal()` 或者 `hasVal()`。

    ```javascript
    // bad
    if (!dragon.age()) {
      return false;
    }

    // good
    if (!dragon.hasAge()) {
      return false;
    }
    ```

  <a name="accessors--consistent"></a><a name="23.4"></a>
  - [24.4](#accessors--consistent) 可以创建 `get()` 和 `set()` 方法，但是要保证一致性。

    ```javascript
    class Jedi {
      constructor(options = {}) {
        const lightsaber = options.lightsaber || 'blue';
        this.set('lightsaber', lightsaber);
      }

      set(key, val) {
        this[key] = val;
      }

      get(key) {
        return this[key];
      }
    }
    ```

**[⬆ 返回目录](#table-of-contents)**

## <a id="events">事件</a>

  <a name="events--hash"></a><a name="24.1"></a>
  - [25.1](#events--hash) 当给事件（无论是 DOM 事件还是更加私有的事件）附加数据时，传入一个对象（通畅也叫做 “hash” ） 而不是原始值。 这样可以让后边的贡献者向事件数据添加更多的数据，而不用找出更新事件的每个处理器。 例如，不好的写法：

    ```javascript
    // bad
    $(this).trigger('listingUpdated', listing.id);

    // ...

    $(this).on('listingUpdated', (e, listingID) => {
      // do something with listingID
    });
    ```

    更好的写法：

    ```javascript
    // good
    $(this).trigger('listingUpdated', { listingID: listing.id });

    // ...

    $(this).on('listingUpdated', (e, data) => {
      // do something with data.listingID
    });
    ```

  **[⬆ 返回目录](#table-of-contents)**

## jQuery

  <a name="jquery--dollar-prefix"></a><a name="25.1"></a>
  - [26.1](#jquery--dollar-prefix) 对于 jQuery 对象的变量使用 `$` 作为前缀。

    ```javascript
    // bad
    const sidebar = $('.sidebar');

    // good
    const $sidebar = $('.sidebar');

    // good
    const $sidebarBtn = $('.sidebar-btn');
    ```

  <a name="jquery--cache"></a><a name="25.2"></a>
  - [26.2](#jquery--cache) 缓存 jQuery 查询。

    ```javascript
    // bad
    function setSidebar() {
      $('.sidebar').hide();

      // ...

      $('.sidebar').css({
        'background-color': 'pink',
      });
    }

    // good
    function setSidebar() {
      const $sidebar = $('.sidebar');
      $sidebar.hide();

      // ...

      $sidebar.css({
        'background-color': 'pink',
      });
    }
    ```

  <a name="jquery--queries"></a><a name="25.3"></a>
  - [26.3](#jquery--queries) 对于 DOM 查询使用层叠 `$('.sidebar ul')` 或 父元素 > 子元素 `$('.sidebar > ul')` 的格式。 [jsPerf](http://jsperf.com/jquery-find-vs-context-sel/16)

  <a name="jquery--find"></a><a name="25.4"></a>
  - [26.4](#jquery--find) 对于有作用域的 jQuery 对象查询使用 `find` 。

    ```javascript
    // bad
    $('ul', '.sidebar').hide();

    // bad
    $('.sidebar').find('ul').hide();

    // good
    $('.sidebar ul').hide();

    // good
    $('.sidebar > ul').hide();

    // good
    $sidebar.find('ul').hide();
    ```

**[⬆ 返回目录](#table-of-contents)**

## <a id="ecmascript-5-compatibility">ECMAScript 5 兼容性</a>

  <a name="es5-compat--kangax"></a><a name="26.1"></a>
  - [27.1](#es5-compat--kangax) 参考 [Kangax](https://twitter.com/kangax/)的 ES5 [兼容性表格](https://kangax.github.io/es5-compat-table/)。

**[⬆ 返回目录](#table-of-contents)**

<a name="ecmascript-6-styles"></a>
## ECMAScript 6+ (ES 2015+) Styles

  <a name="es6-styles"></a><a name="27.1"></a>
  - [28.1](#es6-styles) 这是一个链接到各种 ES6+ 特性的集合。

1. [箭头函数](#arrow-functions)
1. [类](#classes--constructors)
1. [对象简写](#es6-object-shorthand)
1. [对象简洁](#es6-object-concise)
1. [对象计算属性](#es6-computed-properties)
1. [字符串模板](#es6-template-literals)
1. [解构](#destructuring)
1. [默认参数](#es6-default-parameters)
1. [Rest](#es6-rest)
1. [数组展开](#es6-array-spreads)
1. [Let 和 Const](#references)
1. [求幂运算符](#es2016-properties--exponentiation-operator)
1. [迭代器和发生器](#iterators-and-generators)
1. [模块](#modules)

  <a name="tc39-proposals"></a>
  - [28.2](#tc39-proposals) 不要使用尚未达到第3阶段的 [TC39 建议](https://github.com/tc39/proposals)。

    > 为什么? [它们没有最终确定](https://tc39.github.io/process-document/)， 并且它们可能会被改变或完全撤回。我们希望使用JavaScript，而建议还不是JavaScript。

**[⬆ 返回目录](#table-of-contents)**

## <a id="standard-library">标准库</a>

  [标准库](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects)
  包含功能已损坏的实用工具，但因为遗留原因而保留。

  <a name="standard-library--isnan"></a>
  - [29.1](#standard-library--isnan) 使用 `Number.isNaN` 代替全局的 `isNaN`.
    eslint: [`no-restricted-globals`](https://eslint.org/docs/rules/no-restricted-globals)

    > 为什么? 全局的 `isNaN` 强制非数字转化为数字，对任何强制转化为 NaN 的东西都返回 true。

    > 如果需要这种行为，请明确说明。

    ```javascript
    // bad
    isNaN('1.2'); // false
    isNaN('1.2.3'); // true

    // good
    Number.isNaN('1.2.3'); // false
    Number.isNaN(Number('1.2.3')); // true
    ```

  <a name="standard-library--isfinite"></a>
  - [29.2](#standard-library--isfinite) 使用 `Number.isFinite` 代替全局的 `isFinite`.
    eslint: [`no-restricted-globals`](https://eslint.org/docs/rules/no-restricted-globals)

    > 为什么? 全局的 `isFinite` 强制非数字转化为数字，对任何强制转化为有限数字的东西都返回 true。

    > 如果需要这种行为，请明确说明。

    ```javascript
    // bad
    isFinite('2e3'); // true

    // good
    Number.isFinite('2e3'); // false
    Number.isFinite(parseInt('2e3', 10)); // true
    ```

**[⬆ 返回目录](#table-of-contents)**

## Testing

  <a name="testing--yup"></a><a name="28.1"></a>
  - [30.1](#testing--yup) **是的.**

    ```javascript
    function foo() {
      return true;
    }
    ```

  <a name="testing--for-real"></a><a name="28.2"></a>
  - [30.2](#testing--for-real) **没有，但是认真**:
    - 无论你使用那种测试框架，都应该编写测试！
    - 努力写出许多小的纯函数，并尽量减少发生错误的地方。
    - 对于静态方法和 mock 要小心----它们会使你的测试更加脆弱。
    - 我们主要在 Airbnb 上使用 [`mocha`](https://www.npmjs.com/package/mocha) 和 [`jest`](https://www.npmjs.com/package/jest) 。 [`tape`](https://www.npmjs.com/package/tape) 也会用在一些小的独立模块上。
    - 100%的测试覆盖率是一个很好的目标，即使它并不总是可行的。
    - 无论何时修复bug，都要编写一个回归测试。在没有回归测试的情况下修复的bug在将来几乎肯定会再次崩溃。

**[⬆ 返回目录](#table-of-contents)**

## <a id="performance">性能</a>

  - [On Layout & Web Performance](https://www.kellegous.com/j/2013/01/26/layout-performance/)
  - [String vs Array Concat](https://jsperf.com/string-vs-array-concat/2)
  - [Try/Catch Cost In a Loop](https://jsperf.com/try-catch-in-loop-cost)
  - [Bang Function](https://jsperf.com/bang-function)
  - [jQuery Find vs Context, Selector](https://jsperf.com/jquery-find-vs-context-sel/13)
  - [innerHTML vs textContent for script text](https://jsperf.com/innerhtml-vs-textcontent-for-script-text)
  - [Long String Concatenation](https://jsperf.com/ya-string-concat)
  - [Are Javascript functions like `map()`, `reduce()`, and `filter()` optimized for traversing arrays?](https://www.quora.com/JavaScript-programming-language-Are-Javascript-functions-like-map-reduce-and-filter-already-optimized-for-traversing-array/answer/Quildreen-Motta)
  - Loading...

**[⬆ 返回目录](#table-of-contents)**

## <a id="resources">资源</a>

**学习 ES6+**

  - [Latest ECMA spec](https://tc39.github.io/ecma262/)
  - [ExploringJS](http://exploringjs.com/)
  - [ES6 Compatibility Table](https://kangax.github.io/compat-table/es6/)
  - [Comprehensive Overview of ES6 Features](http://es6-features.org/)

**读这个**

  - [Standard ECMA-262](http://www.ecma-international.org/ecma-262/6.0/index.html)

**工具**

  - Code Style Linters
    - [ESlint](https://eslint.org/) - [Airbnb Style .eslintrc](https://github.com/airbnb/javascript/blob/master/linters/.eslintrc)
    - [JSHint](http://jshint.com/) - [Airbnb Style .jshintrc](https://github.com/airbnb/javascript/blob/master/linters/.jshintrc)
  - Neutrino preset - [neutrino-preset-airbnb-base](https://neutrino.js.org/presets/neutrino-preset-airbnb-base/)

**其他编码规范**

  - [Google JavaScript Style Guide](https://google.github.io/styleguide/javascriptguide.xml)
  - [jQuery Core Style Guidelines](https://contribute.jquery.org/style-guide/js/)
  - [Principles of Writing Consistent, Idiomatic JavaScript](https://github.com/rwaldron/idiomatic.js)
  - [StandardJS](https://standardjs.com)

**其他风格**

  - [Naming this in nested functions](https://gist.github.com/cjohansen/4135065) - Christian Johansen
  - [Conditional Callbacks](https://github.com/airbnb/javascript/issues/52) - Ross Allen
  - [Popular JavaScript Coding Conventions on GitHub](http://sideeffect.kr/popularconvention/#javascript) - JeongHoon Byun
  - [Multiple var statements in JavaScript, not superfluous](http://benalman.com/news/2012/05/multiple-var-statements-javascript/) - Ben Alman

**进一步阅读**

  - [Understanding JavaScript Closures](https://javascriptweblog.wordpress.com/2010/10/25/understanding-javascript-closures/) - Angus Croll
  - [Basic JavaScript for the impatient programmer](http://www.2ality.com/2013/06/basic-javascript.html) - Dr. Axel Rauschmayer
  - [You Might Not Need jQuery](http://youmightnotneedjquery.com/) - Zack Bloom & Adam Schwartz
  - [ES6 Features](https://github.com/lukehoban/es6features) - Luke Hoban
  - [Frontend Guidelines](https://github.com/bendc/frontend-guidelines) - Benjamin De Cock

**书籍**

  - [JavaScript: The Good Parts](https://www.amazon.com/JavaScript-Good-Parts-Douglas-Crockford/dp/0596517742) - Douglas Crockford
  - [JavaScript Patterns](https://www.amazon.com/JavaScript-Patterns-Stoyan-Stefanov/dp/0596806752) - Stoyan Stefanov
  - [Pro JavaScript Design Patterns](https://www.amazon.com/JavaScript-Design-Patterns-Recipes-Problem-Solution/dp/159059908X)  - Ross Harmes and Dustin Diaz
  - [High Performance Web Sites: Essential Knowledge for Front-End Engineers](https://www.amazon.com/High-Performance-Web-Sites-Essential/dp/0596529309) - Steve Souders
  - [Maintainable JavaScript](https://www.amazon.com/Maintainable-JavaScript-Nicholas-C-Zakas/dp/1449327680) - Nicholas C. Zakas
  - [JavaScript Web Applications](https://www.amazon.com/JavaScript-Web-Applications-Alex-MacCaw/dp/144930351X) - Alex MacCaw
  - [Pro JavaScript Techniques](https://www.amazon.com/Pro-JavaScript-Techniques-John-Resig/dp/1590597273) - John Resig
  - [Smashing Node.js: JavaScript Everywhere](https://www.amazon.com/Smashing-Node-js-JavaScript-Everywhere-Magazine/dp/1119962595) - Guillermo Rauch
  - [Secrets of the JavaScript Ninja](https://www.amazon.com/Secrets-JavaScript-Ninja-John-Resig/dp/193398869X) - John Resig and Bear Bibeault
  - [Human JavaScript](http://humanjavascript.com/) - Henrik Joreteg
  - [Superhero.js](http://superherojs.com/) - Kim Joar Bekkelund, Mads Mobæk, & Olav Bjorkoy
  - [JSBooks](http://jsbooks.revolunet.com/) - Julien Bouquillon
  - [Third Party JavaScript](https://www.manning.com/books/third-party-javascript) - Ben Vinegar and Anton Kovalyov
  - [Effective JavaScript: 68 Specific Ways to Harness the Power of JavaScript](http://amzn.com/0321812182) - David Herman
  - [Eloquent JavaScript](http://eloquentjavascript.net/) - Marijn Haverbeke
  - [You Don’t Know JS: ES6 & Beyond](http://shop.oreilly.com/product/0636920033769.do) - Kyle Simpson

**博客**

  - [JavaScript Weekly](http://javascriptweekly.com/)
  - [JavaScript, JavaScript...](https://javascriptweblog.wordpress.com/)
  - [Bocoup Weblog](https://bocoup.com/weblog)
  - [Adequately Good](http://www.adequatelygood.com/)
  - [NCZOnline](https://www.nczonline.net/)
  - [Perfection Kills](http://perfectionkills.com/)
  - [Ben Alman](http://benalman.com/)
  - [Dmitry Baranovskiy](http://dmitry.baranovskiy.com/)
  - [nettuts](http://code.tutsplus.com/?s=javascript)

**播客**

  - [JavaScript Air](https://javascriptair.com/)
  - [JavaScript Jabber](https://devchat.tv/js-jabber/)

**[⬆ 返回目录](#table-of-contents)**

## <a id="the-javascript-style-guide-guide">JavaScript风格指南的指南</a>

  - [Reference](https://github.com/airbnb/javascript/wiki/The-JavaScript-Style-Guide-Guide)

## <a id="license">许可证</a>

(The MIT License)

Copyright (c) 2012 康兵奎

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
'Software'), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY
CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

**[⬆ 返回目录](#table-of-contents)**

## <a id="amendments">修正案</a>

我们鼓励您使用此指南并更改规则以适应您的团队的风格指南。下面，你可以列出一些对风格指南的修正。这允许您定期更新您的样式指南，而不必处理合并冲突。

# };
