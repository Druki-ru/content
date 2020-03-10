---
id: release-8.8.4
title: 'Drupal 8.8.4'
path: /8/releases/8.8.4
core: 8
metatags:
  title: 'Drupal 8.8.4: Список изменений'
  description: 'Минорный релиз Drupal 8.8.4, список изменений.'
---

**Дата релиза**: В разработке

> [!WARNING]
> Drupal 8.8.4 находится в разработке. Актуальной версией является [Drupal 8.8.3](release-8.8.3.md).

## Composer

- [#3115624](https://www.drupal.org/node/3115624) [drupal/core-scaffold](../../composer/drupal-core-composer-scaffold.md) теперь генерирует `.gitingore` только с файлами относительно корня, а не для всего проекта.

## Menu Link Content

- [#3052318](https://www.drupal.org/node/3052318) Добавлено объяснение с инструкцией если обнаружено нарушение схемы при обновлении до [Drupal 8.7.0](release-8.7.0.md).

## Тестирование

- [#3118454](https://www.drupal.org/node/3118454) Улучшен тест `SelectTest` для поддержки PostgreSQL 10.
- [#3003401](https://www.drupal.org/node/3003401) Исправлена ошибка в тесте `UpdatePathTestBase`, где метод `::setDatabaseDumpFiles` вызывался дважды.

## Прочие изменения

- [#3118186](https://www.drupal.org/node/3118186) Сообщение об устаревшем методе `Crypt::randomBytes` отформатировано в соответствии со стандартом.
- [#3117188](https://www.drupal.org/node/3117188) Для `@todo` `ProjectSecurityData` исправлена ссылка на актуальный ишью.
- [#3088081](https://www.drupal.org/node/3088081) Теперь при парсинге `core_version_requirement` будет вызываться исключение `InfoParserException` с путём до проблемного файла.
