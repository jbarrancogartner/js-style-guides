# Gartner Peer Insights JavaScript Style Guide

*It was inspired from Airbnb's Javascript [Airbnb's Javascript Style Guide](https://github.com/airbnb/javascript) *

> **Note**: this guide assumes you are using [Babel](https://babeljs.io), and requires that you use [babel-preset-airbnb](https://npmjs.com/babel-preset-airbnb) or the equivalent. It also assumes you are installing shims/polyfills in your app, with [airbnb-browser-shims](https://npmjs.com/airbnb-browser-shims) or the equivalent.

## Table of Contents

  1. [References](#references)
  1. [Objects](#objects)
  1. [Arrays](#arrays)
  1. [Destructuring](#destructuring)
  1. [Strings](#strings)
  1. [Functions](#functions)
  1. [Arrow Functions](#arrow-functions)
  1. [Classes & Constructors](#classes--constructors)
  1. [Modules](#modules)
  1. [Properties](#properties)
  1. [Variables](#variables)
  1. [Blocks](#blocks)
  1. [Comments](#comments)
  1. [Whitespace](#whitespace)
  1. [Commas](#commas)
  1. [Semicolons](#semicolons)
  1. [Naming Conventions](#naming-conventions)
  1. [Promises & Async/Await](#promises-async-await)
  1. [ECMAScript 5 Compatibility](#ecmascript-5-compatibility)
  1. [ECMAScript 6+ (ES 2015+) Styles](#ecmascript-6-es-2015-styles)


## References

  <a name="references--prefer-const"></a><a name="1.1"></a>
  - [1.1](#references--prefer-const) Use `const` for all of your references; avoid using `var`. eslint: [`prefer-const`](https://eslint.org/docs/rules/prefer-const), [`no-const-assign`](https://eslint.org/docs/rules/no-const-assign)

    > Why? This ensures that you can’t reassign your references, which can lead to bugs and difficult to comprehend code.

    ```javascript
    // bad
    var a = 1;
    var b = 2;

    // good
    const a = 1;
    const b = 2;
    ```

  <a name="references--disallow-var"></a><a name="1.2"></a>
  - [1.2](#references--disallow-var) If you must reassign references, use `let` instead of `var`. eslint: [`no-var`](https://eslint.org/docs/rules/no-var)

    > Why? `let` is block-scoped rather than function-scoped like `var`.

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

  <a name="references--block-scope"></a><a name="1.3"></a>
  - [2.3](#references--block-scope) Note that both `let` and `const` are block-scoped, whereas `var` is function-scoped.

    ```javascript
    // const and let only exist in the blocks they are defined in.
    {
      let a = 1;
      const b = 1;
      var c = 1;
    }
    console.log(a); // ReferenceError
    console.log(b); // ReferenceError
    console.log(c); // Prints 1
    ```

    In the above code, you can see that referencing `a` and `b` will produce a ReferenceError, while `c` contains the number. This is because `a` and `b` are block scoped, while `c` is scoped to the containing function.

**[⬆ back to top](#table-of-contents)**

## Objects

  <a name="objects--no-new"></a><a name="2.1"></a>
  - [2.1](#objects--no-new) Use the literal syntax for object creation. eslint: [`no-new-object`](https://eslint.org/docs/rules/no-new-object)

    ```javascript
    // bad
    const item = new Object();

    // good
    const item = {};
    ```

  <a name="es6-object-concise"></a><a name="2.1"></a>
  - [2.1](#es6-object-concise) Use property value shorthand. eslint: [`object-shorthand`](https://eslint.org/docs/rules/object-shorthand)

    > Why? It is shorter and descriptive.

    ```javascript
    const michaelScott = 'Michael Scott';

    // bad
    const obj = {
      michaelScott: michaelScott,
    };

    // good
    const obj = {
      michaelScott,
    };
    ```

  <a name="objects--quoted-props"></a><a name="2.2"></a>
  - [2.2](#objects--quoted-props) Only quote properties that are invalid identifiers. eslint: [`quote-props`](https://eslint.org/docs/rules/quote-props)

    > Why? In general it's considered subjectively easier to read. It improves syntax highlighting, and is also more easily optimized by many JS engines.

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

  <a name="objects--rest-spread"></a><a name="2.3"></a>
  - [2.3](#objects--rest-spread) Prefer the object spread syntax over [`Object.assign`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Object/assign) to shallow-copy objects. Use the object rest parameter syntax to get a new object with certain properties omitted. eslint: [`prefer-object-spread`](https://eslint.org/docs/rules/prefer-object-spread)

    ```javascript
    // very bad
    const original = { a: 1, b: 2 };
    const copy = Object.assign(original, { c: 3 }); // this mutates `original`
    delete copy.a; // so does this

    // bad
    const original = { a: 1, b: 2 };
    const copy = Object.assign({}, original, { c: 3 }); // copy => { a: 1, b: 2, c: 3 }

    // good
    const original = { a: 1, b: 2 };
    const copy = { ...original, c: 3 }; // copy => { a: 1, b: 2, c: 3 }

    const { a, ...noA } = copy; // noA => { b: 2, c: 3 }
    ```

**[⬆ back to top](#table-of-contents)**

## Arrays

  <a name="arrays--literals"></a><a name="3.1"></a>
  - [3.1](#arrays--literals) Use the literal syntax for array creation. eslint: [`no-array-constructor`](https://eslint.org/docs/rules/no-array-constructor)

    ```javascript
    // bad
    const items = new Array();

    // good
    const items = [];
    ```

  <a name="arrays--push"></a><a name="3.2"></a>
  - [3.2](#arrays--push) Use [Array#push](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Array/push) instead of direct assignment to add items to an array.

    ```javascript
    const someStack = [];

    // bad
    someStack[someStack.length] = 'abracadabra';

    // good
    someStack.push('abracadabra');
    ```

  <a name="es6-array-spreads"></a><a name="3.3"></a>
  - [3.3](#es6-array-spreads) Use array spreads `...` to copy arrays.

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

  <a name="arrays--from"></a>
  <a name="arrays--from-iterable"></a><a name="3.4"></a>
  - [3.4](#arrays--from-iterable) To convert an iterable object to an array, use spreads `...` instead of [`Array.from`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Array/from)

    ```javascript
    const foo = document.querySelectorAll('.foo');

    // good
    const nodes = Array.from(foo);

    // best
    const nodes = [...foo];
    ```

**[⬆ back to top](#table-of-contents)**

## Destructuring

  <a name="destructuring--object"></a><a name="4.1"></a>
  - [4.1](#destructuring--object) Use object destructuring when accessing and using multiple properties of an object. eslint: [`prefer-destructuring`](https://eslint.org/docs/rules/prefer-destructuring)

    > Why? Destructuring saves you from creating temporary references for those properties, and from repetitive access of the object. Repeating object access creates more repetitive code, requires more reading, and creates more opportunities for mistakes. Destructuring objects also provides a single site of definition of the object structure that is used in the block, rather than requiring reading the entire block to determine what is used.

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

  <a name="destructuring--array"></a><a name="4.2"></a>
  - [4.2](#destructuring--array) Use array destructuring. eslint: [`prefer-destructuring`](https://eslint.org/docs/rules/prefer-destructuring)

    ```javascript
    const arr = [1, 2, 3, 4];

    // bad
    const first = arr[0];
    const second = arr[1];

    // good
    const [first, second] = arr;
    ```

**[⬆ back to top](#table-of-contents)**

## Strings

  <a name="strings--quotes"></a><a name="5.1"></a>
  - [5.1](#strings--quotes) Use double quotes `""` for strings. eslint: [`quotes`](https://eslint.org/docs/rules/quotes)

    ```javascript
    // bad
    const name = 'Capt. Janeway';

    // bad - template literals should contain interpolation or newlines
    const name = `Capt. Janeway`;

    // good
    const name = "Capt. Janeway";
    ```

  <a name="es6-template-literals"></a><a name="5.2"></a>
  - [5.2](#es6-template-literals) When programmatically building up strings, use template strings instead of concatenation. eslint: [`prefer-template`](https://eslint.org/docs/rules/prefer-template) [`template-curly-spacing`](https://eslint.org/docs/rules/template-curly-spacing)

    > Why? Template strings give you a readable, concise syntax with proper newlines and string interpolation features.

    ```javascript
    // bad
    function sayHi(name) {
      return "How are you, " + name + "?";
    }

    // bad
    function sayHi(name) {
      return ["How are you, ", name, "?"].join();
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

**[⬆ back to top](#table-of-contents)**

## Functions

  <a name="functions--declarations"></a><a name="6.1"></a>
  - [6.1](#functions--declarations) Use named function expressions instead of function declarations. eslint: [`func-style`](https://eslint.org/docs/rules/func-style)

    > Why? Function declarations are hoisted, which means that it’s easy - too easy - to reference the function before it is defined in the file. This harms readability and maintainability. If you find that a function’s definition is large or complex enough that it is interfering with understanding the rest of the file, then perhaps it’s time to extract it to its own module! Don’t forget to explicitly name the expression, regardless of whether or not the name is inferred from the containing variable (which is often the case in modern browsers or when using compilers such as Babel). This eliminates any assumptions made about the Error’s call stack.

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
    // lexical name distinguished from the variable-referenced invocation(s)
    const short = function longUniqueMoreDescriptiveLexicalFoo() {
      // ...
    };
    
    // good
    // use of arrow function to prevent unnamed function
    const foo = () => {
      // ...
    };
    ```


  <a name="es6-default-parameters"></a><a name="6.2"></a>
  - [6.2](#es6-default-parameters) Use default parameter syntax rather than mutating function arguments. It can cause optimization issues, especially in V8.

    ```javascript
    // really bad
    function handleThings(opts, color) {
      // No! We shouldn’t mutate function arguments.
      // Double bad: if opts is falsy it'll be set to an object which may
      // be what you want but it can introduce subtle bugs.
      opts = opts || {};
      color = color || "#FFFFFF";
      // ...
    }

    // still bad
    function handleThings(opts, color) {
      if (opts === void 0) {
        opts = {};
      }
      if (color === void 0) {
        color = "#FFFFFF";
      }
      // ...
    }

    // good
    function handleThings(opts = {}, color = "#FFFFFF") {
      // ...
    }
    ```

  <a name="functions--constructor"></a><a name="6.3"></a>
  - [6.3](#functions--constructor) Never use the Function constructor to create a new function. eslint: [`no-new-func`](https://eslint.org/docs/rules/no-new-func)

    > Why? Creating a function in this way evaluates a string similarly to `eval()`, which opens vulnerabilities.

    ```javascript
    // bad
    var add = new Function("a", "b", "return a + b");

    // still bad
    var subtract = Function("a", "b", "return a - b");
    ```

  <a name="functions--signature-spacing"></a><a name="6.4"></a>
  - [6.4](#functions--signature-spacing) Spacing in a function signature. eslint: [`space-before-function-paren`](https://eslint.org/docs/rules/space-before-function-paren) [`space-before-blocks`](https://eslint.org/docs/rules/space-before-blocks)

    > Why? Consistency is good, and you shouldn’t have to add or remove a space when adding or removing a name.

    ```javascript
    // bad
    const f = function(){};
    const g = function (){};
    const h = function() {};

    // good
    const x = function () {};
    const y = function a() {};
    ```

  <a name="functions--signature-invocation-indentation"></a>
  - [6.5](#functions--signature-invocation-indentation) Functions with multiline signatures, or invocations, should be indented just like every other multiline list in this guide: with each item on a line by itself, with a trailing comma on the last item. eslint: [`function-paren-newline`](https://eslint.org/docs/rules/function-paren-newline)

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

**[⬆ back to top](#table-of-contents)**

## Arrow Functions

  <a name="arrows--use-them"></a><a name="7.1"></a>
  - [7.1](#arrows--use-them) When you must use an anonymous function (as when passing an inline callback), use arrow function notation. eslint: [`prefer-arrow-callback`](https://eslint.org/docs/rules/prefer-arrow-callback), [`arrow-spacing`](https://eslint.org/docs/rules/arrow-spacing)

    > Why? It creates a version of the function that executes in the context of `this`, which is usually what you want, and is a more concise syntax.

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

  <a name="arrows--implicit-return"></a><a name="7.2"></a>
  - [7.2](#arrows--implicit-return) If the function body consists of a single statement returning an [expression](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Expressions_and_Operators#Expressions) without side effects, omit the braces and use the implicit return. Otherwise, keep the braces and use a `return` statement. eslint: [`arrow-parens`](https://eslint.org/docs/rules/arrow-parens), [`arrow-body-style`](https://eslint.org/docs/rules/arrow-body-style)

    > Why? Syntactic sugar. It reads well when multiple functions are chained together.

    ```javascript
    // bad
    [1, 2, 3].map((number) => {
      const nextNumber = number + 1;
      `A string containing the ${nextNumber}.`;
    });

    // good
    [1, 2, 3].map((number) => `A string containing the ${number + 1}.`);

    // good
    [1, 2, 3].map((number) => {
      const nextNumber = number + 1;
      return `A string containing the ${nextNumber}.`;
    });

    // good
    [1, 2, 3].map((number, index) => ({
      [index]: number,
    }));

    // No implicit return with side effects
    function foo(callback) {
      const val = callback();
      if (val === true) {
        // Do something if callback returns true
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

  <a name="arrows--one-arg-parens"></a><a name="7.3"></a>
  - [7.3](#arrows--one-arg-parens) Always include parentheses around arguments for clarity and consistency. eslint: [`arrow-parens`](https://eslint.org/docs/rules/arrow-parens)

    > Why? Minimizes diff churn when adding or removing arguments.

    ```javascript
    // bad
    [1, 2, 3].map(x => x * x);

    // good
    [1, 2, 3].map((x) => x * x);

    // bad
    [1, 2, 3].map(number => (
      `A long string with the ${number}. It’s so long that we don’t want it to take up space on the .map line!`
    ));

    // good
    [1, 2, 3].map((number) => (
      `A long string with the ${number}. It’s so long that we don’t want it to take up space on the .map line!`
    ));
    ```

  <a name="arrows--confusing"></a><a name="7.4"></a>
  - [7.4](#arrows--confusing) Avoid confusing arrow function syntax (`=>`) with comparison operators (`<=`, `>=`). eslint: [`no-confusing-arrow`](https://eslint.org/docs/rules/no-confusing-arrow)

    ```javascript
    // bad
    const itemHeight = (item) => item.height <= 256 ? item.largeSize : item.smallSize;

    // bad
    const itemHeight = (item) => item.height >= 256 ? item.largeSize : item.smallSize;

    // good
    const itemHeight = (item) => (item.height <= 256 ? item.largeSize : item.smallSize);

    // good
    const itemHeight = (item) => {
      const { height, largeSize, smallSize } = item;
      return height <= 256 ? largeSize : smallSize;
    };
    ```

**[⬆ back to top](#table-of-contents)**

## Classes & Constructors

  <a name="constructors--no-useless"></a><a name="8.1"></a>
  - [8.1](#constructors--no-useless) Classes have a default constructor if one is not specified. An empty constructor function or one that just delegates to a parent class is unnecessary. eslint: [`no-useless-constructor`](https://eslint.org/docs/rules/no-useless-constructor)

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

**[⬆ back to top](#table-of-contents)**

## Modules

  <a name="modules--use-them"></a><a name="9.1"></a>
  - [9.1](#modules--use-them) Always use modules (`import`/`export`) over a non-standard module system.

    > Why? Modules are the future.

    ```javascript
    // bad
    const GartnerStyleGuide = require("./GartnerStyleGuide");
    module.exports = AirbnbStyleGuide.es6;

    // ok
    import GartnerStyleGuide from "./GartnerStyleGuide";
    export default GartnerStyleGuide.es6;

    // best
    import { es6 } from "./GartnerStyleGuide";
    export default es6;
    ```

  <a name="modules--no-wildcard"></a><a name="9.2"></a>
  - [9.2](#modules--no-wildcard) Do not use wildcard imports.

    > Why? This makes sure you have a single default export.

    ```javascript
    // bad
    import * as GartnerStyleGuide from "./GartnerStyleGuide";

    // good
    import GartnerStyleGuide from "./GartnerStyleGuide";
    ```

  <a name="modules--prefer-default-export"></a>
  - [9.3](#modules--prefer-default-export) In modules with a single export, prefer default export over named export.
 eslint: [`import/prefer-default-export`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/prefer-default-export.md)
    > Why? To encourage more files that only ever export one thing, which is better for readability and maintainability.

    ```javascript
    // bad
    export function foo() {}

    // good
    export default function foo() {}
    ```

  <a name="modules--multiline-imports-over-newlines"></a>
  - [9.4](#modules--multiline-imports-over-newlines) Multiline imports should be indented just like multiline array and object literals.
 eslint: [`object-curly-newline`](https://eslint.org/docs/rules/object-curly-newline)

    > Why? The curly braces follow the same indentation rules as every other curly brace block in the style guide, as do the trailing commas.

    ```javascript
    // bad
    import {longNameA, longNameB, longNameC, longNameD, longNameE} from "path";

    // good
    import {
      longNameA,
      longNameB,
      longNameC,
      longNameD,
      longNameE,
    } from "path";
    ```

  <a name="modules--import-extensions"></a>
  - [9.5](#modules--import-extensions) Do not include JavaScript filename extensions.
 eslint: [`import/extensions`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/extensions.md)
    > Why? Including extensions inhibits refactoring, proper use of platform-specific logic, and inappropriately hardcodes implementation details of the module you're importing in every consumer.

    ```javascript
    // bad
    import foo from "./foo.js";
    import bar from "./bar.jsx";
    import baz from "./baz/index.web.jsx";

    // good
    import foo from "./foo";
    import bar from "./bar";
    import baz from "./baz";
    ```

**[⬆ back to top](#table-of-contents)**

## Properties

  <a name="properties--dot"></a><a name="10.1"></a>
  - [10.1](#properties--dot) Use dot notation when accessing properties. eslint: [`dot-notation`](https://eslint.org/docs/rules/dot-notation)

    ```javascript
    const luke = {
      jedi: true,
      age: 28,
    };

    // bad
    const isJedi = luke["jedi"];

    // good
    const isJedi = luke.jedi;
    ```

  <a name="properties--bracket"></a><a name="10.2"></a>
  - [10.2](#properties--bracket) Use bracket notation `[]` when accessing properties with a variable.

    ```javascript
    const luke = {
      jedi: true,
      age: 28,
    };

    function getProp(prop) {
      return luke[prop];
    }

    const isJedi = getProp("jedi");
    ```

**[⬆ back to top](#table-of-contents)**

## Variables

  <a name="variables--const"></a><a name="11.1"></a>
  - [11.1](#variables--const) Always use `const` or `let` to declare variables. Not doing so will result in global variables. We want to avoid polluting the global namespace. eslint: [`no-undef`](https://eslint.org/docs/rules/no-undef) [`prefer-const`](https://eslint.org/docs/rules/prefer-const)

    ```javascript
    // bad
    superPower = new SuperPower();

    // good
    const superPower = new SuperPower();
    ```

  <a name="variables--one-const"></a><a name="11.2"></a>
  - [11.2](#variables--one-const) Use one `const` or `let` declaration per variable or assignment. eslint: [`one-var`](https://eslint.org/docs/rules/one-var)

    > Why? It’s easier to add new variable declarations this way, and you never have to worry about swapping out a `;` for a `,` or introducing punctuation-only diffs. You can also step through each declaration with the debugger, instead of jumping through all of them at once.

    ```javascript
    // bad
    const items = getItems(),
        goSportsTeam = true,
        theOfficeCharacter = "Jim";

    // bad
    // (compare to above, and try to spot the mistake)
    const items = getItems(),
        goSportsTeam = true;
        theOfficeCharacter = "Jim";

    // good
    const items = getItems();
    const goSportsTeam = true;
    const theOfficeCharacter = "Jim";
    ```

  <a name="variables--unary-increment-decrement"></a><a name="11.3"></a>
  - [11.3](#variables--unary-increment-decrement) Avoid using unary increments and decrements (`++`, `--`). eslint [`no-plusplus`](https://eslint.org/docs/rules/no-plusplus)

    > Why? Per the eslint documentation, unary increment and decrement statements are subject to automatic semicolon insertion and can cause silent errors with incrementing or decrementing values within an application. It is also more expressive to mutate your values with statements like `num += 1` instead of `num++` or `num ++`. Disallowing unary increment and decrement statements also prevents you from pre-incrementing/pre-decrementing values unintentionally which can also cause unexpected behavior in your programs.

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

**[⬆ back to top](#table-of-contents)**

## Blocks

  <a name="blocks--braces"></a><a name="12.1"></a>
  - [12.1](#blocks--braces) Use braces with all multiline blocks. eslint: [`nonblock-statement-body-position`](https://eslint.org/docs/rules/nonblock-statement-body-position)

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

  <a name="blocks--cuddled-elses"></a><a name="12.2"></a>
  - [12.2](#blocks--cuddled-elses) If you’re using multiline blocks with `if` and `else`, put `else` on the same line as your `if` block’s closing brace. eslint: [`brace-style`](https://eslint.org/docs/rules/brace-style)

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

**[⬆ back to top](#table-of-contents)**

## Comments

  <a name="comments--multiline"></a><a name="13.1"></a>
  - [13.1](#comments--multiline) Use `/** ... */` for multiline comments.

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

  <a name="comments--singleline"></a><a name="13.2"></a>
  - [13.2](#comments--singleline) Use `//` for single line comments. Place single line comments on a newline above the subject of the comment. Put an empty line before the comment unless it’s on the first line of a block.

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
  - [13.3](#comments--spaces) Start all comments with a space to make it easier to read. eslint: [`spaced-comment`](https://eslint.org/docs/rules/spaced-comment)

    ```javascript
    // bad
    //is current tab
    const active = true;

    // good
    // is current tab
    const active = true;
    ```

  <a name="comments--actionitems"></a><a name="13.4"></a>
  - [13.4](#comments--actionitems) Prefixing your comments with `FIXME` or `TODO` helps other developers quickly understand if you’re pointing out a problem that needs to be revisited, or if you’re suggesting a solution to the problem that needs to be implemented. These are different than regular comments because they are actionable. The actions are `FIXME: -- need to figure this out` or `TODO: -- need to implement`.

  <a name="comments--fixme"></a><a name="13.5"></a>
  - [13.5](#comments--fixme) Use `// FIXME:` to annotate problems.

    ```javascript
    class Calculator extends Abacus {
      constructor() {
        super();

        // FIXME: shouldn’t use a global here
        total = 0;
      }
    }
    ```

  <a name="comments--todo"></a><a name="13.6"></a>
  - [13.6](#comments--todo) Use `// TODO:` to annotate solutions to problems.

    ```javascript
    class Calculator extends Abacus {
      constructor() {
        super();

        // TODO: total should be configurable by an options param
        this.total = 0;
      }
    }
    ```

**[⬆ back to top](#table-of-contents)**

## Whitespace

  <a name="whitespace--spaces"></a><a name="14.1"></a>
  - [14.1](#whitespace--spaces) Use soft tabs (space character) set to 2 spaces. eslint: [`indent`](https://eslint.org/docs/rules/indent)

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

  <a name="whitespace--before-blocks"></a><a name="14.2"></a>
  - [14.2](#whitespace--before-blocks) Place 1 space before the leading brace. eslint: [`space-before-blocks`](https://eslint.org/docs/rules/space-before-blocks)

    ```javascript
    // bad
    function test(){
      console.log("test");
    }

    // good
    function test() {
      console.log("test");
    }

    // bad
    dog.set("attr",{
      age: "1 year",
      breed: "Bernese Mountain Dog",
    });

    // good
    dog.set("attr", {
      age: "1 year",
      breed: "Bernese Mountain Dog",
    });
    ```

  <a name="whitespace--around-keywords"></a><a name="14.3"></a>
  - [14.3](#whitespace--around-keywords) Place 1 space before the opening parenthesis in control statements (`if`, `while` etc.). Place no space between the argument list and the function name in function calls and declarations. eslint: [`keyword-spacing`](https://eslint.org/docs/rules/keyword-spacing)

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
      console.log ("Swooosh!");
    }

    // good
    function fight() {
      console.log("Swooosh!");
    }
    ```

  <a name="whitespace--infix-ops"></a><a name="14.4"></a>
  - [14.4](#whitespace--infix-ops) Set off operators with spaces. eslint: [`space-infix-ops`](https://eslint.org/docs/rules/space-infix-ops)

    ```javascript
    // bad
    const x=y+5;

    // good
    const x = y + 5;
    ```

  <a name="whitespace--chains"></a><a name="14.5"></a>
  - [14.5](#whitespace--chains) Use indentation when making long method chains (more than 2 method chains). Use a leading dot, which
    emphasizes that the line is a method call, not a new statement. eslint: [`newline-per-chained-call`](https://eslint.org/docs/rules/newline-per-chained-call) [`no-whitespace-before-property`](https://eslint.org/docs/rules/no-whitespace-before-property)

    ```javascript
    // bad
    $("#items').find(".selected").highlight().end().find(".open").updateCount();

    // bad
    $("#items").
      find(".selected").
        highlight().
        end().
      find(".open").
        updateCount();

    // good
    $('#items')
      .find(".selected")
        .highlight()
        .end()
      .find(".open")
        .updateCount();

    ```

  <a name="whitespace--after-blocks"></a><a name="14.6"></a>
  - [14.6](#whitespace--after-blocks) Leave a blank line after blocks and before the next statement.

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
    ```

  <a name="whitespace--padded-blocks"></a><a name="14.7"></a>
  - [14.7](#whitespace--padded-blocks) Do not pad your blocks with blank lines. eslint: [`padded-blocks`](https://eslint.org/docs/rules/padded-blocks)

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
    
  <a name="whitespace--in-parens"></a><a name="14.8"></a>
  - [14.8](#whitespace--in-parens) Do not add spaces inside parentheses. eslint: [`space-in-parens`](https://eslint.org/docs/rules/space-in-parens)

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
      console.log( foo );
    }

    // good
    if (foo) {
      console.log(foo);
    }
    ```

  <a name="whitespace--in-brackets"></a><a name="14.9"></a>
  - [14.9](#whitespace--in-brackets) Do not add spaces inside brackets. eslint: [`array-bracket-spacing`](https://eslint.org/docs/rules/array-bracket-spacing)

    ```javascript
    // bad
    const foo = [ 1, 2, 3 ];
    console.log(foo[ 0 ]);

    // good
    const foo = [1, 2, 3];
    console.log(foo[0]);
    ```

  <a name="whitespace--in-braces"></a><a name="14.10"></a>
  - [14.10](#whitespace--in-braces) Add spaces inside curly braces. eslint: [`object-curly-spacing`](https://eslint.org/docs/rules/object-curly-spacing)

    ```javascript
    // bad
    const foo = {clark: "kent"};

    // good
    const foo = { clark: "kent" };
    ```

<a name="whitespace--block-spacing"></a>
  - [14.11](#whitespace--block-spacing) Require consistent spacing inside an open block token and the next token on the same line. This rule also enforces consistent spacing inside a close block token and previous token on the same line. eslint: [`block-spacing`](https://eslint.org/docs/rules/block-spacing)

    ```javascript
    // bad
    function foo() {return true;}
    if (foo) { bar = 0;}

    // good
    function foo() { return true; }
    if (foo) { bar = 0; }
    ```

  <a name="whitespace--key-spacing"></a>
  - [14.12](#whitespace--key-spacing) Enforce spacing between keys and values in object literal properties. eslint: [`key-spacing`](https://eslint.org/docs/rules/key-spacing)

    ```javascript
    // bad
    var obj = { foo : 42 };
    var obj2 = { foo:42 };

    // good
    var obj = { foo: 42 };
    ```

**[⬆ back to top](#table-of-contents)**

## Commas

  <a name="commas--leading-trailing"></a><a name="15.1"></a>
  - [15.1](#commas--leading-trailing) Leading commas: **Nope.** eslint: [`comma-style`](https://eslint.org/docs/rules/comma-style)

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
        firstName: "Ada"
      , lastName: "Lovelace"
      , birthYear: 1815
      , superPower: "computers"
    };

    // good
    const hero = {
      firstName: "Ada",
      lastName: "Lovelace",
      birthYear: 1815,
      superPower: "computers",
    };
    ```

  <a name="commas--dangling"></a><a name="15.2"></a>
  - [15.2](#commas--dangling) Additional trailing comma: **Yup.** eslint: [`comma-dangle`](https://eslint.org/docs/rules/comma-dangle)

    > Why? This leads to cleaner git diffs. Also, transpilers like Babel will remove the additional trailing comma in the transpiled code which means you don’t have to worry about the trailing comma problem in legacy browsers.

    ```diff
    // bad - git diff without trailing comma
    const hero = {
         firstName: "Florence",
    -    lastName: "Nightingale"
    +    lastName: "Nightingale",
    +    inventorOf: ["coxcomb chart", "modern nursing"]
    };

    // good - git diff with trailing comma
    const hero = {
         firstName: "Florence",
         lastName: "Nightingale",
    +    inventorOf: ["coxcomb chart", "modern nursing"],
    };
    ```

**[⬆ back to top](#table-of-contents)**

## Semicolons

  <a name="semicolons--required"></a><a name="16.1"></a>
  - [16.1](#semicolons--required) **Yup.** eslint: [`semi`](https://eslint.org/docs/rules/semi)

    > Why? When JavaScript encounters a line break without a semicolon, it uses a set of rules called [Automatic Semicolon Insertion](https://tc39.github.io/ecma262/#sec-automatic-semicolon-insertion) to determine whether it should regard that line break as the end of a statement, and (as the name implies) place a semicolon into your code before the line break if it thinks so. ASI contains a few eccentric behaviors, though, and your code will break if JavaScript misinterprets your line break. These rules will become more complicated as new features become a part of JavaScript. Explicitly terminating your statements and configuring your linter to catch missing semicolons will help prevent you from encountering issues.

    ```javascript
    // bad - raises exception
    const luke = {}
    const leia = {}
    [luke, leia].forEach((jedi) => jedi.father = "vader")

    // bad - raises exception
    const reaction = "No! That’s impossible!"
    (async function meanwhileOnTheFalcon() {
      // handle `leia`, `lando`, `chewie`, `r2`, `c3p0`
      // ...
    }())

    // bad - returns `undefined` instead of the value on the next line - always happens when `return` is on a line by itself because of ASI!
    function foo() {
      return
        "search your feelings, you know it to be foo"
    }

    // good
    const luke = {};
    const leia = {};
    [luke, leia].forEach((jedi) => {
      jedi.father = 'vader';
    });

    // good
    const reaction = "No! That’s impossible!";
    (async function meanwhileOnTheFalcon() {
      // handle `leia`, `lando`, `chewie`, `r2`, `c3p0`
      // ...
    }());

    // good
    function foo() {
      return "search your feelings, you know it to be foo";
    }
    ```

**[⬆ back to top](#table-of-contents)**

## Naming Conventions

  <a name="naming--descriptive"></a><a name="17.1"></a>
  - [17.1](#naming--descriptive) Avoid single letter names. Be descriptive with your naming. eslint: [`id-length`](https://eslint.org/docs/rules/id-length)

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

  <a name="naming--camelCase"></a><a name="17.2"></a>
  - [17.2](#naming--camelCase) Use camelCase when naming objects, functions, and instances. eslint: [`camelcase`](https://eslint.org/docs/rules/camelcase)

    ```javascript
    // bad
    const OBJEcttsssss = {};
    const this_is_my_object = {};
    function c() {}

    // good
    const thisIsMyObject = {};
    function thisIsMyFunction() {}
    ```

  <a name="naming--PascalCase"></a><a name="17.3"></a>
  - [17.3](#naming--PascalCase) Use PascalCase only when naming constructors, classes, React components, singletons or bare objects. eslint: [`new-cap`](https://eslint.org/docs/rules/new-cap)

    ```javascript
    // bad
    function user(options) {
      this.name = options.name;
    }

    const bad = new user({
      name: "nope",
    });
    
    const reactComponent = () => {
      return <p>I'm not a huge Star Wars fan</p>;
    }

    // good
    class User {
      constructor(options) {
        this.name = options.name;
      }
    }

    const good = new User({
      name: "yup",
    });
    
    const ReactComponent = () => {
      return <p>I'm not a huge Star Wars fan</p>;
    }
    ```

  <a name="naming--leading-underscore"></a><a name="17.4"></a>
  - [17.4](#naming--leading-underscore) Do not use trailing or leading underscores. eslint: [`no-underscore-dangle`](https://eslint.org/docs/rules/no-underscore-dangle)

    > Why? JavaScript does not have the concept of privacy in terms of properties or methods. Although a leading underscore is a common convention to mean “private”, in fact, these properties are fully public, and as such, are part of your public API contract. This convention might lead developers to wrongly think that a change won’t count as breaking, or that tests aren’t needed. tl;dr: if you want something to be “private”, it must not be observably present.

    ```javascript
    // bad
    this.__firstName__ = "Michael";
    this.firstName_ = "Michael";
    this._firstName = "Michael";
    const _lastName = "Scott";

    // good
    this.firstName = "Michael";
    const lastName = "Scott"
    ```


  <a name="naming--Acronyms-and-Initialisms"></a>
  - [17.5](#naming--Acronyms-and-Initialisms) Acronyms and initialisms should always be all uppercased, or all lowercased.

    > Why? Names are for readability, not to appease a computer algorithm.

    ```javascript
    // bad
    import SmsContainer from "./containers/SmsContainer";

    // bad
    const HttpRequests = [
      // ...
    ];

    // good
    import SMSContainer from "./containers/SMSContainer";

    // good
    const HTTPRequests = [
      // ...
    ];

    // also good
    const httpRequests = [
      // ...
    ];

    // best
    import TextMessageContainer from "./containers/TextMessageContainer";

    // best
    const requests = [
      // ...
    ];
    ```

**[⬆ back to top](#table-of-contents)**

## Promises & Async/Await

  <a name="accessors--not-required"></a><a name="18.1"></a>
  - [18.1](#accessors--not-required) Async/Await over Promises.

  <a name="accessors--no-getters-setters"></a><a name="18.1"></a>
  - [18.1](#accessors--no-getters-setters) Use Async/Await syntax over Promises. Async/Await syntax used along with `try catch` prevents [callback hell](http://callbackhell.com/). 

    ```javascript
    // bad
    fetchContent.then((response) => {
      // Handle response
      // ...
      
    }).then((data) => {
      // Do additional processing of data
      // ...
      
    }).catch((err) => {
      // Handle error
      // ...
    });

    // good
    try {
      const response = await fetchContent();
      
      // Handle response on constant `response`
      // ...
      
      // Do additional processing of data
      // ...
      
    } catch (err) {
      // Handle error
      // ...
    }
    ```
    
**[⬆ back to top](#table-of-contents)**
