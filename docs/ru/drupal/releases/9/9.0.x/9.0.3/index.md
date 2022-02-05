---
title: 'Drupal 9.0.3'
slug: wiki/drupal/releases/9.0.3
core: 9
metatags:
  title: 'Drupal 9.0.3: Список изменений'
  description: 'Список изменений Drupal 9.0.3.'
authors:
  - Niklan
category:
  area: 'Drupal 9.0.x'
  title: Drupal 9.0.3
  order: 3
---

**Дата релиза**: 5 августа 2020

<Aside>

Данный релиз также содержит изменения внесенные в [Drupal 8.9.3](../../../8/8.9.x/8.9.3/index.md).

</Aside>

## Batch System

- [#2939645](https://www.drupal.org/project/drupal/issues/2939645) В документации к финальному обратному вызову [пакетной обработки данных](../../../../9/batches/index.md) добавлен четвёртый параметр с затраченным временем на обработку.

## Composer

- [#3159735](https://www.drupal.org/project/drupal/issues/3159735) Шаблоны проектов [drupal/recommended-project](../../../../../composer/drupal/recommended-project/index.md) и [drupal/legacy-project](../../../../../composer/drupal/legacy-project/index.md) теперь упоминают Drupal 9 в описании.

## Database System

- [#3152390](https://www.drupal.org/project/drupal/issues/3152390) В тестах, для прямых запросов поля заключены в квадратный скобки.

## Locale

- [#3128389](https://www.drupal.org/project/drupal/issues/3128389) Для `LocaleTranslation` добавлен `DependencySerializationTrait` чтобы он корректно сериализовался.

## Media System

- [#3102402](https://www.drupal.org/project/drupal/issues/3102402) Виджет Media Library больше не добавляет поле «weight» если можно выбрать всего одно значение.

## Migrate Drupal

- [#3157975](https://www.drupal.org/project/drupal/issues/3157975) Удалена неиспользуемая переменная `$system_data`.

## Migrate System

- [#3133516](https://www.drupal.org/project/drupal/issues/3133516) Плагин обработки `extract` теперь может принимать `NULL` в качестве `default_value` и обрабатывать его корректно.

## Search

- [#3047719](https://www.drupal.org/project/drupal/issues/3047719) Справка из `hook_help()` конвертирована в Help Topics.

## SQLite

- [#3145412](https://www.drupal.org/project/drupal/issues/3145412) При удалении пустой БД, теперь происходит отключение данной БД, а только затем удаление. Это решает проблему когда файл БД невозможно удалить из-за недостаточных прав доступа так как она используется.

## Theme system

- [#3155159](https://www.drupal.org/project/drupal/issues/3155159) Комментарий генерируемый для стилей из PostCSS теперь имеет корректную ссылку на изменение связанное с PostCSS, вместо JavaScript.

## User

- [#2934904](https://www.drupal.org/project/drupal/issues/2934904) В тесте `TempStoreDatabaseTest` свойства `$storeFactory` и `$collection` удалены, свойство `$objects` заменено на локальную переменную.

## Views

- [#3113986](https://www.drupal.org/project/drupal/issues/3113986) Добавлена поддержка отображения автора ревизии для Media.
- [#3131126](https://www.drupal.org/project/drupal/issues/3131126) Добавлена поддержка отображения автора ревизии для Block Content.

## Прочие изменения

- [#3155258](https://www.drupal.org/project/drupal/issues/3155258) Исправлено употребление Британского Английского «grey» на Английский вариант «gray».
- [#3130973](https://www.drupal.org/project/drupal/issues/3130973) Добавлена проверка переопределения сервисов баз данных.
- [#3156266](https://www.drupal.org/project/drupal/issues/3156266) Исправлено 70 различных опечаток в словах.
- [#3159535](https://www.drupal.org/project/drupal/issues/3159535) Исправлены опечатки `finegrained`, `perfoming`, `fieldeset`.
- [#3161992](https://www.drupal.org/project/drupal/issues/3161992) Использование устаревшего `ResourceResponse::create()` заменено на `new ResourceResponse()`.
- [#3162031](https://www.drupal.org/project/drupal/issues/3162031) `Symfony\Component\HttpKernel\Event\ViewEvent` в Symfony 5.1 является `final`, поэтому не может быть замокан. Мок теперь создаётся по интерфейсу.

## Ссылки

- [Drupal 9.0.3](https://www.drupal.org/project/drupal/releases/9.0.3) (англ.), drupal.org, 5 августа 2020