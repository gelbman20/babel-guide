# Плагины

Для каждого преобразования и для каждой конструкции языка, которую будет преобразовывать Babel нужны отдельные плагины.

Babel плагины - это обычное `npm` пакеты

К примеру для преобразование Template Literals - ```` используется плагин 

`@babel/transform-template-literals`

Установить этот плагин можно с помощью команды

    npm i -D @babel/plugin-transform-template-literals

# Итоги

Для того что бы трансформировать каждый отдельный синтаксический код - для этого используются плагины, каждый из которых отвечает за определенную роль.