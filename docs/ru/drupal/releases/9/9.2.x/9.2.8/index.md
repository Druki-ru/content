---
title: 'Drupal 9.2.8'
slug: drupal/releases/9.2.8
core: 9
metatags:
  title: 'Drupal 9.2.8: Список изменений'
  description: 'Список изменений Drupal 9.2.8.'
---

> [!WARNING]
> Данная версия находится в разработке и не предназначена для использования.

## Asset Library System

* [#2936067](https://www.drupal.org/node/2936067) Внесены улучшения в CSS агрегатор, который теперь корректно обрабатывает больше различных вариантов использования `@import`.

## Claro

* [#3219340](https://www.drupal.org/node/3219340) Исправлена неполадка с отображением вертикальных вкладок которые имеют `#parents`.

## File System

* [#3192365](https://www.drupal.org/node/3192365) Исправлена неполадка в `TestRunnerKernel`, которая приводила к состоянию гонки при попытке создать `public://simpletest`.

## JSON:API

* [#3228000](https://www.drupal.org/node/3228000) Теперь, отправка `DELETE` на `/jsonapi/user/{UUID}` учитывает настройки того, что необходимо делать с аккаунтом при его удалении.

## Olivero

* [#3223281](https://www.drupal.org/node/3223281) Исправлена неполадка, из-за которой иконка поиска была невидимой в режиме принудительного выбора цветов ([forced-colors](https://developer.mozilla.org/en-US/docs/Web/CSS/@media/forced-colors)) в MS Edge.
* [#3223271](https://www.drupal.org/node/3223271) Исправлена контрастность иконок выпадающих меню для режима повышенной контрастности в Windows.
* [#3242456](https://www.drupal.org/node/3242456) Исправлена контрастность для `fieldset` элементов.
* [#3242469](https://www.drupal.org/node/3242469) Исправлена контрастность для неактивных вертикальных меток элементов форм.
* [#3212670](https://www.drupal.org/node/3212670) Исправлена неполадка с `z-index` строки поиска.