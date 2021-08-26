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

## Тестирование

* [#3191935](https://www.drupal.org/node/3191935) Использование устаревшего `AssertLegacyTrait::assertNoText()` заменено современными методами.