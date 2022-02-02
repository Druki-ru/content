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

## Обновлены старые схемы для `uid` поля сущностей, где может использоваться устаревший метод получения ID пользователя

* [#3153455](https://www.drupal.org/node/3153455) Добавлено обновление, которое исправляет

Сайты, установленные на версии ранее чем Drupal 8.6.0, а теперь использующие Drupal 9, могут столкнуться с проблемой,
что у некоторых сущностей из ядра поле `uid` использует устаревшее значение для `default_value_callback`. Ранее,
использовался метод `{EntityTypeClass}::getCurrentUserId()`, который был объявлен устаревшим в Drupal 8.6.0 и удалён в
Drupal 9.

На таких проектах, подобная ситуация может приводить, как к ошибке: «The website encountered an unexpected error. Please
try again later. Error: Call to a member function getAccountName() on null», так и предупреждению «call_user_func()
expects parameter 1 to be a valid callback, class 'Drupal\node\Entity\Node' does not have a method 'getCurrentUserId' in
Drupal\Core\Field\FieldConfigBase->getDefaultValue()».

В данном релизе добавлены обновления для всех сущностей ядра, которые проверяют, что используется современный метод, и
если обнаруживается старый, он обновляется на новый.

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
* [#3259380](https://www.drupal.org/node/3259380) Исправлена неполадка, из-за которой тулбар CKEditor в режиме фокусировки перекрывал тулбар от Drupal.
* [#3232550](https://www.drupal.org/node/3232550) Улучшены сообщения отображаемые при использовании Internet Explorer.
* [#3261712](https://www.drupal.org/node/3261712) Добавлены тесты для проверки Media кнопок.

## Database System

* [#3256056](https://www.drupal.org/node/3256056) В `QueryAggregateInterface` добавлена документация о том что в запросах также можно использовать оператор `NOT BETWEEN`.
* [#3261629](https://www.drupal.org/node/3261629) Команда `DbDumpCommand` теперь не зависит от драйверов баз данных.

## File System

* [#3254553](https://www.drupal.org/node/3254553) Исправлена неполадка, из-за которой `FileUrlGenerator::generate()` не работал со Stream Wrappers ведущие на внешние ресурсы.

## Filter

* [#3241633](https://www.drupal.org/node/3241633) Исправлена неполадка, из-за которой не отображались режимы отображения для Media сущностей, если они имеют числовое название.

## JavaScript

* [#3258371](https://www.drupal.org/node/3258371) Исправлена неполадка в команде `yarn vendor-update`.
* [#3246211](https://www.drupal.org/node/3246211) Stylelint обновлён до 14 версии.

## Render System

* [#3254328](https://www.drupal.org/node/3254328) Кеш-контексты и кеш-теги заполнителей (`placeholder`) теперь сортируются, прежде будут переданы на дальнейшую обработку. Это исправляет ошибку, из-за которой рендер некоторых блоков мог производиться дважды при использовании ленивых построителей.
* [#3172166](https://www.drupal.org/node/3172166) (отменено) Улучшена проверка в `Element::properties()`, которая приводила к «Notice:
  Trying to access array offset on value of type int» при попытке вызова метода с массивом в качестве аргумента, где ключи являлись числами.

## Toolbar

* [#3258642](https://www.drupal.org/node/3258642) Исправлена документация для `_toolbar_get_subtrees_hash()`.

## Тестирование

* [#3259744](https://www.drupal.org/node/3259744) Сообщение об устаревшем `Drupal\Tests\Listeners\DrupalListener` добавлено в исключения (не будет проваливать тест) для PHPUnit.

## Прочие изменения

* [#2612876](https://www.drupal.org/node/2612876) Исправлены ошибки в документации к `\Drupal\Core\Asset\CssOptimizer::processFile()`.
* [#3257654](https://www.drupal.org/node/3257654) Внесены изменения для исправления ошибок выявленных PHPStan L0.
* [#3106216](https://www.drupal.org/node/3106216) Из ядра удалены неиспользуемые переменные.
* [#3255245](https://www.drupal.org/node/3255245) Изменение [#3231683](https://www.drupal.org/node/3231683) из [Drupal 9.3.0](../9.3.0/index.md) было откачено.
* [#3247666](https://www.drupal.org/node/3247666) `README.txt` файл был актуализирован.
* [#3219649](https://www.drupal.org/node/3219649) Исправлены опечатки в словах начинающихся с «q» до «s».
* [#3258969](https://www.drupal.org/node/3258969) Исправлен некорректный заполнитель для `watchdog_exception()` в `ModuleInstaller` классе.
* [#3174402](https://www.drupal.org/node/3174402) В `TrackerTest` добавлена проверка `$unpublished` материала.