---
id: release-9.1.4
title: 'Drupal 9.1.4'
path: /9/releases/9.1.4
core: 9
metatags:
  title: 'Drupal 9.1.4: Список изменений'
  description: 'Список изменений Drupal 9.1.4.'
---

**Дата релиза**: 4 февраля 2021

## Теперь можно задавать сервисам синонимы

- [#2828099](https://www.drupal.org/project/drupal/issues/2828099)

Исправлена неполадка, которая присутствовала с первых релизов [Drupal 8](../../../../8/index.md) и не позволяла создавать [сервисы](../../../services/index.md) в качестве алиасов для других. Данная ошибка исправлена и теперь вы можете создавать сервисы-синонимы:

```yaml
services:
  service1: '@service2'
```

## Claro

- [#3192763](https://www.drupal.org/project/drupal/issues/3192763) Добавлен список мейнтенеров для темы.

## Comment

- [#3193771](https://www.drupal.org/project/drupal/issues/3193771) Исправлена неполадка приводящая к исключению `PluginNotFoundException`.

## Contact

- [#2960353](https://www.drupal.org/project/drupal/issues/2960353) Кнопка "Предпросмотр" теперь учитывает настройки "Управления отображением формы".

## Entity System

- [#3190285](https://www.drupal.org/project/drupal/issues/3190285) Исправлена неполадка из-за которой `QueryAggregate` не преобразовывал поле являющимся аргументом функции агрегации.

## Form API

- [#2702233](https://www.drupal.org/project/drupal/issues/2702233) Добавлены JavaScript тесты для Form API `#states` состояний: `required`, `visible`, `invisible`, `expanded`, `checked`, `unchecked`.

## Layout Builder

- [#3167901](https://www.drupal.org/project/drupal/issues/3167901) Если для секции не задана метка, будет указана дельта этой секции.

## Media System

- [#3192059](https://www.drupal.org/project/drupal/issues/3192059) Теперь для определения, изменился ли источник, используется основное свойство поля источника, вместо всех значений, которые могут быть динамическими.

## Migration System

- [#3151732](https://www.drupal.org/project/drupal/issues/3151732) [Плагины](../../../plugins/index.md) источников данных расширяющие `DrupalSqlBase` теперь могут указать в аннотации `minimum_version` значение с минимальной версией схемы БД.
- [#2565931](https://www.drupal.org/project/drupal/issues/2565931) Длинные названия для бандлов комментариев из Drupal 7 теперь корректно обрабатываются.
- [#3077322](https://www.drupal.org/project/drupal/issues/3077322) Добавлены тесты на `NULL` значения источников.
- [#3175729](https://www.drupal.org/project/drupal/issues/3175729) Завершена работа над мультиязычными миграциями (i18n). Все известные проблемы и задачи были закрыты.
- [#3191490](https://www.drupal.org/project/drupal/issues/3191490) Улучшена миграция заголовков блоков со значением `<none>`. При миграции данное значение будет заменяться на настройку блока `label_display` равным `0`.
- [#3192888](https://www.drupal.org/project/drupal/issues/3192888) Удалён референс на несуществующий плагин `LoadEntity`.

## Node

- [#3127250](https://www.drupal.org/project/drupal/issues/3127250) Предпросмотр материалов больше не кэшируется Dynamic Page Cache.

## Olivero

- [#3180086](https://www.drupal.org/project/drupal/issues/3180086) Меню теперь отображает только один выпадающий список. Если до этого был открыт другой, он будет скрыт.

## Options

- [#3190231](https://www.drupal.org/project/drupal/issues/3190231) Исправлена ошибка в примере для хука `hook_options_list_alter()`.

## Serialization

- [#3054510](https://www.drupal.org/project/drupal/issues/3054510) Исправлена документация для `NormalizerBase::supportsDenormalization()`.

## SQLite драйвер БД

- [#3159073](https://www.drupal.org/project/drupal/issues/3159073) Драйвер теперь использует нативные возможности БД для `UPSERT` операций.

## Views

- [#2784739](https://www.drupal.org/project/drupal/issues/2784739) Views теперь использует корректные операторы для PostgreSQL. Для нечувствительных к регистру фильтрация будет использован `ILIKE` вместо `LIKE` и `NOT ILIKE` вместо `NOT LIKE`.
- [#3012704](https://www.drupal.org/project/drupal/issues/3012704) Исправлена неполадка из-за которой настройка "Элементов на страницу" не сохранялась корректно при создании отображения типа "Блок".
- [#3000383](https://www.drupal.org/project/drupal/issues/3000383) Улучшены тесты для Views AJAX запросов с двойным слэшем в начале пути.
- [#3167733](https://www.drupal.org/project/drupal/issues/3167733) Улучшена проверка на виджет поля которая могла приводить к нотисам на PHP 7.4.
- [#2223195](https://www.drupal.org/project/drupal/issues/2223195) Поле "Глобальный: Собственный текст" больше не показывает галочку что оно сортируемо.

## Workspaces

- [#3181508](https://www.drupal.org/project/drupal/issues/3181508) Исправлена неполадка из-за которой было невозможно удалить "Workspace" содержащий содержимое.
- [#3191821](https://www.drupal.org/project/drupal/issues/3191821) Улучшена документация для `WorkspaceAssociationInterface::deleteAssociations()`.
- [#3192292](https://www.drupal.org/project/drupal/issues/3192292) Исправлена неполадка из-за которой пользователь с пермишеном `'administer workspaces'` не мог создавать новые воркспейсы.

## Umami Demo

- [#3001660](https://www.drupal.org/project/drupal/issues/3001660) Улучшены стили для адаптивного отображения.

## Тестирование

- [#3159788](https://www.drupal.org/project/drupal/issues/3159788) Удалены оставшиеся вызовы `AssertLegacyTrait::assertText()` и `AssertLegacyTrait::assertNoText()` с передачей `message` параметра.
- [#3192553](https://www.drupal.org/project/drupal/issues/3192553) Вызовы `::assertIdentical(NULL)` заменены на `::assertNull()`.
- [#3192427](https://www.drupal.org/project/drupal/issues/3192427) Вызовы `AssertLegacyTrait::assertNotEqual()` заменены на современные аналоги.
- [#3193600](https://www.drupal.org/project/drupal/issues/3193600) Вызовы `::assertEquals()` с использованием `NULL`, `TRUE` и `FALSE` заменены на более подходящие сравнения.
- [#3192221](https://www.drupal.org/project/drupal/issues/3192221) Изменён порядок передачи аргументов в вызовах `::assertIdentical()` для последующей замены на `::assertSame()`.
- [#2867959](https://www.drupal.org/project/drupal/issues/2867959) Вызовы устаревшего `AssertLegacyTrait::assertIdentical()` заменены на современные аналоги.
- [#3193955](https://www.drupal.org/project/drupal/issues/3193955) Изменён порядок передачи аргументов в вызовах `::assertEqual()` для последующей замены на `::assertEquals()`.

## Прочие изменения

- [#2635440](https://www.drupal.org/project/drupal/issues/2635440) Добавлено дополнительное описание о том, что на самом деле очищает метод `ContentEntityStorageBase::resetCache()`.
- [#2717541](https://www.drupal.org/project/drupal/issues/2717541) Добавлена отсутствующая документация для `hook_block_alter()`.
- [#3186009](https://www.drupal.org/project/drupal/issues/3186009) Исправлена неполадка приводящая к неопределённым переменным.
- [#3170260](https://www.drupal.org/project/ideas/issues/3170260) Добавлена информация об инициативе "Decoupled Menus".
- [#3191468](https://www.drupal.org/project/drupal/issues/3191468) Упразднена инициатива "Admin UI and JavaScript Modernisation" в пользу инициативы "Decoupled Menus".
- [#3194562](https://www.drupal.org/project/drupal/issues/3194562) Добавлены фикстуры с дампами базы данных для [Drupal 9.0.0](../../9.0.x/9.0.0/index.md).
- [#3187241](https://www.drupal.org/project/drupal/issues/3187241) Brian Gilbert (realityloop) добавлен в качестве временного координатора Drupal Core.

## Ссылки

- [Drupal 9.1.4](https://www.drupal.org/project/drupal/releases/9.1.4) (англ.), drupal.org, 4 февраля 2020