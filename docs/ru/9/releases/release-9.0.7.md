---
id: release-9.0.7
title: 'Drupal 9.0.7'
path: /8/releases/9.0.7
core: 9
metatags:
  title: 'Drupal 9.0.7: Список изменений'
  description: 'Список изменений Drupal 9.0.7.'
---

**Дата релиза**: ожидается 7 октября 2020

> [!WARNING]
> Drupal 9.0.7 находится в разработке.

> [!NOTE]
> Данный релиз также содержит изменения внесенные в [Drupal 8.9.7](../../8/releases/release-8.9.7.md).

## Node System

- [#3170246](https://www.drupal.org/project/drupal/issues/3170246) `NodeLoadMultipleTest` теперь `Kernel` тест, а `Functional`.

## Прочие изменения

- [#3156885](https://www.drupal.org/project/drupal/issues/3156885) `\Drupal\error_test\Controller\ErrorTestController::generateWarnings` теперь выбрасывает `E_NOTICE` ошибки для соответствия PHP 8.