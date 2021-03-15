---
title: 'Drupal 9.0.10'
slug: 9/releases/9.0.10
core: 9
metatags:
  title: 'Drupal 9.0.10: Список изменений'
  description: 'Список изменений Drupal 9.0.10.'
---

**Дата релиза**: 4 декабря 2020

> [!NOTE]
> Данный релиз также содержит изменения внесенные в [Drupal 8.9.8](../../../../8/releases/8.9.x/8.9.8/index.md) и [Drupal 8.9.11](../../../../8/releases/8.9.x/8.9.11/index.md).

## Install

- [#3176652](https://www.drupal.org/project/drupal/issues/3176652) В документации к `install_drupal()` исправлена отсылка на удалённую функцию.

## System

- [#3173891](https://www.drupal.org/project/drupal/issues/3173891) Удалена неиспользуемая переменная `$assert_session` в `UpdateScriptTest`.

## Layout Builder

- [#3126746](https://www.drupal.org/project/drupal/issues/3126746) `LayoutBuilderHtmlEntityFormController` теперь расширяет `FormController`.

## Migration System

- [#3160015](https://www.drupal.org/project/drupal/issues/3160015) `str_replace()` больше не вызывается если путь состоит из одних слешей.

## Quickedit

- [#3037436](https://www.drupal.org/project/drupal/issues/3037436) Внесены множественные улучшения в тест `QuickEditImageTest`.

## Прочие изменения

- [#3069026](https://www.drupal.org/project/drupal/issues/3069026) Удалён вызов `::addAutowiringType()` из `YamlFileLoader`.
- [#3177557](https://www.drupal.org/project/drupal/issues/3177557) Внесены улучшения в `\Drupal\error_test\Controller\ErrorTestController::generateWarnings()` для избежания ошибки в PHP 8.

## Ссылки

- [Drupal 9.0.10](https://www.drupal.org/project/drupal/releases/9.0.10) (англ.), drupal.org, 4 декабря 2020