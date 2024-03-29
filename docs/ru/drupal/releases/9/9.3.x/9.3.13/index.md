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
  title: Drupal 9.3.13
  order: 13
---

**Дата релиза:** 11 мая 2022

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
- [#3261599](https://www.drupal.org/node/3261599) Добавлена поддержка `<ol start>` и `<ol reversed>`.
- [#3276974](https://www.drupal.org/node/3276974) (отменено) Исправлена неполадка, из-за которой не работали режимы отображения Media сущностей при отключенном выравнивании.
- [#3228691](https://www.drupal.org/node/3228691) Расширена валидация аттрибутов на предмет XSS.

## Claro

- [#3269341](https://www.drupal.org/node/3269341) Улучшено отображение элемента `<details>` в режиме `forced-colors`.
- [#3130305](https://www.drupal.org/node/3130305) Улучшено отображение фоновых изображений в режиме `forced-colors`.
- [#3273056](https://www.drupal.org/node/3273056) Улучшено отображение «skip link» в режиме `forced-colors`.
- [#3277274](https://www.drupal.org/node/3277274) Использование несуществующей переменной `--color-whitesmoke` заменено на `--color-gray-050`.

## Contextual Links

- [#3270709](https://www.drupal.org/node/3270709) Упоминание `contextual_pre_render_placeholder()` заменено на `\Drupal\contextual\Element\ContextualLinksPlaceholder`.

## Database API

- [#3269091](https://www.drupal.org/node/3269091) Улучшена документация для метода `Schema::findTables()`.

## JavaScript

- [#3266912](https://www.drupal.org/node/3266912) Улучшены проверки для Yarn зависимостей, не позволяющие обновлять зависимости выше патч-релизов на продакшен среде.

## Queue

- [#3230541](https://www.drupal.org/node/3230541) Исправлена неполадка, из-за которой резерв элемента очереди при использовании `@QueueWorker` производился всего на 1 секунду.

## Render API

- [#2717921](https://www.drupal.org/node/2717921) Добавлена документация для свойства `#has_garbage_value`.

## Umami

- [#3277309](https://www.drupal.org/node/3277309) Обновлены ссылки на документацию в `README.txt`.

## Update

- [#2995367](https://www.drupal.org/node/2995367) Обновлены название фикстур в `release-history`.

## Прочие изменения

- [#3272035](https://www.drupal.org/node/3272035) В CSPell словарь добавлены слова «linktext» и «canvastext».
- [#3269085](https://www.drupal.org/node/3269085) Исправлена неполадка в `EntityAutocompleteController` которая могла приводить к провалу `EntityAutocompleteTest`.
- [#3277743](https://www.drupal.org/node/3277743) Обновлены имена мейнтенеров в `MAINTAINERS.txt` файле.

## Ссылки

- [Drupal 9.3.13](https://www.drupal.org/project/drupal/releases/9.3.13) (англ.), drupal.org, 11 мая 2022
