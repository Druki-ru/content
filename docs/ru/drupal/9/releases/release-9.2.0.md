---
id: release-9.2.0
title: 'Drupal 9.2.0'
path: /9/releases/9.2.0
core: 9
metatags:
  title: 'Drupal 9.2.0: Список изменений'
  description: 'Список изменений Drupal 9.2.0.'
---

**Дата релиза**: 2 июня 2021\
**Прекращение исправления ошибок**: 01 декабря 2021\
**Прекращение поддержки безопасности**: 2022

> [!WARNING]
> Drupal 9.2.0 находится в разработке.

## AJAX

- [#3179939](https://www.drupal.org/project/drupal/issues/3179939) Удалён неиспользуемый `AjaxTestBase`.

## Book

- [#2575827](https://www.drupal.org/project/drupal/issues/2575827) `BookNavigationBlock` и `BookNavigationCacheContext` теперь получают информацию из `route_match` [сервиса](../services/services.md), вместо получения из аттрибутов запроса.

## Locale

- [#3179258](https://www.drupal.org/project/drupal/issues/3179258) Теперь, при поиски строки для перевода в интерфейсе, удаляются начальные и конечные пробелы в поисковой фразе.
