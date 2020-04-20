# Сборка под конкретный браузер

Трансформировать все что можно через Babel - совсем не нужно и на это есть несколько причин

1. Важно понять кто будет целевой аудиторией проекта, если это внутренний проект компании, где все пользуются свежей версией Chrome или Firefox, то зачем трансформировать код в более старые версии
2. Трансформированный код будет больше по размеру и дольше и сложнее обрабатываться браузером
3. Новые спецификации лучше обрабатываются браузером, благодаря самому браузеру и его внутренним возможностей

# Проверить какой браузер поддерживает

Можно с помощью сайта

[https://caniuse.com/](https://caniuse.com/)

# Автоматизация и поддержка конкретных версий

`babel-preset-env` позволяет указывать список браузеров, на которых должен запускаться код

    "presets": [["@babel/env", {
        "targets": {
          "edge": "18",
          "chrome": "74"
        }
      }]],

# Динамический выбор

Мы конечно не будем следить за каждой версией браузера, поэтому удобнее использовать динамическую выборку браузеров

Так мы можем указать `"last 2 chrome version"` и код будет трансформироваться под последнее две версии Chrome

Но как узнать последние две версии браузера ? Для этого нужно включить мод `debug` , что бы в консоли видеть конкретные числа и значение

    "presets": [["@babel/env", {
        "debug": true,
        "targets": [
          "last 2 chrome version",
          "last 2 firefox version",
          "last 2 ios version"
        ]
      }]],

После запуска Babel получаем следующий результат

    $ npx babel src --out-dir build
    @babel/preset-env: `DEBUG` option
    
    Using targets:
    {
      "chrome": "80",
      "firefox": "74",
      "ios": "13.2"
    }
    
    Using modules transform: auto
    
    Using plugins:
      proposal-nullish-coalescing-operator { "ios":"13.2" }
      proposal-optional-chaining { "ios":"13.2" }
      syntax-json-strings { "chrome":"80", "firefox":"74", "ios":"13.2" }
      syntax-optional-catch-binding { "chrome":"80", "firefox":"74", "ios":"13.2" }
      syntax-async-generators { "chrome":"80", "firefox":"74", "ios":"13.2" }
      syntax-object-rest-spread { "chrome":"80", "firefox":"74", "ios":"13.2" }
      transform-dotall-regex { "firefox":"74" }
      proposal-unicode-property-regex { "firefox":"74" }
      transform-named-capturing-groups-regex { "firefox":"74" }
      transform-modules-commonjs { "chrome":"80", "firefox":"74", "ios":"13.2" }
      proposal-dynamic-import { "chrome":"80", "firefox":"74", "ios":"13.2" }
    
    Using polyfills: No polyfills were added, since the `useBuiltIns` option was not set.
    Successfully compiled 1 file with Babel.

За этот функционал отвечает библиотек `browserlist`

[browserslist/browserslist](https://github.com/browserslist/browserslist)

## Более гибкие условия

К примеру можно указать такую конструкцию, которая покажет, что нужно использовать браузеры, которыми пользуются более 0.3% людей во всем мире

    {
      "presets": [["@babel/env", {
        "debug": true,
        "targets": [
          "> 0.3%"
        ]
      }]],
      "plugins": [ "@babel/proposal-class-properties" ]
    }

Если мы запустим Babel с такими настройками то получим намного больший список браузеров и список трансформаций в том числе

    npx babel src --out-dir build
    @babel/preset-env: `DEBUG` option
    
    Using targets:
    {
      "chrome": "49",
      "edge": "18",
      "firefox": "73",
      "ie": "9",
      "ios": "12.2",
      "opera": "66",
      "safari": "12.1",
      "samsung": "10.1"
    }
    
    Using modules transform: auto
    
    Using plugins:
      proposal-nullish-coalescing-operator { "chrome":"49", "edge":"18", "ie":"9", "ios":"12.2", "opera":"66", "safari":"12.1", "samsung":"10.1" }
      proposal-optional-chaining { "chrome":"49", "edge":"18", "firefox":"73", "ie":"9", "ios":"12.2", "opera":"66", "safari":"12.1", "samsung":"10.1" }
      proposal-json-strings { "chrome":"49", "edge":"18", "ie":"9" }
      proposal-optional-catch-binding { "chrome":"49", "edge":"18", "ie":"9" }
      transform-parameters { "ie":"9" }
      proposal-async-generator-functions { "chrome":"49", "edge":"18", "ie":"9" }
      proposal-object-rest-spread { "chrome":"49", "edge":"18", "ie":"9" }
      transform-dotall-regex { "chrome":"49", "edge":"18", "firefox":"73", "ie":"9" }
      proposal-unicode-property-regex { "chrome":"49", "edge":"18", "firefox":"73", "ie":"9" }
      transform-named-capturing-groups-regex { "chrome":"49", "edge":"18", "firefox":"73", "ie":"9" }
      transform-async-to-generator { "chrome":"49", "ie":"9" }
      transform-exponentiation-operator { "chrome":"49", "ie":"9" }
      transform-template-literals { "ie":"9", "ios":"12.2", "safari":"12.1" }
      transform-literals { "ie":"9" }
      transform-function-name { "chrome":"49", "edge":"18", "ie":"9" }
      transform-arrow-functions { "ie":"9" }
      transform-block-scoped-functions { "ie":"9" }
      transform-classes { "ie":"9" }
      transform-object-super { "ie":"9" }
      transform-shorthand-properties { "ie":"9" }
      transform-duplicate-keys { "ie":"9" }
      transform-computed-properties { "ie":"9" }
      transform-for-of { "chrome":"49", "ie":"9" }
      transform-sticky-regex { "ie":"9" }
      transform-unicode-regex { "chrome":"49", "ie":"9" }
      transform-spread { "ie":"9" }
      transform-destructuring { "chrome":"49", "ie":"9" }
      transform-block-scoping { "ie":"9" }
      transform-typeof-symbol { "ie":"9" }
      transform-new-target { "ie":"9" }
      transform-regenerator { "chrome":"49", "ie":"9" }
      transform-modules-commonjs { "chrome":"49", "edge":"18", "firefox":"73", "ie":"9", "ios":"12.2", "opera":"66", "safari":"12.1", "samsung":"10.1" }
      proposal-dynamic-import { "chrome":"49", "edge":"18", "firefox":"73", "ie":"9", "ios":"12.2", "opera":"66", "safari":"12.1", "samsung":"10.1" }
    
    Using polyfills: No polyfills were added, since the `useBuiltIns` option was not set.
    Successfully compiled 1 file with Babel.

### Убрать IE

Это можно сделать так

    "targets": [
        "> 0.3%",
        "not ie > 0"
    ]

**В результате получим**

    $ npx babel src --out-dir build
    @babel/preset-env: `DEBUG` option
    
    Using targets:
    {
      "chrome": "49",
      "edge": "18",
      "firefox": "73",
      "ios": "12.2",
      "opera": "66",
      "safari": "12.1",
      "samsung": "10.1"
    }
    
    Using modules transform: auto
    
    Using plugins:
      proposal-nullish-coalescing-operator { "chrome":"49", "edge":"18", "ios":"12.2", "opera":"66", "safari":"12.1", "samsung":"10.1" }
      proposal-optional-chaining { "chrome":"49", "edge":"18", "firefox":"73", "ios":"12.2", "opera":"66", "safari":"12.1", "samsung":"10.1" }
      proposal-json-strings { "chrome":"49", "edge":"18" }
      proposal-optional-catch-binding { "chrome":"49", "edge":"18" }
      proposal-async-generator-functions { "chrome":"49", "edge":"18" }
      proposal-object-rest-spread { "chrome":"49", "edge":"18" }
      transform-dotall-regex { "chrome":"49", "edge":"18", "firefox":"73" }
      proposal-unicode-property-regex { "chrome":"49", "edge":"18", "firefox":"73" }
      transform-named-capturing-groups-regex { "chrome":"49", "edge":"18", "firefox":"73" }
      transform-async-to-generator { "chrome":"49" }
      transform-exponentiation-operator { "chrome":"49" }
      transform-template-literals { "ios":"12.2", "safari":"12.1" }
      transform-function-name { "chrome":"49", "edge":"18" }
      transform-for-of { "chrome":"49" }
      transform-unicode-regex { "chrome":"49" }
      transform-destructuring { "chrome":"49" }
      transform-regenerator { "chrome":"49" }
      transform-modules-commonjs { "chrome":"49", "edge":"18", "firefox":"73", "ios":"12.2", "opera":"66", "safari":"12.1", "samsung":"10.1" }
      proposal-dynamic-import { "chrome":"49", "edge":"18", "firefox":"73", "ios":"12.2", "opera":"66", "safari":"12.1", "samsung":"10.1" }
    
    Using polyfills: No polyfills were added, since the `useBuiltIns` option was not set.
    Successfully compiled 1 file with Babel.

# Итоги

- `babel-preset-env` позволяет использовать выражения, чтобы описать список браузеров
- `browserlist` отвечает за парсинг выражений
`"targets" : ["last 2 chrome version", "> 0.3%"]`
- Добавив в конфигурации `babel-preset-env` флаг `debug: true`, можно получить детальную информацию о браузерах и трансформациях