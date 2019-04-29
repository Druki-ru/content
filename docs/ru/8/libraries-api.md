---
id: libraries-api
title: Libraries API
core: 8
search-keywords:
 - где как подключать JS JavaScript CSS добавить на страницу
 - куда вставить JS CSS скрипт
metatags:
  title: 'Drupal 8: Libraries API'
  description: 'Подключаем JavaScript и CSS в Drupal 8 правильно!'
---

В Drupal 8 CSS и JS загружаются по принципу ассетов. Библиотека может содержать один и более CSS файлов, один и более JS файлов, а также JS настройки.

Drupal загружает только те библиотеки на странице, которые были запрошены в момент её построения. Таким образом, Drupal не загружает все существующие библиотеки на одной странице, так как это вредит производительности.

## Объявление библиотеки

Для того чтобы объявить библиотеку, добавьте файл `*.libraries.yml` в корне вашего модуля или темы (рядом с .info.yml файлом). Например, если ваш модуль называется `foo`, то файл должен иметь название `foo.libraries.yml`.

Данный файл может объявлять сколько угодно библиотек. Каждая библиотека в файле описывает какие ассеты необходимо подключать, например:

```yaml
cuddly-slider:
  version: 1.x
  css:
    layout:
      css/cuddly-slider-layout.css: {}
    theme:
      css/cuddly-slider-theme.css: {}
  js:
    js/cuddly-slider.js: {}
```

Вы можете обратить внимание на то, что у стилей есть дополнительные вложения в виде layout и theme, который отсутствуют у JS. Эти вложения обозначают какого типа данные стили.

Данные типы влияют на вес файлов при их подключении и всего их присутствует 5 уровней:

- `base`: CSS reset, нормалайзы, оформление HTML элементов. Назначается вес `CSS_BASE = -200`.
- `layout`: Структура страницы, например подключение различных сеток. Назначается вес `CSS_LAYOUT = -100`.
- `component`: Переиспользуемые элементы интерфейса. Назначается вес `CSS_COMPONENT = 0`.
- `state`: Стили, которые отображает различные состояния компонентов. Назначается вес `CSS_STATE = 100`.
- `theme`: Оформление компонентов. Назначается вес `CSS_THEME = 200`.

