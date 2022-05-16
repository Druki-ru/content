---
title: 'Drupal 9.3.14'
slug: wiki/drupal/releases/9.3.14
core: 9
metatags:
  title: 'Drupal 9.3.14: Список изменений'
  description: 'Список изменений Drupal 9.3.14.'
authors:
  - Niklan
category:
  area: 'Drupal 9.3.x'
  title: Drupal 9.3.14 (в разработке)
  order: 14
---

<Aside type="warning">

Данная версия находится в разработке и не предназначена для использования.

</Aside>

## CKEditor 5

- [#3269657](https://www.drupal.org/node/3269657) Добавлена миграция для `media_embed` фильтра из CKEditor 4 → 5.
- [#3276218](https://www.drupal.org/node/3276218) Включено тестирование unrestrcited ссылок.

## Datetime

- [#2636086](https://www.drupal.org/node/2636086) В `FilterDateTest` добавлено больше проверок для операторов фильтров Views.

## Entity System

- [#3252100](https://www.drupal.org/node/3252100) Теперь значение `revision_default` поля также задаётся если текущая ревизия помечена ревизией по умолчанию.

## File

- [#3272336](https://www.drupal.org/node/3272336) Тесты модуля теперь используют тему оформления Stark вместо Classy.

## JSON:API

- [#3279103](https://www.drupal.org/node/3279103) Удалён неиспользуемый код из `JsonApiFunctionalTest`.

## Layout BUilder

- [#3278314](https://www.drupal.org/node/3278314) Улучшена документация для метода `InlineBlockUsageInterface::getUsage()`, который также может возвращать `FALSE`.

## Quick Edit

- [#3280614](https://www.drupal.org/node/3280614) Внесены улучшения в тест `QuickEditFileTest`.

## Render

- [#3270081](https://www.drupal.org/node/3270081) Исправлено форматирование документации в `RenderInterface`.

## Search

- [#3218562](https://www.drupal.org/node/3218562) `SearchSimplifyTest` переименован в `SearchTextProcessorTest`.

## Seven

- [#3277552](https://www.drupal.org/node/3277552) Добавлено визуальное оформление фокусировки элементов CKEditor.

## Views

- [#2917239](https://www.drupal.org/node/2917239) Views больше не собирает форму настроек когда не используются поля.
- [#2314443](https://www.drupal.org/node/2314443) Исправлена неполадка, из-за которой изменение названия представления не обновляло заголовок страницы в предпросмотре.

## Прочие изменения

- [#3279502](https://www.drupal.org/node/3279502) Исправлены некорректные аннотации `@property`.