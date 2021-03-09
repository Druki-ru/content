---
id: release-8.9.14
title: 'Drupal 8.9.14'
path: /8/releases/8.9.14
core: 8
metatags:
  title: 'Drupal 8.9.14: Список изменений'
  description: 'Список изменений Drupal 8.9.14.'
---

**Дата релиза**: 7 апреля 2021

## Cron System

- [#3201470](https://www.drupal.org/project/drupal/issues/3201470) Реализации `hook_cron()` с использованием `EntityQuery` теперь явно указывают что права доступа проверять не нужно.

## Editor

- [#2857444](https://www.drupal.org/project/drupal/issues/2857444) Улучшено отслеживание файлов в полях отличных от `text`, `text_long` и `text_with_summary`. Теперь данные файлы отслеживаются для всех полей что расширяют `TextItemBase`.

## Media System

- [#3192260](https://www.drupal.org/project/drupal/issues/3192260) Исправлена настройка DrupalCI для `CKEditorIntegrationTest` который мог случайно проваливаться.

## Migration System

[#3184650](https://www.drupal.org/project/drupal/issues/3184650) В миграцию ContentEntity добавлена возможность указывать ключ источника в котором хранится ID ревизии. Для более подробного описания изменения смотрите [список изменений Drupal 9.1.5](../../9/releases/release-9.1.5.md).

## Serialization

- [#3054510](https://www.drupal.org/project/drupal/issues/3054510) Исправлена документация для `NormalizerBase::supportsDenormalization()`.

## Views

- [#2969107](https://www.drupal.org/project/drupal/issues/2969107) Аргументы для даты больше не возвращают HTTP 500 при некорректном формате.

## Прочие изменения

- [#3192231](https://www.drupal.org/project/drupal/issues/3192231) Исправлена работа теста `UnroutedUrlTest` на dev версиях PHP.
- [#3199205](https://www.drupal.org/project/drupal/issues/3199205) `Archive_Tar` обновлён до 1.4.13.