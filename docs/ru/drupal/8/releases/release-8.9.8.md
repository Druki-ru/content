---
id: release-8.9.8
title: 'Drupal 8.9.8'
path: /8/releases/8.9.8
core: 8
metatags:
  title: 'Drupal 8.9.8: Список изменений'
  description: 'Список изменений Drupal 8.9.8.'
---

**Дата релиза**: ожидается 4 ноября 2020

> [!WARNING]
> Данная версия находится в разработке, актуальная [Drupal 8.9.7](release-8.9.7.md).

## DateTime

- [#3175395](https://www.drupal.org/project/drupal/issues/3175395) Удалено неиспользуемое свойство `#html` из `DateTimeFormatterBase::buildDateWithIsoAttribute`.

## Тестирование

- [#3175112](https://www.drupal.org/project/drupal/issues/3175112) Исправлена ошибка в `hold_test` модуле, который создавал файлы в неположенном месте что приводило к различным случайным ошибкам.