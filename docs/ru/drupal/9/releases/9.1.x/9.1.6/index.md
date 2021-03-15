---
title: 'Drupal 9.1.6'
slug: 9/releases/9.1.6
core: 9
metatags:
  title: 'Drupal 9.1.6: Список изменений'
  description: 'Список изменений Drupal 9.1.6.'
---

**Дата релиза**: 7 апреля 2021

> [!WARNING]
> Данный релиз находится в разработке. Актуальная версия [Drupal 9.1.6](index.md).

## Migration System

- [#3200535](https://www.drupal.org/project/drupal/issues/3200535) В `ContentEntityTest` добавлены тесты с родительским ID.

## Views

- [#3165784](https://www.drupal.org/project/drupal/issues/3165784) `PathPluginBase` теперь задаёт своим [маршрутам](../../../routing/index.md) опцию `utf8` в значение `TRUE`.

## Тестирование

- [#3182653](https://www.drupal.org/project/drupal/issues/3182653) При запуске тестов на PHPUnit 9, теперь выдаётся сообщение о том что необходимо запросить пакет `phpspec/prophecy-phpunit` для корректной работы.
- [#3198400](https://www.drupal.org/project/drupal/issues/3198400) Сравнения с вовлечением `xpath` заменены более подходящими методами.
