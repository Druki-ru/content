---
id: release-9.0.3
title: 'Drupal 9.0.3'
path: /9/releases/9.0.3
core: 9
metatags:
  title: 'Drupal 9.0.3: Список изменений'
  description: 'Список изменений Drupal 9.0.3.'
---

> [!IMPORTANT]
> Данный релиз находится в разработке. Актуальная версия [Drupal 9.0.2](release-9.0.2.md).

> [!NOTE]
> Данный релиз также содержит изменения внесенные в [Drupal 8.9.3](../../8/releases/release-8.9.3.md).

## Composer

- [#3159735](https://www.drupal.org/project/drupal/issues/3159735) Шаблоны проектов [drupal/recommended-project](../../composer/drupal-recommended-project.md) и [drupal/legacy-project](../../composer/drupal-legacy-project.md) теперь упоминают Drupal 9 в описании.

## Database System

- [#3152390](https://www.drupal.org/project/drupal/issues/3152390) В тестах, для прямых запросов поля заключены в квадратный скобки.

## Entity System

- [#3145076](https://www.drupal.org/project/drupal/issues/3145076) Базовые поля типа `map` теперь могут быть корректно удалены.

## Media System

- [#3124302](https://www.drupal.org/project/drupal/issues/3124302) Media Library теперь производит проверку на наличие прав для ревизии которая редактируется.
- [#3102402](https://www.drupal.org/project/drupal/issues/3102402) Виджет Media Library больше не добавляет поле «weight» если можно выбрать всего одно значение.

## Migrate System

- [#3133516](https://www.drupal.org/project/drupal/issues/3133516) Плагин обработки `extract` теперь может принимать `NULL` в качестве `default_value` и обрабатывать его корректно.

## Search

- [#3047719](https://www.drupal.org/project/drupal/issues/3047719) Справка из `hook_help()` конвертирована в Help Topics.

## SQLite

- [#3145412](https://www.drupal.org/project/drupal/issues/3145412) При удалении пустой БД, теперь происходит отключение данной БД, а только затем удаление. Это решает проблему когда файл БД невозможно удалить из-за недостаточных прав доступа так как она используется.

## User

- [#2934904](https://www.drupal.org/project/drupal/issues/2934904) В тесте `TempStoreDatabaseTest` свойства `$storeFactory` и `$collection` удалены, свойство `$objects` заменено на локальную переменную.

## Прочие изменения

- [#3155258](https://www.drupal.org/project/drupal/issues/3155258) Исправлено употребление Британского Английского «grey» на Английский вариант «gray».
- [#3130973](https://www.drupal.org/project/drupal/issues/3130973) Добавлена проверка переопределения сервисов баз данных.
- [#3156266](https://www.drupal.org/project/drupal/issues/3156266) Исправлено 70 различных опечаток в словах.
- [#3159535](https://www.drupal.org/project/drupal/issues/3159535) Исправлены опечатки `finegrained`, `perfoming`, `fieldeset`.