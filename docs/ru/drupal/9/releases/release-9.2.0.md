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

## Объект исключения теперь передаётся в контексте в логер

- [#2949419](https://www.drupal.org/project/drupal/issues/2949419)

При логировании исключения теперь можно получить объект самого исключения из контекста `$context['exception']`.

Данное изменение соответствует [PSR-3](https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-3-logger-interface.md#13-context) а также позволит проще обрабатывать ошибки сторонними решениями, например [Raven](https://www.drupal.org/project/raven).

## Передача StatementInterface объекта в Connection::query() помечена устаревшим

- [#3154439](https://www.drupal.org/node/3154439)

Передача объекта реализующего `Drupal\Core\Database\StatementInterface` в `Drupal\Core\Database\Connection::query()` помечена устаревшей и будет удалена в [Drupal 10](../../10/drupal-10.md). Сейчас метод позволяет передавать только строковые значения для параметра `$query`.

Было:

```php
  $db = \Drupal\Core\Database\Database::getConnection();
  $stmt = $db->prepareStatement('SELECT * FROM {test}', []);
  $result = $db->query($stmt);
```

Стало:

```php
  $db = \Drupal\Core\Database\Database::getConnection();
  $result = $db->query('SELECT * FROM {test}');
```

## AJAX

- [#3179939](https://www.drupal.org/project/drupal/issues/3179939) Удалён неиспользуемый `AjaxTestBase`.

## Book

- [#2575827](https://www.drupal.org/project/drupal/issues/2575827) `BookNavigationBlock` и `BookNavigationCacheContext` теперь получают информацию из `route_match` [сервиса](../services/services.md), вместо получения из аттрибутов запроса.

## Form API

- [#2702233](https://www.drupal.org/project/drupal/issues/2702233) Добавлены JavaScript тесты для Form API `#states` состояний: `required`, `visible`, `invisible`, `expanded`, `checked`, `unchecked`.

## Layout Builder

- [#3180674](https://www.drupal.org/project/drupal/issues/3180674) Удалён неиспользуемый модуль `layout_builder_overrides_test`.

## System

- [#2409413](https://www.drupal.org/project/drupal/issues/2409413) Удалены неиспользуемые RSS настройки и описания.

## Taxonomy

- [#3170185](https://www.drupal.org/project/drupal/issues/3170185) `Drupal\taxonomy\Form\OverviewTerms` теперь использует `pager.manager` для получения текущей страницы вместо `Request`.

## Тестирование

- [#3176655](https://www.drupal.org/project/drupal/issues/3176655) `GoutteDriver` помечен устаревшим, вместо него ядро теперь использует `BrowserKitDriver`.

## Прочие изменения

- [#3173636](https://www.drupal.org/project/drupal/issues/3173636) В `PagerManager` добавлена реализация `PagerManagerInterface::findPage()`.