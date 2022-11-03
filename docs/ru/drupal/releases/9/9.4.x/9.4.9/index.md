---
title: 'Drupal 9.4.9'
slug: wiki/drupal/releases/9.4.9
core: 9
metatags:
  title: 'Drupal 9.4.9: Список изменений'
  description: 'Список изменений Drupal 9.4.9.'
authors:
  - Niklan
category:
  area: 'Drupal 9.4.x'
  title: Drupal 9.4.9
  order: 9
---

<Aside type="warning">

Данная версия находится в разработке и не предназначена для использования.

</Aside>

## CKEditor 5

- [#3276469](https://www.drupal.org/node/3276469) Исправлена рамка для элемента `MediaImageTextAlternativeUi`.
- [#3314511](https://www.drupal.org/node/3314511) Исправлена неполадка в миграции CKEditor 4 → 5 из-за которой могли проваливаться тесты.
- [#3315319](https://www.drupal.org/node/3315319) Исправлены неполадки в тестах приводящие к их провалам.
- [#3314478](https://www.drupal.org/node/3314478) Правила для глобальных аттрибутов теперь упрощаются в `HTMLRestrictions`.
- [#3313946](https://www.drupal.org/node/3313946) Редактор обновлён до версии 35.2.1.
- [#3313473](https://www.drupal.org/node/3313473) Плагины CKEditor 5 теперь поддерживают объявление через деривативы.

## Claro

- [#3311776](https://www.drupal.org/node/3311776) Проблемы с зависимостями теперь выделены цветом.
- [#3308733](https://www.drupal.org/node/3308733) Иконка анимации загрузки теперь корректно выровнена по вертикали.

## Database System

- [#3312641](https://www.drupal.org/node/3312641) Возвращён функционал создания временных таблиц `Connection::queryTemporary()`.

## Entity System

- [#3145501](https://www.drupal.org/node/3145501) Исправлена неполадка, из-за которой вызов `::processMultivalueBaseFieldHandler()` мог приводить к ошибкам обновления.

## JSON:API

- [#3280302](https://www.drupal.org/node/3280302) В `JsonApiDocumentTopLevelNormalizerTest` исправлен вызов с лишним аргументом.

## Layout Builder

- [#3315490](https://www.drupal.org/node/3315490) Внесены улучшения в `InlineBlockPrivateFilesTest`, исправляющие случайные фейлы теста.
- [#3316224](https://www.drupal.org/node/3316224) Исправлены случайные провалы теста `JSWebAssertTest`.

## Locale

- [#3074765](https://www.drupal.org/node/3074765) Улучшена производительность импорта переводов для конкретного проекта.

## Media

- [#3251647](https://www.drupal.org/node/3251647) Исправлена неполадка из-за которой могла некорректно отображаться форма ассоциаций значений при создании или редактировании медиа типа.

## Media Library

- [#3315753](https://www.drupal.org/node/3315753) Внесены улучшения в `WidgetUploadTest`.

## Migrate Drupal

- [#3314134](https://www.drupal.org/node/3314134) В плагин миграции поля `taxonomy_term_reference` добавлена поддержка `i18n_taxonomy_term_reference_plain`.

## Quickedit

- [#3317515](https://www.drupal.org/node/3317515) Исправлены случайные провалы теста `CKEditor5IntegrationTest::testArticleNode()`.

## Views

- [#3284983](https://www.drupal.org/node/3284983) Исправлена неполадка из-за которой `FilterPluginBase::groupForm()` мог вызывать запрос перевода на уже переведённую строку.

## Тестирование

- [#3314710](https://www.drupal.org/node/3314710) В `DrupalSelenium2Driver` добавлен собственная реализация `::dragTo()`.
- [#3315227](https://www.drupal.org/node/3315227) Улучшена реализация ожиданий в `JSWebAssert` и `DocumentElement`.
- [#3315362](https://www.drupal.org/node/3315362) Удалён дублирующий тест `JSWebWithWebDriverAssertTest`.
- [#3316816](https://www.drupal.org/node/3316816) `DrupalSelenuim2Driver::setValue()` теперь вызывает события обновления инпута и формы.

## Прочие изменения

- [#3259109](https://www.drupal.org/node/3259109) Исправлены ошибки PHPStan «Cannot unset offset».
- [#3045612](https://www.drupal.org/node/3045612) (отменено) В DrupalCI уменьшено количество одновременно выполняемых JavaScript тестов с 15 до 10 из-за возможных проблем с DNS на AWS.
- [#3309047](https://www.drupal.org/node/3309047) Исправлены ошибки PHPStan «should return {type} but return statement is missing».
- [#3316224](https://www.drupal.org/node/3316224) Исправлены случайные провалы тестов `JSWebAssertTest`.
- [#3314469](https://www.drupal.org/node/3314469) `ContextDefinition::isSatisfiedBy()` теперь учитывает множественное поле или нет.
- [#3251817](https://www.drupal.org/node/3251817) Для тестов с более чем одной группой теперь корректно собирается информация о всех необходимых тестах для запуска в `run-tests.sh`.