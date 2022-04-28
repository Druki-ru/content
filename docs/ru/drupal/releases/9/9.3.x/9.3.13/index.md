---
title: 'Drupal 9.3.13'
slug: wiki/drupal/releases/9.3.13
core: 9
metatags:
  title: 'Drupal 9.3.13: Список изменений'
  description: 'Список изменений Drupal 9.3.13.'
authors:
  - Niklan
category:
  area: 'Drupal 9.3.x'
  title: Drupal 9.3.13 (в разработке)
  order: 13
---

<Aside type="warning">

Текущий релиз находится в разработке и не предназначен для использования на рабочих сайтах.

</Aside>

## CKEditor 5

- [#3274278](https://www.drupal.org/node/3274278) Добавлена поддержка миграции `codetag` плагина, предоставляемого одноимённым сторонним модулем, из CKEditor 4.
- [#3273325](https://www.drupal.org/node/3273325) Улучшено сообщение с информацией о совместимости сторонних плагинов.
- [#3230230](https://www.drupal.org/node/3230230) Добавлена поддержка подписей для таблиц.
- [#3261943](https://www.drupal.org/node/3261943) Улучшено поведение при нажатии «Применить изменения для разрешённых тегов» с недопустимыми значениями.
- [#3245720](https://www.drupal.org/node/3245720) Добавлена поддержка выбора режима представления для `<drupal-media>`.
- [#3276670](https://www.drupal.org/node/3276670) Исправлена неполадка, из-за которой при наличии определённых режимов отображения редактор не мог инициализироваться.
- [#3276627](https://www.drupal.org/node/3276627) Внесены улучшения в логику `CKEditor5::shouldHaveVisiblePluginSettingsForm()`.
- [#3248425](https://www.drupal.org/node/3248425) Добавлены недостающая документация для плагинов.
- [#3229078](https://www.drupal.org/node/3229078) Добавлены юнит-тесты для всех `@CKEditor5Plugin` плагинов.
- [#3277405](https://www.drupal.org/node/3277405) Обновлена зависимость `@ckeditor/ckeditor5-list` до версии 34.0.1.

## Claro

- [#3269341](https://www.drupal.org/node/3269341) Улучшено отображение элемента `<details>` в режиме `forced-colors`.
- [#3130305](https://www.drupal.org/node/3130305) Улучшено отображение фоновых изображений в режиме `forced-colors`.
- [#3273056](https://www.drupal.org/node/3273056) Улучшено отображение «skip link» в режиме `forced-colors`.

## Database API

- [#3269091](https://www.drupal.org/node/3269091) Улучшена документация для метода `Schema::findTables()`.

## Render API

- [#2717921](https://www.drupal.org/node/2717921) Добавлена документация для свойства `#has_garbage_value`.

## Прочие изменения

- [#3272035](https://www.drupal.org/node/3272035) В CSPell словарь добавлены слова «linktext» и «canvastext».
- [#3269085](https://www.drupal.org/node/3269085) Исправлена неполадка в `EntityAutocompleteController` которая могла приводить к провалу `EntityAutocompleteTest`.