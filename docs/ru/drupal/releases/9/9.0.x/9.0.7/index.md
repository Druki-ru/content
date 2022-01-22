---
title: 'Drupal 9.0.7'
slug: wiki/drupal/releases/9.0.7
core: 9
metatags:
  title: 'Drupal 9.0.7: Список изменений'
  description: 'Список изменений Drupal 9.0.7.'
authors:
  - Niklan
  - arraksis
---

**Дата релиза**: 8 октября 2020

> [!NOTE]
> Данный релиз также содержит изменения внесенные в [Drupal 8.9.7](../../../8/8.9.x/8.9.7/index.md).

## Configuration system

- [#3152320](https://www.drupal.org/project/drupal/issues/3152320) Добавлены недостающие аргументы DI для `ExtensionInstallStorage::createCollection()`.

## Migration System

- [#3098282](https://www.drupal.org/project/drupal/issues/3098282) Улучшено поведение если в запросе ID полей превышают VARCHAR(255).

## Node System

- [#3170246](https://www.drupal.org/project/drupal/issues/3170246) `NodeLoadMultipleTest` теперь `Kernel` тест, а не `Functional`.

## Прочие изменения

- [#3156885](https://www.drupal.org/project/drupal/issues/3156885) `\Drupal\error_test\Controller\ErrorTestController::generateWarnings` теперь выбрасывает `E_NOTICE` ошибки для соответствия PHP 8.
- [#3172425](https://www.drupal.org/project/drupal/issues/3172425) Использование `Symfony\Component\Lock\Factory` заменено на `Symfony\Component\Lock\LockFactory`.
- [#3170020](https://www.drupal.org/project/drupa/issues/3170020) Из `MAINTAINERS.txt` удалён раздел «API-first Initiative», так как она выполнена.

## Ссылки

- [Drupal 9.0.7](https://www.drupal.org/project/drupal/releases/9.0.7) (англ.), drupal.org, 8 октября 2020
