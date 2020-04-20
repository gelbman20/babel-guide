# Работа с JSX

# Напишем исходный JSX код main.js

    import React from 'react'
    import ReactDOM from 'react-dom'
    
    const App = () => <p>Hello World</p>
    
    ReactDOM.render(<App />, document.getElementById('root'))

# Установит `react` `react-dom`

    npm i react react-dom

# Установим `preset` для React

    npm i -D @babel/preset-react

Этот пресет содержит несколько плагинов который поможет трансформировать JSX

# Добавим `preset-react` в `.babelrc`

    {
      "presets": [
        [
          "@babel/env",
          {
            "corejs": 3,
            "useBuiltIns": "usage",
            "debug": true,
            "modules": false
          }
        ],
        [
          "@babel/react"
        ]
      ],
      "plugins": [
        "@babel/proposal-class-properties"
      ]
    }

# Итоге

- `@babel/preset-react` - содержит трансформации, необходимые для работы с `JSX` кодом
- `npm i -D @babel/preset-react`
- конфигурация `.babelrc`

    "presets": [
    	[
    		"@babel/env", {...}
    	],
    	[
    		"@babel/react"
    	]
    ]