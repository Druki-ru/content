---
title: 'Drupal 9.2.5'
slug: drupal/releases/9.2.5
core: 9
metatags:
  title: 'Drupal 9.2.5: Список изменений'
  description: 'Список изменений Drupal 9.2.5.'
---

> [!WARNING]
> Данная версия находится в разработке и не предназначена для использования.

## Media System

* [#3063343](https://www.drupal.org/node/3063343) `MediaLibraryState` теперь реализует `CacheableDependencyInterface`.
* [#3222616](https://www.drupal.org/node/3222616) Улучшено регулярное выражение для YouTube, которое теперь позволяет добавлять плейлисты в качестве удаленных видео.

## Syslog

* [#3224414](https://www.drupal.org/node/3224414) Исправлена неполадка из-за которой модуль Syslog использовал свои настройки до того как они были записаны.

## Typed Data System

* [#2047119](https://www.drupal.org/node/2047119) Акутализирована документация аннотации `DataType`.

## Тестирование

* [#3191935](https://www.drupal.org/node/3191935) Использование устаревшего `AssertLegacyTrait::assertNoText()` заменено современными методами.
* [#3170396](https://www.drupal.org/node/3170396) Вызовы `t()` заменены на `::pageTextContains()` в вызовах `::assertRaw()` и `::assertNoRaw()`.
* [#3227060](https://www.drupal.org/node/3227060) Вызов устаревшего `AssertLegacyTrait::assertNoRaw()` заменены на современные аналоги.

## Прочие изменения

* [#2989893](https://www.drupal.org/node/2989893) Из примера `substr` плагина-обработчика удалён лишний ключ `key`.