Данные уровни являются частью [SMACSS](https://smacss.com/) стандарта. Вы не можете добавлять новые ключи.

Пример выше предполагает что JavaScript файл `cuddy-slider.js` находится в папке `js` вашего модуля или темы. Если вам необходимо подключить библиотеку относительно корня Drupal ядра, путь должен начинаться с `/`. Библиотека будет называться `cuddly-slider`.

> [!NOTE]
> Используя {Drush}(drush), вы можете сгенерировать заготовку для библиотеки, как для модуля, так и для темы используя команды: `drush generate module-libraries`, `drush generate theme-libraries`, `drush generate yml-module-libraries`, `drush generate yml-theme-libraries`.

## Подключение зависимостей

Допустим вы подключаете JavaScript файл, которому необходимо jQuery для работы. jQuery поставляется Drupal ядром по умолчанию, и вам не нужно заботиться об этом. Но подключая вашу библиотеку на странице, jQuery не подключится, так как Drupal не будет знать, что он нужен. Для этого, библиотекам можно указывать зависимости, которые также необходимо подключить на странице где будет вызвана ваша библиотека.

Для этого вам потребуется знать название библиотеки, и модуль или тему, которая его объявляет. Зависимости указываются в формате `[module-theme-name]/[library-name]`. Зависимости указываются в разделе dependencies конкретной библиотеки. 

```yaml
cuddly-slider:
  version: 1.x
  css:
    layout:
      css/cuddly-slider-layout.css: {}
    theme:
      css/cuddly-slider-theme.css: {}
  js:
    js/cuddly-slider.js: {}
  dependencies:
    - core/jquery
```

В дальнейшем, другие библиотеки также смогут использовать вашу библиотеку `[module-theme-name]/cuddly-slider` в качестве зависимости.

## Свойства ассетов

Вы можете добавлять свойства к своим библиотекам. Распологаются они после пути до файла в фигурных скобках `{}`. Все ассеты должны иметь данное значение, как минимум пустой маппинг, сами же свойства являются опциональными.

### attributes

- **Применимо**: CSS и JS

Позволяет задать аттрибуты к тегу подключения.

Например:

```yaml
cuddly-slider:
  version: 1.x
  css:
    layout:
      css/cuddly-slider-layout.css: {
        attributes: {
          crossorigin: anonymous
        }
      }
```

В результате добавит на страницу:

```html
<link rel="stylesheet" href="/modules/custom/MODULENAME/css/cuddly-slider-layout.css" crossorigin="anonymous">
```

### browsers

- **Применимо**: CSS и JS

Позволяет задать условия, для каких браузеров данный ассет будет подключен.

Например:

```yaml
cuddly-slider:
  version: 1.x
  css:
    layout:
      css/cuddly-slider-layout.css: {
        browsers: {
          IE: 'lte IE 9'
          '!IE': false
        }
      }
```

В результате добавит на страницу:

```html
<!--[if lte IE 9]>
<script src="/modules/custom/MODULENAME/css/cuddly-slider-layout.css"></script>
<![endif]-->
```

### media

- **Применимо**: CSS
- **По умолчанию**: `all`

Позволяет задать медиа тип, для которого загружать данный ассет.

Например:

```yaml
cuddly-slider:
  version: 1.x
  css:
    layout:
      css/cuddly-slider-layout.css: {
        media: print
      }
```

В результате добавит на страницу:

```html
<link rel="stylesheet" href="/modules/custom/MODULENAME/css/cuddly-slider-layout.css" media="print">
```

### minified

- **Применимо**: CSS и JS
- **По умолчанию**: `false`

Позволяет указать, что данный ассет уже минифицирован и его не следует обрабатывать.

Например:

```yaml
cuddly-slider:
  version: 1.x
  css:
    layout:
      css/cuddly-slider-layout.css: {
        minified: true
      }
```

### preprocess

- **Применимо**: CSS и JS
- **По умолчанию**: `true`

Определяет, должен ли данный ассет аггрегироваться. Например, если на странице подключается 10 CSS файлов, и у всех стоит `true`, то при включении кэширования, данные файлы будут собраны в один, а те, у которых данное значение стоит `false`, будут подключены отдельно.

### weight

- **Применимо**: CSS и JS
- **По умолчанию**: 0

Позволяет менят вес ассета в пределах своей группы. Чем больше вес, тем позднее будет подключена библиотека.

## Подключение библиотек

Для того чтобы ассеты библиотеки были загружены на странице, к странице нужно указать зависимость от данной библиотеки. Это можно сделать различными способами и на разных этапах выполнения.

### Подключение через render array

Для того чтобы подключить библиотеку при помощи render array, вы должны модифицировать его примерно таким образом:

```php
$build['some_render_element']['#attached']['library'][] = 'mymodule/cuddly-slider';
```

Данный способ применим ко всем render array, не зависимо где вы находитесь. Это может касаться hook theme, форм, препроцесса страниц, render element и др.

### Подключение библиотеки из Twig

Вы можете подключить библиотеку к шаблону, при помощи испоьзования необходимого `hook_preprocess_HOOK()` и способа подключения через render array, а также при помощи специальной Twig функции `attach_library()`.

```twig
{{ attach_library('mymodule/cuddly-slider') }}
<div>Some markup {{ message }}</div>
```

## Ссылки

- [Adding stylesheets (CSS) and JavaScript (JS) to a Drupal 8 module](https://www.drupal.org/docs/8/creating-custom-modules/adding-stylesheets-css-and-javascript-js-to-a-drupal-8-module) (англ)
- [Adding stylesheets (CSS) and JavaScript (JS) to a Drupal 8 theme](https://www.drupal.org/docs/8/theming/adding-stylesheets-css-and-javascript-js-to-a-drupal-8-theme) (англ)
- [Drupal 8: Libraries API (Добавление CSS/JS на страницы)](https://niklan.net/blog/72), Niklan, 2015
