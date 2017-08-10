
У вас должны быть установлены:

- [nodejs и npm](https://nodejs.org/)
- [gulp](http://gulpjs.com/)
- [yarn](https://yarnpkg.com/)

Далее выполняем:

```
yarn
gulp build
```

После выкачивания всех зависимостей проекта и успешной первичной сборки, запустите задачу `gulp` или `gulp serve` и можете приступать к работе.

## Что включено?

- [BrowserSync](http://www.browsersync.io/)
- компиляция jade и sass
- сжатие стилей, скриптов ([ES2015](https://github.com/lukehoban/es6features)) и изображений
- проверка javascript-кода на наличие ошибок ([eslint](http://eslint.org/))

### BrowserSync

Задача `gulp serve` откроет вкладку в браузере, и в консоли вы увидите local и external пути до вашего проекта. Так вы сможете отлаживать верстку сразу в нескольких браузерах и устройствах.

### Jade

В папке src/jade расположены шаблоны для генерации. После выполнения задачи `gulp jade` в корне проекта появятся html-файлы, которые будут результатом компиляции из вышеуказанной папки с исходниками.

По умолчанию имеется один index.jade, в котором импортируются includes/head.jade, includes/header.jade и includes/footer.jade. Их стоит использовать в других подобных создаваемых шаблонах.

Файлы, расположенные в папке src/jade/includes, не компилируются.

### Стили

Все стили находятся в папке src/sass. Gulp собирает их в один файл main.min.css и кладет в папку dist/css.

Все делится на логические блоки (файлы). Если вы хотите добавить еще один, создайте новый sass-файл и подключите его в main.sass.

Вы можете активировать плагин [gulp-uncss](https://www.npmjs.com/package/gulp-uncss), чтобы убрать ненужные стили (см. файл gulpfile.js: задача `sass-build`).

Стоит придерживаться единому стилю написания кода, чтобы обеспечить высокую скорость разработки и поддерживаемость ваших проектов.

#### Postcss

В шаблоне для изменения скомпилированного css-кода используются [плагины](http://postcss.parts/) [Postcss](https://github.com/postcss/postcss).

- [autoprefixer](https://github.com/postcss/autoprefixer) (автоматически проставляет браузерные префиксы)
- [cssnano](https://github.com/ben-eb/cssnano) (сжимает css-код)

### Изображения

За сжатие изображений отвечает задача `gulp images` (в том числе `gulp webp`). Вы можете вызывать ее самостоятельно, либо запустить `gulp`, и после этого каждый файл изображения, размещенный в src/img (включая svg), будет сжат и положен в dist/img с тем же именем. Также используется генерация [WebP-изображений](https://developers.google.com/speed/webp/). Их можно использовать так:

**html:**

```
<picture>
	<source srcset="dist/img/image.webp" type="image/webp">
	<img src="dist/img/image.jpg">
</picture>
```

**css (вместе с [modernizr](https://modernizr.com/)):**

```
body
    background-position: center center
    background-attachment: fixed

.no-webp body
    background-image: url(#{$img}/bg-main.jpg)

.webp body
    background-image: url(#{$img}/bg-main.webp)
```

Вы можете использовать [систему символов SVG](https://css-tricks.com/svg-symbol-good-choice-icons/). Эти символы описываются единожды в самом начале документа (для этого заводится один скрытый тег svg), а затем их можно использовать повторно где-либо на страницах, менять их заливку, размер, применять трансформации (поворот и др.), а также анимировать какие-либо значения (fill и др.) с помощью [transition](http://www.w3schools.com/css/css3_transitions.asp) или применять сложную анимацию на основе [keyframes](http://www.w3schools.com/cssref/css3_pr_animation-keyframes.asp).

Для автоматизации процесса хорошо подходит плагин [gulp-svgstore](https://www.npmjs.com/package/gulp-svgstore). Именно он используется в Andylight. Все иконки в виде скрытого svg автоматически добавляются в шаблон, располагаясь сперва в подключаемом js-файле, а затем встраиваясь в страницу. Просто кладите нужные иконки в папку `src/img/svg/icons` и используйте их на страницах с помощью конструкции *svg use* (задача `gulp svgstore`).

#### Favicons

Для генерации favicons вы можете пользоваться сервисом [RealFaviconGenerator](http://realfavicongenerator.net/). Положите сгенерированные иконки в папку `/src/favicons`.

### Скрипты

Для сборки ваших и сторонних модулей используется [Browserify](http://browserify.org/).

Все необходимые пакеты устанавливаются через `yarn add package`

Входная точка приложения - src/js/app.js.

### Финальная сборка

`gulp build`

