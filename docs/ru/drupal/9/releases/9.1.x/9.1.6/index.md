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

## Aggregator

* [#3203369](https://www.drupal.org/project/drupal/issues/3203369) Вызовы Entity Query в модуле больше не учитывают права доступа.

## Asset Library System

* [#2407187](https://www.drupal.org/project/drupal/issues/2407187) Оптимизирован пересчёт зависимостей библиотек, что даёт примерный прирост производительности в >=4% для времени генерации страницы в целом.

## Block

* [#3199730](https://www.drupal.org/project/drupal/issues/3199730) (откачено) Исправлена неполадка, из-за которой описание блока, созданного Views, санитизировалось дважды.

## Block Content

* [#3203625](https://www.drupal.org/project/drupal/issues/3203625) `BlockContentUuidLookup::resolveCacheMiss()` больше не учитывает права доступа.

## CSS

* [#2958588](https://www.drupal.org/project/drupal/issues/2958588) Исправлены неполадки в стилях для «Off canvas» элемента приводящие к некорректному отображению.

## Entity System

* [#3202040](https://www.drupal.org/project/drupal/issues/3202040) Формы массового удаления сущностей (`block_content`, `comment`, `media`, `node`) больше не учитывают права доступа.
* [#3201714](https://www.drupal.org/project/drupal/issues/3201714) Чистка «мусора» за удалённой сущностью больше не учитывает права доступа.

## Field System

* [#2917606](https://www.drupal.org/project/drupal/issues/2917606) Исправлена неполадка, из-за которой вызов `FieldStorageConfigStorage::loadByProperties()` с передачей `entity_type` и `field_name` возвращал пустой результат.
* [#3094366](https://www.drupal.org/project/drupal/issues/3094366) Строковый форматтер больше не позволяет делать значение ссылкой на содержимое, если у содержимого нет `canonical` маршрута.

## Field UI

* [#3202440](https://www.drupal.org/project/drupal/issues/3202440) `FieldStorageConfigEditForm::validateCardinality()` больше не учитывает права доступа.

## Help Topics

* [#3047722](https://www.drupal.org/project/drupal/issues/3047722) Справка для модулей `content_moderations` и `workflows` конвертирована в Help Topics.

## Layout Builder

* [#3203401](https://www.drupal.org/project/drupal/issues/3203401) `LayoutBuilderEntityViewDisplayForm::hasOverrides()` больше не учитывает права доступа.
* [#3090941](https://www.drupal.org/project/drupal/issues/3090941) Layout Builder больше не ожидает что все элементы [хлебных крошек](../../../services/tagged/breadcrumb-builder/index.md) имеют свои [маршруты](../../../routing/routes-and-controllers/index.md).

## Media System

* [#3204389](https://www.drupal.org/project/drupal/issues/3204389) `MediaRevisionAccessCheck::countDefaultLanguageRevisions()` больше не учитывает права доступа.
* [#3008712](https://www.drupal.org/project/drupal/issues/3008712) oEmbed теперь проверяет все доступные точки доступа для одного провайдера, вместо первого попавшегося.
* [#3106659](https://www.drupal.org/project/drupal/issues/3106659) Типы мультимедиа, что не имеют поля с источником больше не выводятся на странице отчёта состояния.

## Menu Link Content

* [#3204138](https://www.drupal.org/project/drupal/issues/3204138) `MenuLinkContentDeriver::getDerivativeDefinitions()` больше не учитывает права доступа.

## Menu UI

* [#3204140](https://www.drupal.org/project/drupal/issues/3204140) `menu_ui_get_menu_link_defaults()` больше не учитывает права доступа.

## Migration System

* [#3200535](https://www.drupal.org/project/drupal/issues/3200535) В `ContentEntityTest` добавлены тесты с родительским ID.
* [#3067609](https://www.drupal.org/project/drupal/issues/3067609) Исправлена неполадка при миграции значений по умолчанию для ссылок.
* [#3199999](https://www.drupal.org/project/drupal/issues/3199999) Добавлена документация для плагинов-источников языков сайта.
* [#3203265](https://www.drupal.org/project/drupal/issues/3203265) Улучшен тайпхинт в документации для свойства `MigrateProcessTestCase::$row`.
* [#3204986](https://www.drupal.org/project/drupal/issues/3204986) Добавлена документация для плагинов-источников полей.

## MySQL DB Driver

* [#3205024](https://www.drupal.org/project/drupal/issues/3205024) Добавлен отсутствующий `use` в `Drupal\Core\Database\Driver\mysql\Connection`.

## Olivero

* [#3202808](https://www.drupal.org/project/drupal/issues/3202808) Исправлен некорректный синтаксис в `book-tree.html.twig`.

## System

* [#3106455](https://www.drupal.org/project/drupal/issues/3106455) Исправлено использование несуществующей переменной в `TestFileTransfer`.
* [#3205344](https://www.drupal.org/project/drupal/issues/3205344) Вызов `system_requirements()` теперь производится только при наличии у пользователя прав доступа к данной странице.

## Tracker

* [#3202107](https://www.drupal.org/project/drupal/issues/3202107) При массовом удалении сущностей модуля Tracker, теперь выводится предупреждение со всеми материалами, независимо от прав доступа.

## User

* [#3203366](https://www.drupal.org/project/drupal/issues/3203366) `user_is_blocked()` больше не учитывает права доступа.

## Views

* [#3165784](https://www.drupal.org/project/drupal/issues/3165784) `PathPluginBase` теперь задаёт своим [маршрутам](../../../routing/index.md) опцию `utf8` в значение `TRUE`.

## Workspaces

* [#3203596](https://www.drupal.org/project/drupal/issues/3203596) `WorkspacePublisher::getDifferringRevisionIdsOnTarget()` больше не учитывает права доступа.
* [#3192363](https://www.drupal.org/project/drupal/issues/3192363) Добавлен тест, проверяющий, что для сущности `workspace` нельзя включить функционал `moderation` путём удаления соответствующего хендлера с сущности.

## Тестирование

* [#3182653](https://www.drupal.org/project/drupal/issues/3182653) При запуске тестов на PHPUnit 9, теперь выдаётся сообщение о том что необходимо запросить пакет `phpspec/prophecy-phpunit` для корректной работы.
* [#3198400](https://www.drupal.org/project/drupal/issues/3198400) Сравнения с вовлечением `xpath` заменены более подходящими методами.
* [#3202915](https://www.drupal.org/project/drupal/issues/3202915) Сравнения, использующие `xpath` на текстовые области, заменены более подходящими методами.
* [#3204764](https://www.drupal.org/project/drupal/issues/3204764) Исправлены тесты, которые имели `return`.

## Прочие изменения

* [#3196388](https://www.drupal.org/project/drupal/issues/3196388) Исправлена ссылка ведущая на ответственность мейнтенеров.
* [#3034324](https://www.drupal.org/project/drupal/issues/3034324) Исправлен тайпхинт в документации параметра `$property` метода `FormStateInterface::has()`.
* [#3196699](https://www.drupal.org/project/drupal/issues/3196699) Названия хендлеров в `EntityTypeInterface` исправлены на корректные.
* [#3204353](https://www.drupal.org/project/drupal/issues/3204353) Исправлен пример для `hook_link_alter()`.
* [#2850057](https://www.drupal.org/project/drupal/issues/2850057) В интерфейс `SelectionInterface` добавлена отсутствующая документация параметров.