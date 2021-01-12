---
id: release-9.1.3
title: 'Drupal 9.1.3'
path: /9/releases/9.1.3
core: 9
metatags:
  title: 'Drupal 9.1.3: Список изменений'
  description: 'Список изменений Drupal 9.1.3.'
---

**Дата релиза**: 3 февраля 2021

> [!WARNING]
> Данный релиз находится в разработке, актуальная версия [Drupal 9.1.2](release-9.1.2.md).

## Contact

- [#2960353](https://www.drupal.org/project/drupal/issues/2960353) Кнопка "Предпросмотр" теперь учитывает настройки "Управления отображением формы".

## Layout Builder

- [#3167901](https://www.drupal.org/project/drupal/issues/3167901) Если для секции не задана метка, будет указана дельта этой секции.

## Migration System

- [#3151732](https://www.drupal.org/project/drupal/issues/3151732) Плагины источников данных расширяющие `DrupalSqlBase` теперь могут указать в аннотации `minimum_version` значение с минимальной версией схемы БД.
- [#2565931](https://www.drupal.org/project/drupal/issues/2565931) Длинные названия для бандлов комментариев из Drupal 7 теперь корректно обрабатываются.
- [#3077322](https://www.drupal.org/project/drupal/issues/3077322) Добавлены тесты на `NULL` значения источников.

## Olivero

- [#3180086](https://www.drupal.org/project/drupal/issues/3180086) Меню теперь отображает только один выпадающий список. Если до этого был открыт другой, он будет скрыт.

## SQLite драйвер БД

- [#3159073](https://www.drupal.org/project/drupal/issues/3159073) Драйвер теперь использует нативные возможности БД для `UPSERT` операций.

## Views

- [#2784739](https://www.drupal.org/project/drupal/issues/2784739) Views теперь использует корректные операторы для PostgreSQL. Для нечувствительных к регистру фильтрация будет использован `ILIKE` вместо `LIKE` и `NOT ILIKE` вместо `NOT LIKE`.
- [#3012704](https://www.drupal.org/project/drupal/issues/3012704) Исправлена неполадка из-за которой настройка "Элементов на страницу" не сохранялась корректно при создании отображения типа "Блок".

## Workspaces

- [#3181508](https://www.drupal.org/project/drupal/issues/3181508) Исправлена неполадка из-за которой было невозможно удалить "Workspace" содержащий содержимое.
- [#3191821](https://www.drupal.org/project/drupal/issues/3191821) Улучшена документация для `WorkspaceAssociationInterface::deleteAssociations()`.

## Umami Demo

- [#3001660](https://www.drupal.org/project/drupal/issues/3001660) Улучшены стили для адаптивного отображения.

## Тестирование

- [#3159788](https://www.drupal.org/project/drupal/issues/3159788) (откачено для 9.1.x) Удалены оставшиеся вызовы `AssertLegacyTrait::assertText()` и `AssertLegacyTrait::assertNoText()` с передачей `message` параметра.
