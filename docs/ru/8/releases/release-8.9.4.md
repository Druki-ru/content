---
id: release-8.9.4
title: 'Drupal 8.9.4'
path: /8/releases/8.9.4
core: 8
metatags:
  title: 'Drupal 8.9.4: Список изменений'
  description: 'Список изменений Drupal 8.9.4.'
---

**Дата релиза**: ожидается 2 сентября 2020

> [!WARNING]
> Данная версия находится в разработке. Актуальная версия [Drupal 8.9.3](release-8.9.3.md). 

## Composer

- [#3130685](https://www.drupal.org/project/drupal/issues/3130685) Исправлена ошибка стандартов кодирования для генерируемого [drupal/core-composer-scaffold](../../composer/drupal-core-composer-scaffold.md) `autoload.php` файла.

## Content Moderation

- [#3112433](https://www.drupal.org/project/drupal/issues/3112433) Исправлена неполадка из-за которой были некорректные результаты при фильтрации по статусу материала и использовании группировки.

## Database System

- [#3160267](https://www.drupal.org/project/drupal/issues/3160267) Употребление статичных запросов в тестах заменено на динамические запросы.

## Field System

- [#3163687](https://www.drupal.org/project/drupal/issues/3163687) Удалена неиспользуемая переменная `$id` в `BulkDeleteTest`.

## Filter

- [#3151101](https://www.drupal.org/project/drupal/issues/3151101) Употребление «whitelist» и «blacklist» в Filter модуле заменено на более подходящие.

## JSON:API

- [#3025372](https://www.drupal.org/project/drupal/issues/3025372) Исправлена неполадка из-за которой при фильтрации мог быть пустой объект для связи.

## Migration System

- [#3116841](https://www.drupal.org/project/drupal/issues/3116841) Исправлена фикстура для drupal6 миграций Node 1.

## Прочие изменения

- [#3163703](https://www.drupal.org/project/drupal/issues/3163703) Исправлена опечатка в `DeprecatedServicePropertyTrait`.