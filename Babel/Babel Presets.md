# Babel Presets

Настройка Babel с помощью плагинов, довольно трудоемкий и не всегда точный процесс, так как для поддержки определенного стандарта потребуется более десятков плагинов, если не более 30. Хранить и поддерживать такие конфигурационные файлы сложно и не удобно. 

В Babel есть **Presets**, проще говоря уже готовый набор plugins, которые заточены под определенный стандарт и охватывают все нужные аспекты, учитывая нюансы и детали.

# Preset env

Этот preset содержит все современные стандарты, поддерживает даже те стандарты которые еще не вошли в спецификацию языка, но экспериментальные возможности нужно настраивать отдельно. 

Проще говоря этот пресет отображает самую свежую спецификацию языка на данный момент.

## Установка

Preset - это также обычный `npm` пакет

    npm i -D @babel/preset-env

## Конфигурационный файл

Файл - `.babelrc`

    {
      "presets": [ "@babel/env" ]
    }

## Запуск пресета

Используется та же команда что и ранее

    npx babel src --out-dir build

### Исходный файл main.js

    class App {
    
      constructor(name) {
        this.name = name
      }
    
      getName() {
        return this.name
      }
    
      sayHello(msg = 'Default Message') {
        console.log(`Hello ${msg}`)
      }
    }
    
    const app = new App('Andrii Helever')
    app.sayHello(app.getName())

### Результат после работы Babel

    "use strict";
    
    function _classCallCheck(instance, Constructor) { if (!(instance instanceof Constructor)) { throw new TypeError("Cannot call a class as a function"); } }
    
    function _defineProperties(target, props) { for (var i = 0; i < props.length; i++) { var descriptor = props[i]; descriptor.enumerable = descriptor.enumerable || false; descriptor.configurable = true; if ("value" in descriptor) descriptor.writable = true; Object.defineProperty(target, descriptor.key, descriptor); } }
    
    function _createClass(Constructor, protoProps, staticProps) { if (protoProps) _defineProperties(Constructor.prototype, protoProps); if (staticProps) _defineProperties(Constructor, staticProps); return Constructor; }
    
    var App = /*#__PURE__*/function () {
      function App(name) {
        _classCallCheck(this, App);
    
        this.name = name;
      }
    
      _createClass(App, [{
        key: "getName",
        value: function getName() {
          return this.name;
        }
      }, {
        key: "sayHello",
        value: function sayHello() {
          var msg = arguments.length > 0 && arguments[0] !== undefined ? arguments[0] : 'Default Message';
          console.log("Hello ".concat(msg));
        }
      }]);
    
      return App;
    }();
    
    var app = new App('Andrii Helever');
    app.sayHello(app.getName());

## Экспериментальные возможности

Допустим напишем какой то синтаксис, который еще не вошел в самую свежую спецификацию языка, сделаем этот код внутри класса `App`

    getName = () => {
        return this.name
    }

После того как мы запустим Babel он выдаст ошибку, где покажет какой экспериментальный синтаксис мы использовали и подскажет какой еще нужно добавить plugin для обработки этого синтаксиса

    $ npx babel src --out-dir build
    { SyntaxError: C:\my-projects\template-trunk\src\main.js: Support for the experimental syntax 'classProperties' isn't currently enabled (7:11):
    
       5 |   }
       6 | 
    >  7 |   getName = () => {
         |           ^
       8 |     return this.name
       9 |   }
      10 | 
    
    Add @babel/plugin-proposal-class-properties (https://git.io/vb4SL) to the 'plugins' section of your Babel config to enable transformation.

### Для этого сделаем следующие настройки `.babelrc` , также установит этот пакет

    {
      "presets": [ "@babel/env" ],
      "plugins": [ "@babel/proposal-class-properties" ]
    }

    npm i -D @babel/plugin-proposal-class-properties

### Запустим Babel

    npx babel src --out-dir build

### Исходный файл

    class App {
    
      constructor(name) {
        this.name = name
      }
    
      getName = () => {
        return this.name
      }
    
      sayHello(msg = 'Default Message') {
        console.log(`Hello ${msg}`)
      }
    }
    
    const app = new App('Andrii Helever')
    app.sayHello(app.getName())

### Результат

    "use strict";
    
    function _classCallCheck(instance, Constructor) { if (!(instance instanceof Constructor)) { throw new TypeError("Cannot call a class as a function"); } }
    
    function _defineProperties(target, props) { for (var i = 0; i < props.length; i++) { var descriptor = props[i]; descriptor.enumerable = descriptor.enumerable || false; descriptor.configurable = true; if ("value" in descriptor) descriptor.writable = true; Object.defineProperty(target, descriptor.key, descriptor); } }
    
    function _createClass(Constructor, protoProps, staticProps) { if (protoProps) _defineProperties(Constructor.prototype, protoProps); if (staticProps) _defineProperties(Constructor, staticProps); return Constructor; }
    
    function _defineProperty(obj, key, value) { if (key in obj) { Object.defineProperty(obj, key, { value: value, enumerable: true, configurable: true, writable: true }); } else { obj[key] = value; } return obj; }
    
    var App = /*#__PURE__*/function () {
      function App(name) {
        var _this = this;
    
        _classCallCheck(this, App);
    
        _defineProperty(this, "getName", function () {
          return _this.name;
        });
    
        this.name = name;
      }
    
      _createClass(App, [{
        key: "sayHello",
        value: function sayHello() {
          var msg = arguments.length > 0 && arguments[0] !== undefined ? arguments[0] : 'Default Message';
          console.log("Hello ".concat(msg));
        }
      }]);
    
      return App;
    }();
    
    var app = new App('Andrii Helever');
    app.sayHello(app.getName());

# Итоги

1. У `Babel` есть `Preset` - список заранее сконфигурированных настроек, которые можно заранее передать в Babel для того что бы не перечислять их в пустую, на самом деле они умнее чем мы думаем.
2. Также мы можем миксовать `presets` с `plugins` 
`{
  "presets": [ "@babel/env" ],
  "plugins": [ "@babel/proposal-class-properties" ]
}`
3. Также как и `plugins` устанавливаются через `npm`