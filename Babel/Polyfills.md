# Polyfills

# Введение

Когда обновляется стандарт языка, новые стандарты это новые функции, а не **новый синтаксис.** 

Пример такой функции это `Array.prototype.flat()`. Метод `flat()` возвращает новый массив, в котором все элементы вложенных подмассивов были рекурсивно "подняты" на указанный уровень depth.

    var arr1 = [1, 2, [3, 4]];
    arr1.flat(); 
    // [1, 2, 3, 4]

Эта новая функция не добавляет никакого нового синтаксиса, единственное что появляется - новая функция которая объявлена на объекте `prototype`

**Пример кода с этой функцией**

    class App {
      run = async (name = 'World') => {
        console.log(`Hello ${name}`)
        console.log([1, 2, [2, 3]].flat())
      }
    }
    
    const app = new App()
    app.run()
      .then(() => console.log('done'))
      .catch(() => console.log('Error'))

С точки зрения все окей, потому что мы создаем массив и на него применяем функции. Но вот не все браузеры понимают и поддерживают функцию `flat()`

**Решить эту проблему не сложно**

Достаточно создать свою пользовательскую функцию и определить ее на прототипе `Array`

    Array.prototype.flat = () => {} // И нужно написать реализацию это функции естественно

**Вот этот код и называется `Polyfill`**

# Установка core-js

`Core-js` - это библиотека которая занимается полифилами

    npm i core-js

Настройка `.babelrc`

- coreJs - указывает версию
- useBuiltIn: usage - указывает что нужно добавить только те полифилы которые используются и которые не поддерживаются указными ранее версиями браузеров, к примеру мы указывали последние версии Chrome и Firefox - в них эта функция есть по умолчанию, поэтому если поменять версию браузера, в котором нет это функции то такой полифил точно будет в итоговой сборке

    {
      "presets": [["@babel/env", {
        "corejs": 3,
        "useBuiltIns": "usage",
        "debug": true
      }]],
      "plugins": [ "@babel/proposal-class-properties" ]
    }

## Укажем ie11 для проверки

**Файл `.browserslistrc`**

    last 2 chrome version
    last 2 firefox version
    last 1 ie version

Выполним следующие команды и получим

    $ npx babel src --out-dir build
    @babel/preset-env: `DEBUG` option
    
    Using targets:
    {
      "chrome": "80",
      "firefox": "74",
      "ie": "11"
    }
    
    Using modules transform: auto
    
    Using plugins:
      proposal-nullish-coalescing-operator { "ie":"11" }
      proposal-optional-chaining { "ie":"11" }
      proposal-json-strings { "ie":"11" }
      proposal-optional-catch-binding { "ie":"11" }
      transform-parameters { "ie":"11" }
      proposal-async-generator-functions { "ie":"11" }
      proposal-object-rest-spread { "ie":"11" }
      transform-dotall-regex { "firefox":"74", "ie":"11" }
      proposal-unicode-property-regex { "firefox":"74", "ie":"11" }
      transform-named-capturing-groups-regex { "firefox":"74", "ie":"11" }
      transform-async-to-generator { "ie":"11" }
      transform-exponentiation-operator { "ie":"11" }
      transform-template-literals { "ie":"11" }
      transform-literals { "ie":"11" }
      transform-function-name { "ie":"11" }
      transform-arrow-functions { "ie":"11" }
      transform-classes { "ie":"11" }
      transform-object-super { "ie":"11" }
      transform-shorthand-properties { "ie":"11" }
      transform-duplicate-keys { "ie":"11" }
      transform-computed-properties { "ie":"11" }
      transform-for-of { "ie":"11" }
      transform-sticky-regex { "ie":"11" }
      transform-unicode-regex { "ie":"11" }
      transform-spread { "ie":"11" }
      transform-destructuring { "ie":"11" }
      transform-block-scoping { "ie":"11" }
      transform-typeof-symbol { "ie":"11" }
      transform-new-target { "ie":"11" }
      transform-regenerator { "ie":"11" }
      transform-modules-commonjs { "chrome":"80", "firefox":"74", "ie":"11" }
      proposal-dynamic-import { "chrome":"80", "firefox":"74", "ie":"11" }
    
    Using polyfills with `usage` option:
    
    [C:\my-projects\template-trunk\src\main.js] Added following core-js polyfills:
      es.array.flat { "ie":"11" }
      es.array.unscopables.flat { "ie":"11" }
      es.object.to-string { "ie":"11" }
      es.promise { "ie":"11" }
    
    [C:\my-projects\template-trunk\src\main.js] Based on your code and targets, added regenerator-runtime.
    Successfully compiled 1 file with Babel.

**Исходный файл `main.js`**

    class App {
      run = async (name = 'World') => {
        console.log(`Hello ${name}`)
        console.log([1, 2, [2, 3]].flat())
      }
    }
    
    const app = new App()
    app.run()
      .then(() => console.log('done'))
      .catch(() => console.log('Error'))

