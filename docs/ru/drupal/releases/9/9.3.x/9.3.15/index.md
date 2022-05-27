---
title: 'Drupal 9.3.15'
slug: wiki/drupal/releases/9.3.15
core: 9
metatags:
  title: 'Drupal 9.3.15: Список изменений'
  description: 'Список изменений Drupal 9.3.15.'
authors:
  - Niklan
category:
  area: 'Drupal 9.3.x'
  title: Drupal 9.3.15 (в разработке)
  order: 15
---

<Aside type="warning">

Данная версия находится в разработке и не предназначена для использования.

</Aside>

## CKEditor 5

- [#3269657](https://www.drupal.org/node/3269657) Добавлена миграция для `media_embed` фильтра из CKEditor 4 → 5.
- [#3276218](https://www.drupal.org/node/3276218) Включено тестирование unrestrcited ссылок.
- [#3278394](https://www.drupal.org/node/3278394) Исправлена неполадка, из-за которой `diff()` мог возвращать некорректный результат при наличии значения в аттрибуте.
- [#3259593](https://www.drupal.org/node/3259593) Функционал выравнивания теперь доступен только как выпадающий список без отдельных кнопок.
- [#3280602](https://www.drupal.org/node/3280602) Внесены улучшения в `HTMLRestrictions`.
- [#3275237](https://www.drupal.org/node/3275237) Внесены улучшения в `DrupalImageUploadEditing`.
- [#3277438](https://www.drupal.org/node/3277438) CKEditor обновлён до версии 34.1.0.

## Contact

- [#3260920](https://www.drupal.org/node/3260920) Исправлена некорректная операция в `MessageEntityTest`.

## Contextual Links

- [#2580263](https://www.drupal.org/node/2580263) Внесены улучшения в `contextual_preprecess()` для увеличения производительности (на 6%).

## Datetime

- [#2636086](https://www.drupal.org/node/2636086) В `FilterDateTest` добавлено больше проверок для операторов фильтров Views.

## Entity System

- [#3252100](https://www.drupal.org/node/3252100) Теперь значение `revision_default` поля также задаётся если текущая ревизия помечена ревизией по умолчанию.

## Extension System

- [#2513524](https://www.drupal.org/node/2513524) Исправлена неполадка, из-за которой модули не обнаруживались, если в строке с типом расширения был комментарий.

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

## Responsive Image

- [#3250582](https://www.drupal.org/node/3250582) Плагин источника миграции `ResponsiveImageStyles` теперь расширяет `DrupalSqlBase`.

## Search

- [#3218562](https://www.drupal.org/node/3218562) `SearchSimplifyTest` переименован в `SearchTextProcessorTest`.

## Seven

- [#3277552](https://www.drupal.org/node/3277552) Добавлено визуальное оформление фокусировки элементов CKEditor.

## Taxonomy

- [#3058409](https://www.drupal.org/node/3058409) Улучшена документация для `TermStorage::loadTree()`.

## Views

- [#2917239](https://www.drupal.org/node/2917239) Views больше не собирает форму настроек когда не используются поля.
- [#2314443](https://www.drupal.org/node/2314443) Исправлена неполадка, из-за которой изменение названия представления не обновляло заголовок страницы в предпросмотре.

## Прочие изменения

- [#3279502](https://www.drupal.org/node/3279502) Исправлены некорректные аннотации `@property`.
- [#3268746](https://www.drupal.org/node/3268746) Исправлены ошибки стандарта кодирования `Drupal.Commenting.DocComment.ShortSingleLine`.
- [#3232714](https://www.drupal.org/node/3232714) Создание экземпляра класса через мокинг заменено на вызов класса непосредственно, если никакой функционал мока не был при этом задействован.