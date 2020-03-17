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

## Entity API

- [#2605904](https://www.drupal.org/node/2605904) `EntityManager::clearDisplayModeInfo` теперь возвращает сервис `entity_type.manager`.

## Menu Link Content

- [#3052318](https://www.drupal.org/node/3052318) Добавлено объяснение с инструкцией если обнаружено нарушение схемы при обновлении до [Drupal 8.7.0](release-8.7.0.md).

## Тестирование

- [#3118454](https://www.drupal.org/node/3118454) Улучшен тест `SelectTest` для поддержки PostgreSQL 10.
- [#3003401](https://www.drupal.org/node/3003401) Исправлена ошибка в тесте `UpdatePathTestBase`, где метод `::setDatabaseDumpFiles` вызывался дважды.
- [#3094151](https://www.drupal.org/node/3094151) `ExpectDeprecationTrait` изменён для поддержки PHPUnit 8.

## Прочие изменения

- [#3118186](https://www.drupal.org/node/3118186) Сообщение об устаревшем методе `Crypt::randomBytes` отформатировано в соответствии со стандартом.
- [#3117188](https://www.drupal.org/node/3117188) Для `@todo` `ProjectSecurityData` исправлена ссылка на актуальный ишью.
- [#3088081](https://www.drupal.org/node/3088081) Теперь при парсинге `core_version_requirement` будет вызываться исключение `InfoParserException` с путём до проблемного файла.
- [#3118439](https://www.drupal.org/node/3118439) Исправлен некорректный комментарий для `PrivateKey::__construct`.
- [#3056543](https://www.drupal.org/node/3056543) Ссылки меню и термины таксономии что не имеют переводов по умолчанию, автоматически исправляются прежде чем сущность становится ревизионной.
- [#2865416](https://www.drupal.org/node/2865416) Ссылки в README.txt что ведут на документацию по Drupal 7 были исправлены на корректные.
- [#2917600](https://www.drupal.org/node/2917600) Функция `update_fix_compatibility()` помечена устаревшей и обновления больше не будут пытаться удалить модули.
- [#3103529](https://www.drupal.org/node/3103529) Теперь таблицы кэша не создаются в момент инициализации установки.
- [#3118581](https://www.drupal.org/node/3118581) Улучшена документация о длине результата `Crypt::randomBytesBase64`.
- [#3119445](https://www.drupal.org/node/3119445) Исправлена некорректная документация `BanIpManager::__construct`.
- [#3105925](https://www.drupal.org/node/3105925) Документация `FieldDefinition::setDisplayOptions` исправлена в соответствии с `FieldDefinitionInterface::getDisplayOptions`.
- [#3119847](https://www.drupal.org/node/3119847) Исправлены опечатки в `UpdaterTest` и `InfoParserUnitTest`.
