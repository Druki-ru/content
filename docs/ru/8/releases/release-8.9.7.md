---
id: release-8.9.7
title: 'Drupal 8.9.7'
path: /8/releases/8.9.7
core: 8
metatags:
  title: 'Drupal 8.9.7: Список изменений'
  description: 'Список изменений Drupal 8.9.7.'
---

**Дата релиза**: ожидается 7 октября 2020

> [!WARNING]
> Drupal 8.9.7 находится в разработке.

## CKEditor

- [#3170648](https://www.drupal.org/project/drupal/issues/3170648) Исправлена ошибка в коде приводящая к предупреждениям на PHP 8.

## Form System

- [#315176](https://www.drupal.org/project/drupal/issues/315176) Из `BookSettingsForm` удалено неиспользуемое значение `$form['array_filter']`.

## JavaScript

- [#3122002](https://www.drupal.org/project/drupal/issues/3122002) Для Nightwatch тестов `TestSiteInstallTestScript` заменён на `TestSiteMultilingualInstallTestScript` чтобы уменьшить количество обращений к внешним ресурсам (localize.drupal.org) и стабильность тестов.

## Mail System

- [#2822334](https://www.drupal.org/project/drupal/issues/2822334) Улучшена обработка значений в нижнем регистре в `Unicode::mimeHeaderDecode()`.

## Media System

- [#3124302](https://www.drupal.org/project/drupal/issues/3124302) Media Library теперь проверяет права доступа к ревизии сущности, которая редактируется.
- [#3071760](https://www.drupal.org/project/drupal/issues/3071760) oEmbed система больше не добавляет query параметры для `thumbnail_url`.

## Migration System

- [#3159849](https://www.drupal.org/project/drupal/issues/3159849) Добавлены отсутствующие тесты для свойства `filepath` в `FileTest`.
- [#3101045](https://www.drupal.org/project/drupal/issues/3101045) Плагин источника `LanguageContentSettingsTaxonomyVocabulary` теперь делает выборку по `language` только при её наличии.
- [#3110839](https://www.drupal.org/project/drupal/issues/3110839) У формы настроек подключения в Drupal Migrate UI убрано описание для префикса таблиц, которое вводило в заблуждение и находилось там по ошибке.
- [#3159101](https://www.drupal.org/project/drupal/issues/3159101) `\Drupal\migrate\Plugin\migrate\source\SqlBase` теперь позволяет делать корректные миграции с SQLite в качестве источника и любым назначением.
- [#2969551](https://www.drupal.org/project/drupal/issues/2969551) Теперь при исключении миграции указывают файл и строку с проблемой.