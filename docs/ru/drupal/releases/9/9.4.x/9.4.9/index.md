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

## Claro

- [#3311776](https://www.drupal.org/node/3311776) Проблемы с зависимостями теперь выделены цветом.
- [#3308733](https://www.drupal.org/node/3308733) Иконка анимации загрузки теперь корректно выровнена по вертикали.

## JSON:API

- [#3280302](https://www.drupal.org/node/3280302) В `JsonApiDocumentTopLevelNormalizerTest` исправлен вызов с лишним аргументом.

## Media

- [#3251647](https://www.drupal.org/node/3251647) Исправлена неполадка из-за которой могла некорректно отображаться форма ассоциаций значенйи при создании или редактировании медиа типа.

## Migrate Drupal

- [#3314134](https://www.drupal.org/node/3314134) В плагин миграции поля `taxonomy_term_reference` добавлена поддержка `i18n_taxonomy_term_reference_plain`.

## Views

- [#3284983](https://www.drupal.org/node/3284983) Исправлена неполадка из-за которой `FilterPluginBase::groupForm()` мог вызывать запрос перевода на уже переведённую строку.

## Тестирование

- [#3314710](https://www.drupal.org/node/3314710) В `DrupalSelenium2Driver` добавлен собственная реализация `::dragTo()`.
- [#3315227](https://www.drupal.org/node/3315227) Улучшена реализация ожиданий в `JSWebAssert` и `DocumentElement`.
- [#3315362](https://www.drupal.org/node/3315362) Удалён дублирующий тест `JSWebWithWebDriverAssertTest`.

## Прочие изменения

- [#3259109](https://www.drupal.org/node/3259109) Исправлены ошибки PHPStan «Cannot unset offset».