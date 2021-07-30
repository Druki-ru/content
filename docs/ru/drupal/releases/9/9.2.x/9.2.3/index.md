---
title: 'Drupal 9.2.3'
slug: drupal/releases/9.2.3
core: 9
metatags:
  title: 'Drupal 9.2.3: Список изменений'
  description: 'Список изменений Drupal 9.2.3.'
---

> [!IMPORTANT]
> Данный релиз находится в разработке и не предназначен для использования.

## Database

* [#1479220](https://www.drupal.org/project/drupal/issues/1479220) Для метода `Merge::execute()` добавлена отсутствующая документация.

## File

* [#2834958](https://www.drupal.org/project/drupal/issues/2834958) Внесено улучшение в функцию `file_validate_extensions()` — теперь для управляемых (managed) файлов получение расширения производится по его URI, а не по названию файла, которое можно менять и может не содержать расширения.

## Media System

* [#3080666](https://www.drupal.org/project/drupal/issues/3080666) Исправлена неполадка, из-за которой oEmbed не мог получить миниатюру если в URL файла отсутствовало расширение файла. Теперь если расширение файла отсутствует, будет предпринята попытка определить его по MIME-типу.

## Text

* [#2750925](https://www.drupal.org/project/drupal/issues/2750925) Исправлена неполадка в генерации образцов значений при помощи `generateSampleValue()` для `TextItem` поля если максимальная длина поля менее 3 символов.

## Views

* [#3221933](https://www.drupal.org/project/drupal/issues/3221933) Исправлена неполадка приводящая к PHP Notice: «Notice: Undefined index: left_field in Drupal\views\Plugin\views\join\JoinPluginBase->__construct()».

## Тестирование

* [#3222783](https://www.drupal.org/project/drupal/issues/3222783) Результат вызова метода `PHPUnit\Framework\Assert::assertEquals()` больше не используется как результат теста.

## Прочие изменения

* [#3220379](https://www.drupal.org/project/drupal/issues/3220379) Пример с кодом для `NullCoalesce` теперь корректно отформатирован.