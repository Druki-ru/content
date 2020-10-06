---
id: release-9.0.7
title: 'Drupal 9.0.7'
path: /9/releases/9.0.7
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

## Configuration system

- [#3152320](https://www.drupal.org/project/drupal/issues/3152320) Добавлены недостающие аргументы DI для `ExtensionInstallStorage::createCollection()`.

## Migration System

- [#3098282](https://www.drupal.org/project/drupal/issues/3098282) Улучшено поведение если в запросе ID полей превышают VARCHAR(255).
- [#2904546](https://www.drupal.org/project/drupal/issues/2904546) Редирект для `admin/reports/upgrade` теперь передает аргументы корректно, вместо использования `$_SESSION`.

## Node System

- [#3170246](https://www.drupal.org/project/drupal/issues/3170246) `NodeLoadMultipleTest` теперь `Kernel` тест, а `Functional`.

## Прочие изменения

- [#3156885](https://www.drupal.org/project/drupal/issues/3156885) `\Drupal\error_test\Controller\ErrorTestController::generateWarnings` теперь выбрасывает `E_NOTICE` ошибки для соответствия PHP 8.
- [#3172425](https://www.drupal.org/project/drupal/issues/3172425) Использование `Symfony\Component\Lock\Factory` заменено на `Symfony\Component\Lock\LockFactory`.
- [#3170020](https://www.drupal.org/project/drupal/issues/3170020) Из `MAINTAINERS.txt` удалён раздел «API-first Initiative», так как она выполнена.