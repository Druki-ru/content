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

## CSS

- [#3166360](https://www.drupal.org/project/drupal/issues/3166360) CSSLint тестирование отключено для Drupal CI, так как его задачу покрывают тесты Stylelint. 

## CKEditor

- [#3170648](https://www.drupal.org/project/drupal/issues/3170648) Исправлена ошибка в коде приводящая к предупреждениям на PHP 8.
- [#3173037](https://www.drupal.org/project/drupal/issues/3173037) Запущен `yarn prettier` для `/core/modules/ckeditor/js/plugins/drupalimagecaption/plugin.es6.js`, который был пропущен перед коммитом.

## Claro

- [#3171366](https://www.drupal.org/project/drupal/issues/3171366) Из скомпилированных CSS файлов теперь удаляются комментарии.

## Comment

- [#2614720](https://www.drupal.org/project/drupal/issues/2614720) Исправлена неполадка привяодащая к фатальной ошибке при попытке загрузить/отрендерить «битые» (например, если комментарии по какой-то причине остались, а материал, к которому они принадлежат уже удалён) комментарии.

## Field

- [#2821352](https://www.drupal.org/project/drupal/issues/2821352) `EntityReferenceAutocompleteWidget` теперь не требует обязательно указывать настройку `target_bundles` для того чтобы работала настройка `auto_create`.

## File

- [#3156949](https://www.drupal.org/project/drupal/issues/3156949) Для вывода названия файла при попытке загрузить файл больше допустимого размера теперь используется `SplFileInfo::getClientOriginalName` вместо `SplFileInfo::getFilename`.

## Form System

- [#315176](https://www.drupal.org/project/drupal/issues/315176) Из `BookSettingsForm` удалено неиспользуемое значение `$form['array_filter']`.

## JavaScript

- [#3122002](https://www.drupal.org/project/drupal/issues/3122002) Для Nightwatch тестов `TestSiteInstallTestScript` заменён на `TestSiteMultilingualInstallTestScript` чтобы уменьшить количество обращений к внешним ресурсам (localize.drupal.org) и стабильность тестов.

## Mail System

- [#2822334](https://www.drupal.org/project/drupal/issues/2822334) Улучшена обработка значений в нижнем регистре в `Unicode::mimeHeaderDecode()`.

## Media System

- [#3124302](https://www.drupal.org/project/drupal/issues/3124302) Media Library теперь проверяет права доступа к ревизии сущности, которая редактируется.
- [#3071760](https://www.drupal.org/project/drupal/issues/3071760) oEmbed система больше не добавляет query параметры для `thumbnail_url`.
- [#3171743](https://www.drupal.org/project/drupal/issues/3171743) Media Library теперь позволяет переопределять формы загрузок для источников.

## Migration System

- [#3159849](https://www.drupal.org/project/drupal/issues/3159849) Добавлены отсутствующие тесты для свойства `filepath` в `FileTest`.
- [#3101045](https://www.drupal.org/project/drupal/issues/3101045) Плагин источника `LanguageContentSettingsTaxonomyVocabulary` теперь делает выборку по `language` только при её наличии.
- [#3110839](https://www.drupal.org/project/drupal/issues/3110839) У формы настроек подключения в Drupal Migrate UI убрано описание для префикса таблиц, которое вводило в заблуждение и находилось там по ошибке.
- [#3159101](https://www.drupal.org/project/drupal/issues/3159101) `\Drupal\migrate\Plugin\migrate\source\SqlBase` теперь позволяет делать корректные миграции с SQLite в качестве источника и любым назначением.
- [#2969551](https://www.drupal.org/project/drupal/issues/2969551) Теперь при исключении миграции указывают файл и строку с проблемой.
- [#2678510](https://www.drupal.org/project/drupal/issues/2678510) Из кода `CredentialForm` удалены `@todo` комментарии.
- [#3165763](https://www.drupal.org/project/drupal/issues/3165763) Тесты `MigrateFieldTest` и `MigrateFieldInstance` объеденены в один.
- [#3171510](https://www.drupal.org/project/drupal/issues/3171510) Добавлена документация для `default_bundle` в `destination\Entity`.
- [#3143721](https://www.drupal.org/project/drupal/issues/3143721) Тестирование источников вынесено в отдельный тест `SourceProviderTest`.
- [#2949400](https://www.drupal.org/project/drupal/issues/2949400) Из `MigrateProcessInterface` часть документации перенесена в `ProcessPluginBase` так как не является его частью.
- [#3119254](https://www.drupal.org/project/drupal/issues/3119254) Добавленны маппинги для фоматтеров референсов на термины таксономии `taxonomy_term_reference_plain` и `taxonomy_term_reference_rss_category` из Drupal 7.
- [#3110064](https://www.drupal.org/project/drupal/issues/3110064) Добавлена поддержка миграций специальных маршрутов: `<nolink>`, `<none>`.

## Node

- [#2586013](https://www.drupal.org/project/drupal/issues/2586013) Функция `node_views_analyze()` перенесена в `node.views.inc`.

## Path

- [#3158262](https://www.drupal.org/project/drupal/issues/3158262) Удалена неиспользуемая переменная `$aliasManager` в `AliasTest`.

## Search

- [#3173873](https://www.drupal.org/project/drupal/issues/3173873) Удалена неиспользуемая переменная `$pos` в `SearchQuery`.

## System

- [#3173976](https://www.drupal.org/project/drupal/issues/3173976) Удалена неиспользуемая переменная `$fiel_path` в `ConfigTest`.

## Taxonomy

- [#2132773](https://www.drupal.org/project/drupal/issues/2132773) К SQL запросам больше не добавляется тег `term_access` если включена опция «Отключить перезапись SQL».

## Прочие изменения

- [#3051932](https://www.drupal.org/project/drupal/issues/3051932) Ссылка в документации к `EntityStorageInterface::delete` исправлена на актуальную.
- [#3156878](https://www.drupal.org/project/drupal/issues/3156878) `DateTimePlus` теперь вызывает `checkdate()` только если значение не пустое (`!empty()`).
- [#3173435](https://www.drupal.org/project/drupal/issues/3173435) Исправлено повторение «not» в `PharExtensionInterceptor`.
- [#3173437](https://www.drupal.org/project/drupal/issues/3173437) Исправлено повторение «or» в `FieldOptionTranslation`.