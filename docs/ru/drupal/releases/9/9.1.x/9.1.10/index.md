---
title: 'Drupal 9.1.10'
slug: wiki/drupal/releases/9.1.10
core: 9
metatags:
  title: 'Drupal 9.1.10: Список изменений'
  description: 'Список изменений Drupal 9.1.10.'
authors:
  - Niklan
  - chesn0k
  - arraksis
category:
  area: 'Drupal 9.1.x'
  title: Drupal 9.1.10
  order: 10
---

## Aggregator

- [#3212354](https://www.drupal.org/project/drupal/issues/3212354) `AggregatorFeedBlock::build()` теперь всегда возвращает массив.

## CKEditor

- [#3215970](https://www.drupal.org/project/drupal/issues/3215970) CKeditor откачен до версии 4.16.1, чтобы соответствовать версиям обновлений для [SA-CORE-2021-003](../../../../security/sa-core/2021-003/index.md).

## Database System

- [#3210913](https://www.drupal.org/project/drupal/issues/3210913) Исправлена ошибка в `DbDumpCommandTest`.

## Entity System

- [#2608750](https://www.drupal.org/project/drupal/issues/2608750) Теперь нельзя создать поле-связи на сущность, у которой отсутствует ключ `id`.

## Install System

- [#3145563](https://www.drupal.org/project/drupal/issues/3145563) Улучшена сериализация маршрутов для лучшей совместимости между PHP 7.4 и 7.3.

## Migration System

- [#3212539](https://www.drupal.org/project/drupal/issues/3212539) Добавлены соответствия для форматтеров ссылок из D7.

## Plugin System

- [#3126747](https://www.drupal.org/project/drupal/issues/3126747) `ContextDefinition` при обнаружении типа данных начинающегося с `entity:*` теперь передаёт его `EntityContextDefinition`.

## Statistics

- [#3190820](https://www.drupal.org/project/drupal/issues/3190820) Улучшена подготовка токенов в `statistics_tokens()` которая ранее могла приводить к предупреждениям.

## Views

- [#3212521](https://www.drupal.org/project/drupal/issues/3212521) Исправлены опечатки в `core/modules/views/src/Plugin/views/filter/FilterPluginBase.php`.
- [#2868258](https://www.drupal.org/project/drupal/issues/2868258) `View::preRenderViewElement()` теперь выбрасывает исключение если представление не найдено.

## Тестирование

- [#3131281](https://www.drupal.org/project/drupal/issues/3131281) Использование `::assertEqual()` заменено на `::assertEquals()`.
- [#3139404](https://www.drupal.org/project/drupal/issues/3139404) Использование устаревшего `AssertLegacyTrait::assertText()` заменено на современные аналоги.
- [#3186661](https://www.drupal.org/project/drupal/issues/3186661) Удалены вызовы `::drupalPostForm()`.

## Прочие изменения

- [#3211936](https://www.drupal.org/project/drupal/issues/3211936) Исправлено «состояние гонки» при создании директорий для [стилей изображений](../../../../9/image/image-styles/index.md).
- [#3207734](https://www.drupal.org/project/drupal/issues/3207734) Исправлены ошибки стандартов кодирования `Drupal.Commenting.InlineVariableComment`.

## Ссылки

- [Drupal 9.1.10](https://www.drupal.org/project/drupal/releases/9.1.10) (англ.), drupal.org, 4 июня 2021