**Результат после трансформации через Babel**

    "use strict";
    
    require("core-js/modules/es.array.flat");
    
    require("core-js/modules/es.array.unscopables.flat");
    
    require("core-js/modules/es.object.to-string");
    
    require("core-js/modules/es.promise");
    
    require("regenerator-runtime/runtime");
    
    function asyncGeneratorStep(gen, resolve, reject, _next, _throw, key, arg) { try { var info = gen[key](arg); var value = info.value; } catch (error) { reject(error); return; } if (info.done) { resolve(value); } else { Promise.resolve(value).then(_next, _throw); } }
    
    function _asyncToGenerator(fn) { return function () { var self = this, args = arguments; return new Promise(function (resolve, reject) { var gen = fn.apply(self, args); function _next(value) { asyncGeneratorStep(gen, resolve, reject, _next, _throw, "next", value); } function _throw(err) { asyncGeneratorStep(gen, resolve, reject, _next, _throw, "throw", err); } _next(undefined); }); }; }
    
    function _classCallCheck(instance, Constructor) { if (!(instance instanceof Constructor)) { throw new TypeError("Cannot call a class as a function"); } }
    
    function _defineProperty(obj, key, value) { if (key in obj) { Object.defineProperty(obj, key, { value: value, enumerable: true, configurable: true, writable: true }); } else { obj[key] = value; } return obj; }
    
    var App = function App() {
      _classCallCheck(this, App);
    
      _defineProperty(this, "run", /*#__PURE__*/_asyncToGenerator( /*#__PURE__*/regeneratorRuntime.mark(function _callee() {
        var name,
            _args = arguments;
        return regeneratorRuntime.wrap(function _callee$(_context) {
          while (1) {
            switch (_context.prev = _context.next) {
              case 0:
                name = _args.length > 0 && _args[0] !== undefined ? _args[0] : 'World';
                console.log("Hello ".concat(name));
                console.log([1, 2, [2, 3]].flat());
    
              case 3:
              case "end":
                return _context.stop();
            }
          }
        }, _callee);
      })));
    };
    
    var app = new App();
    app.run().then(function () {
      return console.log('done');
    }).catch(function () {
      return console.log('Error');
    });

Как видим тут мы подключаем дополнительные функции с помощью

    require("core-js/modules/es.array.flat");

В данном примере используются `require` - это подключение модулей, характерное для `node.js`, но нам это не нужно, учитывая что работу с модулями браузеры не поддерживают, поэтому с этой задачей будет справляться `webpack` . Поэтому мы можем отключить модули в настройка `.babelrc` - `"modules": false`

    {
      "presets": [["@babel/env", {
        "corejs": 3,
        "useBuiltIns": "usage",
        "debug": true,
        "modules": false
      }]],
      "plugins": [ "@babel/proposal-class-properties" ]
    }
    

В итоге будут использоваться стандартные `import`

    import "core-js/modules/es.array.flat";
    import "core-js/modules/es.array.unscopables.flat";
    import "core-js/modules/es.object.to-string";
    import "core-js/modules/es.promise";
    import "regenerator-runtime/runtime";
    
    function asyncGeneratorStep(gen, resolve, reject, _next, _throw, key, arg) { try { var info = gen[key](arg); var value = info.value; } catch (error) { reject(error); return; } if (info.done) { resolve(value); } else { Promise.resolve(value).then(_next, _throw); } }
    
    function _asyncToGenerator(fn) { return function () { var self = this, args = arguments; return new Promise(function (resolve, reject) { var gen = fn.apply(self, args); function _next(value) { asyncGeneratorStep(gen, resolve, reject, _next, _throw, "next", value); } function _throw(err) { asyncGeneratorStep(gen, resolve, reject, _next, _throw, "throw", err); } _next(undefined); }); }; }
    
    function _classCallCheck(instance, Constructor) { if (!(instance instanceof Constructor)) { throw new TypeError("Cannot call a class as a function"); } }
    
    function _defineProperty(obj, key, value) { if (key in obj) { Object.defineProperty(obj, key, { value: value, enumerable: true, configurable: true, writable: true }); } else { obj[key] = value; } return obj; }
    
    var App = function App() {
      _classCallCheck(this, App);
    
      _defineProperty(this, "run", /*#__PURE__*/_asyncToGenerator( /*#__PURE__*/regeneratorRuntime.mark(function _callee() {
        var name,
            _args = arguments;
        return regeneratorRuntime.wrap(function _callee$(_context) {
          while (1) {
            switch (_context.prev = _context.next) {
              case 0:
                name = _args.length > 0 && _args[0] !== undefined ? _args[0] : 'World';
                console.log("Hello ".concat(name));
                console.log([1, 2, [2, 3]].flat());
    
              case 3:
              case "end":
                return _context.stop();
            }
          }
        }, _callee);
      })));
    };
    
    var app = new App();
    app.run().then(function () {
      return console.log('done');
    }).catch(function () {
      return console.log('Error');
    });

# Итоги

- `Polyfill` - код, который добавляет глобальную функцию, если ее еще нет в браузере
- `core-js` - библиотека полифилов
- конфигурация `.babelrc`

    "presets": [["@babel/env"], {
    	"corejs": 3,
      "useBuiltIns": "usage",
      "debug": true,
      "modules": false
    }]