---
title: 'Drupal 9.3.4'
slug: wiki/drupal/releases/9.3.4
core: 9 
metatags:
  title: 'Drupal 9.3.4: Список изменений'
  description: 'Список изменений Drupal 9.3.4.'
authors:
  - Niklan
---

> [!WARNING]
> Данная версия находится в разработке и не предназначена для использования на живых сайтах.

## Claro

* [#3191527](https://www.drupal.org/node/3191527) Исправлена неполадка, из-за которой позиционирование диалогового окна могло быть некорректным.
* [#3247994](https://www.drupal.org/node/3247994) Исправлена неполадка, из-за которой могли некорректно добавляться CSS классы на элемент формы `password`.

## CKEditor 5

* [#3255077](https://www.drupal.org/node/3255077) Исправлены опечатки в описании для CKEditor 5 фильтра.
* [#3228778](https://www.drupal.org/node/3228778) Плагины для CKEditor 5 теперь могут использовать `Drupal.t()` для перевода строк.
* [#3238257](https://www.drupal.org/node/3238257) Если CKEditor заменил `<textarea>` на который вёл якорь, то теперь CKEditor перехыватает такой якорь и перенаправляет фокусировку на себя.
* [#3258668](https://www.drupal.org/node/3258668) Удалена лишняя разметка из `ckeditor5.admin.es6.js`.
* [#3259179](https://www.drupal.org/node/3259179) Внесены улучшения в `ckeditor5.ckeditor5.yml` файла, для более плавного переезда на него в будущем.
* [#3259174](https://www.drupal.org/node/3259174) Улучшен тест `SmartDefaultSettingsTest`.
* [#3248188](https://www.drupal.org/node/3248188) Добавлена валидация `drupal.conditions` для плагинов CKeditor5.
* [#3248177](https://www.drupal.org/node/3248177) Исправлена неполадка, из-за которой плагин пометки языка в тексте было невозможно убрать с тулбара.

## Database System

* [#3256056](https://www.drupal.org/node/3256056) В `QueryAggregateInterface` добавлена документация о том что в запросах также можно использовать оператор `NOT BETWEEN`.

## File System

* [#3254553](https://www.drupal.org/node/3254553) Исправлена неполадка, из-за которой `FileUrlGenerator::generate()` не работал со Stream Wrappers ведущие на внешние ресурсы.

## Filter

* [#3241633](https://www.drupal.org/node/3241633) Исправлена неполадка, из-за которой не отображались режимы отображения для Media сущностей, если они имеют числовое название.

## JavaScript

* [#3258371](https://www.drupal.org/node/3258371) Исправлена неполадка в команде `yarn vendor-update`.

## Render System

* [#3254328](https://www.drupal.org/node/3254328) Кеш-контексты и кеш-теги заполнителей (`placeholder`) теперь сортируются, прежде будут переданы на дальнейшую обработку. Это исправляет ошибку, из-за которой рендер некоторых блоков мог производиться дважды при использовании ленивых построителей.

## Тестирование

* [#3259744](https://www.drupal.org/node/3259744) Сообщение об устаревшем `Drupal\Tests\Listeners\DrupalListener` добавлено в исключения (не будет проваливать тест) для PHPUnit.

## Прочие изменения

* [#2612876](https://www.drupal.org/node/2612876) Исправлены ошибки в документации к `\Drupal\Core\Asset\CssOptimizer::processFile()`.
* [#3257654](https://www.drupal.org/node/3257654) Внесены изменения для исправления ошибок выявленных PHPStan L0.
* [#3106216](https://www.drupal.org/node/3106216) Из ядра удалены неиспользуемые переменные.
* [#3255245](https://www.drupal.org/node/3255245) Изменение [#3231683](https://www.drupal.org/node/3231683) из [Drupal 9.3.0](../9.3.0/index.md) было откачено.
* [#3247666](https://www.drupal.org/node/3247666) `README.txt` файл был актуализирован.