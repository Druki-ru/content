---
title: 'Drupal 9.1.8'
slug: 9/releases/9.1.8
core: 9
metatags:
  title: 'Drupal 9.1.8: Список изменений'
  description: 'Список изменений Drupal 9.1.8.'
---

> [!WARNING]
> Данный релиз находится в разработке.

## Aggregator

* [#3212354](https://www.drupal.org/project/drupal/issues/3212354) `AggregatorFeedBlock::build()` теперь всегда возвращает массив.

## Entity System

* [#2608750](https://www.drupal.org/project/drupal/issues/2608750) Теперь нельзя создать поле-связи на сущность, у которой отсутствует ключ `id`.

## Install System

* [#3145563](https://www.drupal.org/project/drupal/issues/3145563) Улучшена сериализация маршрутов для лучшей совместимости между PHP 7.4 и 7.3.

## Migration System

* [#3212539](https://www.drupal.org/project/drupal/issues/3212539) Добавлены соответствия для форматтеров ссылок из D7.

## Views

* [#3212521](https://www.drupal.org/project/drupal/issues/3212521) Исправлены опечатки в `core/modules/views/src/Plugin/views/filter/FilterPluginBase.php`.
