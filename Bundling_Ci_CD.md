# Сборка CI CD
* webpack
* отличия webpack loader / plugin
* babel
***

**webpack** - bundler (сборщик модулей). С помощью плагинов умеет: 
* Собирать в один файл js (при необходимости минифицировать, кастовать uglify)
* транспилировать JavaScript следующего поколения до более старого стандарта JavaScript (ES5) с помощью Babel
* может конвертировать встроенные изображения в data:URI, а также импортировать изображения.
* позволяет использовать *require()* для CSS файлов
* webpack-dev-server (в нём встроен локальный сервер и livereload (“живая перезагрузка браузера”))
* может работать с Hot Module Replacement (замена модулей в рантайм)
* разделить бандл на чанки
* Tree Shaking

**Loader** - принимает содержимое файла, преобразовывают и включают результат в общую сборку.
* стили `css-loader, sass-loader, postcss-loader`
* js, ts - `babel-loader, ts-loader`
* файлы - `url-loader, file-loader`
* svg - `svgo-loader`

**Plugins** - встраиваются в pipline и преобразовывают код. Наиболее популярные: 
* HtmlWebpackPlugin - создает html и подключает скрипты
* HotModuleReplacementPlugin - частичная замена модулей при изменении кода в рантайме
* DefinePlugin - добавляет гобальные переменные webpack
* ExtractTextWebpackPlugin извлекает текст (css) в отдельный файл
* bundleAnalyzer - создает отчет по составу бандла с весом модулей
* NoEmitOnErrorsPlugin - пропускает ошибки на этапе компиляции
* CopyWebpackPlugin - копирует файлы в сборочную директорию (например статику)

**Babel** - преобразователь кода из ECMAScript 2015+ в более ранние версии для поддержки браузером. Преобразовывает синтакс, применяет полифилы, транспилирует jsx. Пример конфига:
```json
{
  "plugins": [
    "babel-plugin-styled-components",
    "@babel/plugin-proposal-class-properties",
    "@babel/plugin-transform-runtime"
  ],
  "presets": ["@babel/preset-env", "@babel/preset-react"]
}
```
