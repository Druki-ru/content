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

## Claro

- [#3311776](https://www.drupal.org/node/3311776) Проблемы с зависимостями теперь выделены цветом.

## JSON:API

- [#3280302](https://www.drupal.org/node/3280302) В `JsonApiDocumentTopLevelNormalizerTest` исправлен вызов с лишним аргументом.

## Media

- [#3251647](https://www.drupal.org/node/3251647) Исправлена неполадка из-за которой могла некорректно отображаться форма ассоциаций значенйи при создании или редактировании медиа типа.

## Migrate Drupal

- [#3314134](https://www.drupal.org/node/3314134) В плагин миграции поля `taxonomy_term_reference` добавлена поддержка `i18n_taxonomy_term_reference_plain`.

## Views

- [#3284983](https://www.drupal.org/node/3284983) Исправлена неполадка из-за которой `FilterPluginBase::groupForm()` мог вызывать запрос перевода на уже переведённую строку.