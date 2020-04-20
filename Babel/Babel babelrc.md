# Конфигурация Babel (.babelrc)

Для быстрого запуска Babel с консоли можно использовать ранее написанную команду 

    npx babel src --out-dir build --pluggins @babel/plugin-transform-literals, @babel/plugin-transform-classes, @babel/plugin-transform-block-scoping

Но использовать такую длинную команду мало того, что не удобно, это и не практично.

# Конфигурационные файлы

Файл  `.babelrc` - обычный JSON файл

    {
      "plugins": [
        "@babel/plugin-transform-literals", 
        "@babel/plugin-transform-classes", 
        "@babel/plugin-transform-block-scoping"
      ]
    }

Теперь достаточно прописать следующую команду и Babel автоматически подхватит конфигурационный файл

    npx babel src --out-dir build

# Другие моменты конфигураций

[https://babeljs.io/docs/en/configuration](https://babeljs.io/docs/en/configuration)

# Итоги

1. Лучше настройки Babel описывать в конфигурационном фале, он может быть
    1. JSON файлом .babelrc - что чаще всего принято
    2. JS файлом
2. Такие настройки проще и удобнее поддерживать