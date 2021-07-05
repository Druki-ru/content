---
title: 'Drupal 9.2.1'
slug: 9/releases/9.2.1
core: 9
metatags:
  title: 'Drupal 9.2.1: Список изменений'
  description: 'Список изменений Drupal 9.2.1.'
---

> [!WARNING]
> Данная версия находится в разработке.

## File System

* [#2228087](https://www.drupal.org/project/drupal/issues/2228087) В интерфейс `PhpStreamWrapperInterface` добавлена документация к методам, а классы, реализующие его теперь ссылаются на его документацию.

## Help

* [#3067727](https://www.drupal.org/project/drupal/issues/3067727) Документация для модулей `comment`, `node`, `path` и `taxonomy` была конвертирована из `hook_help()` в Help Topic.
* [#3094482](https://www.drupal.org/project/drupal/issues/3094482) Документация для модуля `action` была конвертирована из `hook_help()` в Help Topic.
* [#3095739](https://www.drupal.org/project/drupal/issues/3095739) Документация для модулей `contextual`, `help`, `inline_form_errors`, `quickedit`, `settings_tray`, `shortcut`, `toolbar`  и `tour` была конвертирована из `hook_help()` в Help Topic.

## Media System

* [#3097416](https://www.drupal.org/project/drupal/issues/3097416) При встраивании медиа при помощи CKEditor, теперь пользователь может выбрать только тот режим отображения, которые включены для данного типа медиа.

## Migration System

* [#3213621](https://www.drupal.org/project/drupal/issues/3213621) Исправлена фикстура для миграций Drupal 7.
* [#3053167](https://www.drupal.org/project/drupal/issues/3053167) Мультиязычные свойства `migrate_drupal.migrate_drupal.yml` были вынесены в соответствующие миграции.
* [#3164520](https://www.drupal.org/project/drupal/issues/3164520) `FieldableEntity::getFieldValues()` теперь гарантирует что значения поля будут отсортированы по дельте хранения. Ранее, отсутствие данной сортировки, могло приводить к миграции значений поле в обратном порядке.
* [#3209353](https://www.drupal.org/project/drupal/issues/3209353) Добавлена отсутствующая документация к плагинам источникам предоставляемые модулями `node` и `taxonomy`.
* [#3196583](https://www.drupal.org/project/drupal/issues/3196583) `MigrationLookup` теперь может сам определить ID источников в перечисленных миграциях.
* [#3103031](https://www.drupal.org/project/drupal/issues/3103031) В плагин источник `FieldOptionTranslation` значение `bundle` используется как ID источника.

## Node System

* [#3048848](https://www.drupal.org/project/drupal/issues/3048848) Блок `SyndicateBlock` теперь генерирует корректный URL для `rss.xml` с использованием `Url::fromUri()`, а не хардкод значение.

## Тестирование

* [#3213734](https://www.drupal.org/project/drupal/issues/3213734) В `AssertButtonsTrait` исправлен некорректный синтаксис.
* [#2879159](https://www.drupal.org/project/drupal/issues/2879159) Исправлены вызовы `::assertEquals()` с некорректным порядком передачи аргументов.
* [#3156396](https://www.drupal.org/project/drupal/issues/3156396) Для сравнений для подсчёта размера и количества теперь используется `::assertSameSize()`.
* [#3215143](https://www.drupal.org/project/drupal/issues/3215143) Комментарии ссылающиеся на несуществующий `::assertEqual()` были обновлены.
* [#3175718](https://www.drupal.org/project/drupal/issues/3175718) Исправлена неполадка из-за которой проверка на `theme_token` могла случайным образом проваливаться. Это связано с тем что `\Behat\Mink\Element\DocumentElement::getText()` возвращает текст очищенный от script тегов внутри `<body>` элемента, которые используются Drupal для передачи Drupal Settings.