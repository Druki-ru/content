---
id: release-8.9.2
title: 'Drupal 8.9.2'
path: /8/releases/8.9.2
core: 8
metatags:
  title: 'Drupal 8.9.2: Список изменений'
  description: 'Список изменений Drupal 8.9.2.'
---

**Дата релиза**: В разработке

> [!WARNING]
> Данный релиз находится в разработке.

## Aggregator

- [#3146474](https://www.drupal.org/project/drupal/issues/3146474) Удалена неиспользуемая переменная `$next` в `AggregatorController.php` файле.

## Basic Auth

- [#3149799](https://www.drupal.org/project/drupal/issues/3149799) Код метода `BasicAuth::authenticate` поправлен для соответствия интерфейсу.

## Batch System

- [#3028621](https://www.drupal.org/project/drupal/issues/3028621) Добавлена документация о том, что при формировании [пакетной обработки данных](../batches/batches.md) метод `::setFile()` должен вызываться до `::addOperation()`, если файл содержит код функции обратного вызова.

## Block Content

- [#3137430](https://www.drupal.org/project/drupal/issues/3137430) Убрано повторное описание `label_collection` в аннотации сущности.

## Claro

- [#3097540](https://www.drupal.org/project/drupal/issues/3097540) Исправлена неполадка, из-за которой метка «Машинное имя» перекрывалось оформление фокусировки инпута.

## Composer

- [#3134648](https://www.drupal.org/node/3134648) `drupal/core-recommended` больше не запрашивает `composer/installers`.
- [#3127918](https://www.drupal.org/project/drupal/issues/3127918) Обновлена информация о пакетах для того чтобы появилась информация о поддержке проектов используемых в Drupal. Это новая возможность [Composer](../../composer/composer.md) [1.10](https://blog.packagist.com/composer-fund/).
- [#3153677](https://www.drupal.org/project/drupal/issues/3153677) `composer.lock` файл обновлён для соответствия актуальным хэшей зависимостей, который поменялись в [Drupal 8.9.1](release-8.9.1.md).
- [#3154611](https://www.drupal.org/project/drupal/issues/3154611) `composer.lock` файл обновлён для соответствия структуре Composer 1.10.

## Content Moderation

- [#2946750](https://www.drupal.org/project/drupal/issues/2946750) Исправлена неполадка, при которой все типы материалов становились ревизионными даже если они не находились под moderation workflow.

## Field

- [#2673688](https://www.drupal.org/project/drupal/issues/2673688) Удалены оставшиеся упоминания `hook_field_schema()` которого больше нет.

## File

- [#3150661](https://www.drupal.org/project/drupal/issues/3150661) Исправлено некорректное использование XPath.

## Plugin System

- [#2904467](https://www.drupal.org/project/drupal/issues/2904467) Теперь в качестве ключей аннотации могут быть использованы числа.

## Views

- [#2947588](https://www.drupal.org/project/drupal/issues/2947588) В файле `\Drupal\Tests\views\Kernel\ModuleTest` удалены неиспользуемые тесты, а те что используются приведены в актуальное состояние.
- [#3150474](https://www.drupal.org/project/drupal/issues/3150474) Исправлена документация о возвращаемом типе данных `Drupal\views\Views::getView`.
- [#3143316](https://www.drupal.org/project/drupal/issues/3143316) Улучшены проверки, для предотвращения появления исключения «Getting the base fields is not supported for entity type».

## Update

- [#3150990](https://www.drupal.org/project/drupal/issues/3150990) Исправлена ошибка, приводящая к падению установки Drupal при исключении.

## User

- [#3072305](https://www.drupal.org/project/drupal/issues/3072305) Исправлена неполадка, приводящая к ошибке `Notice: Undefined index: #item in user_user_view_alter()`.

## Тестирование

- [#3139132](https://www.drupal.org/project/drupal/issues/3139132) В тесте `BigPipeTest` использование `LIMIT` непосредственно в запросе заменено на вызов `queryRange()`.
- [#3139422](https://www.drupal.org/project/drupal/issues/3139422) Использование устаревшего `AssertLegacyTrait::assertOptionByText` заменено на `$this->assertSession()->optionExists()`.
- [#3142752](https://www.drupal.org/project/drupal/issues/3142752) Удалена передача аргумента `$message` в оставшихся вызовах `::assertEscaped()` и `::assertNoEscaped()`.
- [#3139402](https://www.drupal.org/project/drupal/issues/3139402) Использование `AssertLegacyTrait::assertIdenticalObject` заменено на `$this->assertEquals()`.
- [#3139414](https://www.drupal.org/project/drupal/issues/3139414) Использование `AssertLegacyTrait::assertLink` и `AssertLegacyTrait::assertNoLink` заменены на `$this->assertSession()->linkExists()`.
- [#3150731](https://www.drupal.org/project/drupal/issues/3150731) В тесте `FileSystemModuleDiscoveryDataProviderTrait` использование захардкоженных `/` в формировании пути заменено на `DIRECTORY_SEPARATOR`.

## Прочие изменения

- [#3151087](https://www.drupal.org/project/drupal/issues/3151087) Употребление «whitelist» заменено на «allowed*» в функции `file_munge_filename()` и её тестах.
- [#2937513](https://www.drupal.org/project/drupal/issues/2937513) Исправлены ошибки для соответствия `Drupal.Commenting.DocComment.TagGroupSpacing`.
- [#3143196](https://www.drupal.org/project/drupal/issues/3143196) Ссылка на загрузку в `CHANGELOG.txt` заменена с конкретной на алиас последней актуальной версии.
- [#3133033](https://www.drupal.org/project/drupal/issues/3133033) Исправлены ошибки для соответствия `Drupal.Array.Array.LongLineDeclaration`.
- [#3151091](https://www.drupal.org/project/drupal/issues/3151091) Употребление «whitelist» заменено на «allowed*» в классе `Drupal\Component\Utility\Xss` и его тестах.
- [#3150471](https://www.drupal.org/project/drupal/issues/3150471) Некорректно указанная константа `TrustedCallbackInterface::TRIGGER_DEPRECATION` в документации `DoTrustedCallbackTrait::doTrustedCallback` заменена на верную `TrustedCallbackInterface::TRIGGER_WARNING`.
- [#3138791](https://www.drupal.org/project/drupal/issues/3138791) Исправлены опечатки в слове «bubbleable».
- [#3138788](https://www.drupal.org/project/drupal/issues/3138788) Исправлены опечатки в слове «autcomplete».
- [#3154203](https://www.drupal.org/project/drupal/issues/3154203) Исправлены опечатки в слове «appear».
- [#3154533](https://www.drupal.org/project/drupal/issues/3154533) Исправлены опечатки в слове «Drupal».
- [#3138796](https://www.drupal.org/project/drupal/issues/3138796) Исправлены опечатки в слове «cotrol».

## Ссылки

- [Drupal 8.9.1](https://www.drupal.org/project/drupal/releases/8.9.1) (англ.), drupal.org, 17 июня 2020