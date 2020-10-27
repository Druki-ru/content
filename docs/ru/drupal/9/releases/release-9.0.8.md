---
id: release-9.0.8
title: 'Drupal 9.0.8'
path: /9/releases/9.0.8
core: 9
metatags:
  title: 'Drupal 9.0.8: Список изменений'
  description: 'Список изменений Drupal 9.0.8.'
---

**Дата релиза**: ожидается 4 ноября 2020

> [!WARNING]
> Данная версия находится в разработке, актуальная [Drupal 9.0.7](release-9.0.7.md).

> [!NOTE]
> Данный релиз также содержит изменения внесенные в [Drupal 8.9.8](../../8/releases/release-8.9.8.md).

## Contact

- [#2223967](https://www.drupal.org/project/drupal/issues/2223967) Исправлено двойное декодирование сообщения.

## Install

- [#3176652](https://www.drupal.org/project/drupal/issues/3176652) В документации к `install_drupal()` исправлена отсылка на удалённую функцию.

## System

- [#3173891](https://www.drupal.org/project/drupal/issues/3173891) Удалена неиспользуемая переменная `$assert_session` в `UpdateScriptTest`.

## Quickedit

- [#3037436](https://www.drupal.org/project/drupal/issues/3037436) Внесены множественные улучшения в тест `QuickEditImageTest`.

## Прочие изменения

- [#3069026](https://www.drupal.org/project/drupal/issues/3069026) Удалён вызов `::addAutowiringType()` из `YamlFileLoader`.
- [#3177557](https://www.drupal.org/project/drupal/issues/3177557) Внесены улучшения в `\Drupal\error_test\Controller\ErrorTestController::generateWarnings()` для избежания ошибки в PHP 8.