# Файл конфигураций browserslist

# `package.json`

Добавить этот параметр 

    "browserslist": [
        "last 2 chrome version",
        "last 2 firefox version"
      ],

**В итоге получим**

    $ npx babel src --out-dir build
    @babel/preset-env: `DEBUG` option
    
    Using targets:
    {
      "chrome": "80",
      "firefox": "74"
    }
    
    Using modules transform: auto
    
    Using plugins:
      syntax-nullish-coalescing-operator { "chrome":"80", "firefox":"74" }
      syntax-optional-chaining { "chrome":"80", "firefox":"74" }
      syntax-json-strings { "chrome":"80", "firefox":"74" }
      syntax-optional-catch-binding { "chrome":"80", "firefox":"74" }
      syntax-async-generators { "chrome":"80", "firefox":"74" }
      syntax-object-rest-spread { "chrome":"80", "firefox":"74" }
      transform-dotall-regex { "firefox":"74" }
      proposal-unicode-property-regex { "firefox":"74" }
      transform-named-capturing-groups-regex { "firefox":"74" }
      transform-modules-commonjs { "chrome":"80", "firefox":"74" }
      proposal-dynamic-import { "chrome":"80", "firefox":"74" }
    
    Using polyfills: No polyfills were added, since the `useBuiltIns` option was not set.
    Successfully compiled 1 file with Babel.

# .browserslistrc

Создаем в корне проекта файл `.browserslistrc`

И указываем по 1 значение на строку

    last 2 chrome version
    last 2 firefox version

**В итоге получим тот же результат**

    $ npx babel src --out-dir build
    @babel/preset-env: `DEBUG` option
    
    Using targets:
    {
      "chrome": "80",
      "firefox": "74"
    }
    
    Using modules transform: auto
    
    Using plugins:
      syntax-nullish-coalescing-operator { "chrome":"80", "firefox":"74" }
      syntax-optional-chaining { "chrome":"80", "firefox":"74" }
      syntax-json-strings { "chrome":"80", "firefox":"74" }
      syntax-optional-catch-binding { "chrome":"80", "firefox":"74" }
      syntax-async-generators { "chrome":"80", "firefox":"74" }
      syntax-object-rest-spread { "chrome":"80", "firefox":"74" }
      transform-dotall-regex { "firefox":"74" }
      proposal-unicode-property-regex { "firefox":"74" }
      transform-named-capturing-groups-regex { "firefox":"74" }
      transform-modules-commonjs { "chrome":"80", "firefox":"74" }
      proposal-dynamic-import { "chrome":"80", "firefox":"74" }
    
    Using polyfills: No polyfills were added, since the `useBuiltIns` option was not set.
    Successfully compiled 1 file with Babel.

## Список браузеров

Команда которая покажет список браузеров поддерживающихся нашим списком в `package.json` или `.browserslist`

    npx browserslist

**Результат**

    $ npx browserslist
    chrome 81
    chrome 80
    firefox 75
    firefox 74

# Итог

Есть несколько вариантов что бы указать список браузеров для Babel

- В файле `.babelrc` - нужно использовать конфигурацию babel-preset-env и в блоке `targets`
- В файле `package.json` - нужно добавить блок `browserslist`
- Файл `.browserslist` - в каждой строке по отдельному выражению