---
id: release-9.1.5
title: 'Drupal 9.1.5'
path: /9/releases/9.1.5
core: 9
metatags:
  title: 'Drupal 9.1.5: Список изменений'
  description: 'Список изменений Drupal 9.1.5.'
---

**Дата релиза**: 3 марта 2021

> [!WARNING]
> Данная версия находится в разработке, актуальная версия [Drupal 9.1.4](release-9.1.4.md).

## Configuration System

- [#3196050](https://www.drupal.org/project/drupal/issues/3196050) Исправлена документация для `StorageConfigBase::validateValue()`.

## Editor

- [#2857444](https://www.drupal.org/project/drupal/issues/2857444) Улучшено отслеживание файлов в полях отличных от `text`, `text_long` и `text_with_summary`. Теперь данные файлы отслеживаются для всех полей что расширяют `TextItemBase`.

## Entity System

- [#2693485](https://www.drupal.org/project/drupal/issues/2693485) Пункты добавления материалов (`/node/add`) теперь сортируются по метке, а не по машинному имени.

## Migration System

- [#2558857](https://www.drupal.org/project/drupal/issues/2558857) Оптимизирована очистка памяти после загрузки сущностей.
- [#3165944](https://www.drupal.org/project/drupal/issues/3165944) Миграция `d7_shortcut` больше не указывает в качестве зависимости миграцию `d7_menu_links`.
- [#3189476](https://www.drupal.org/project/drupal/issues/3189476) Миграция `node_translation_menu_links` теперь имеет опциональные зависимости `d6_menu` и `d7_menu`.
- [#3196177](https://www.drupal.org/project/drupal/issues/3196177) Добавлена документация для плагинов источников `VariableMultiRow`, `Variable`, `d6/VariableTranslation` и `d7/VariableTranslation`.

## Тестирование

- [#2571475](https://www.drupal.org/project/drupal/issues/2571475) `KernelTestBase` теперь могу производить внешние HTTP запросы.

## Прочие изменения

- [#3195951](https://www.drupal.org/project/drupal/issues/3195951) Из `MAINTAINERS.txt` удалён пустой раздел "Provisional membership".
- [#3195277](https://www.drupal.org/project/drupal/issues/3195277) Из `MAINTAINERS.txt` удалены упоминания Drupal 8.
- [#3196391](https://www.drupal.org/project/drupal/issues/3196391) Упоминание "Drupal Core" в `MAINTAINERS.txt` приведено к единому стилю.
- [#3196433](https://www.drupal.org/project/drupal/issues/3196433) Исправлена ссылка ведущая на документацию форматов времени PHP.net