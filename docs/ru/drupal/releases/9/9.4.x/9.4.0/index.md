---
title: 'Drupal 9.4.0'
slug: drupal/releases/9.4.0
core: 9
metatags:
  title: 'Drupal 9.4.0: Список изменений'
  description: 'Список изменений Drupal 9.4.0.'
---

> [!WARNING]
> Данная версия находится в разработке. Список изменений может меняться и дополняться, даты могут смещаться.

**Дата релиза**: июнь 2022\
**Активная поддержка до**: декабрь 2022\
**Поддержка безопасности до**: июнь 2023

## Хук `THEME_ENGINE_init()` объявлен устаревшим

* [#3158289](https://www.drupal.org/node/3158289) 

Хук `THEME_ENGINE_init()` объявлен устаревшим и будет удалён в [Drupal 10](../../../../10/index.md). Замена данному хуку не предоставляется.

## Claro

* [#3214170](https://www.drupal.org/node/3214170) Кнопка «Отмена» теперь отцентрована в off-canvas модальном окне.
* [#3247994](https://www.drupal.org/node/3247994) Исправлена неполадка, из-за которой мог некорректно работать элемент формы `password`.

## Composer

* [#3246595](https://www.drupal.org/node/3246595) Зависимости ядра обновлены на 01.11.21.

## Database System

* [#3213644](https://www.drupal.org/node/3213644) Внесены улучшения в тест `StatementWrapperLegacyTest::testClientStatementMethod()`.

## Filter

* [#3227821](https://www.drupal.org/node/3227821) Исправлена неполадка в фильтре «Заменять переводы строк соответствующими HTML-тегами», которая могла приводить к поломке SVG элементов.

## Image

* [#3223233](https://www.drupal.org/node/3223233) Улучшены заголовки страниц для форм добавления и редактирования [стилей изображений](../../../../9/image/image-styles/index.md).

## JavaScript

* [#3238860](https://www.drupal.org/node/3238860) Использование `jQuery.map()` заменено на нативную `map()` функцию.

## Migration System

* [#3246053](https://www.drupal.org/node/3246053) Обновлено значение `filesize` файла `ds9.txt` в `file_managed`.
* [#3163663](https://www.drupal.org/node/3163663) Исправлена неполадка из-за которой плагин-обработчик `download` мог приводить к предупреждению «failed to open stream: Too many open file».
* [#3087332](https://www.drupal.org/node/3087332) Плагин-обработчик `d6_url_alias_language` объявлен устаревшим.

## Olivero

* [#3186992](https://www.drupal.org/node/3186992) Исправлена неполадка, из-за которой навигационные пункты меню могли выходить за рамки контейнера.

## Views

* [#2569381](https://www.drupal.org/node/2569381) Из `Drupal\views\Plugin\views\area\Result` удалён лишний вызов `XSS::adminFilter()`.

## Symfony 6

* [#3232074](https://www.drupal.org/node/3232074) Для классов расширяющих `Normalizer` добавлен тайпхинт `array|string|int|float|bool|\ArrayObject|null` методу `::normalize()`.
* [#3232095](https://www.drupal.org/node/3232095) Произведён рефакторинг сервиса `update.root` для того чтобы он возвращал объект вместо строки.
* [#3232131](https://www.drupal.org/node/3232131) В `DebugClassLoader` добавлены тайпхинты.

## Прочие изменения

* [#3038596](https://www.drupal.org/node/3038596) В `drupalci.yml` добавлено напоминание о том, что данный файл требуется в ручном изменении при создании ново ветки ядра.
* [#3245820](https://www.drupal.org/node/3245820) Из кода удалены упоминания удалённого `\Drupal\Core\Action\Plugin\Action\PublishAction`.