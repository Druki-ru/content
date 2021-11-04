---
title: 'Drupal 9.2.8'
slug: drupal/releases/9.2.8
core: 9
metatags:
  title: 'Drupal 9.2.8: Список изменений'
  description: 'Список изменений Drupal 9.2.8.'
---

**Дата релиза**: 3 ноября 2021

## Asset Library System

* [#2936067](https://www.drupal.org/node/2936067) Внесены улучшения в CSS агрегатор, который теперь корректно обрабатывает больше различных вариантов использования `@import`.
* [#2945033](https://www.drupal.org/node/2945033) Исправлена неполадка, из-за которой было невозможно указать `rel="alternate"` с одниаковыми адресами, но разными `hreflang`.

## CKeditor

* [#2763075](https://www.drupal.org/node/2763075) Исправлена неполадка приводящая к ошибке «Uncaught TypeError: f.format_tags.split is not a function».

## Claro

* [#3219340](https://www.drupal.org/node/3219340) Исправлена неполадка с отображением вертикальных вкладок которые имеют `#parents`.
* [#3203745](https://www.drupal.org/node/3203745) Внесены улучшения в `claro_form_system_modules_alter()` для более стабильной работы при изменении форм.

## Composer

* [#3241318](https://www.drupal.org/node/3241318) Удалён скрипт `core/tests/Drupal/Tests/Composer/Plugin/Scaffold/fixtures/scripts/disable-git-bin/git`. Теперь он генерируется на лету.

## Database System

* [#3244156](https://www.drupal.org/node/3244156) Внесены улучшения в `EntityQueryAggregateTest` для корректной работы на SQL серверах.

## Editor

* [#2974156](https://www.drupal.org/node/2974156) Исправлена неполадка в `editor_entity_update()`, из-за которой обработка могла производиться несколько раз, если сущность сохраняется несколько раз в одном процессе.

## File System

* [#3192365](https://www.drupal.org/node/3192365) Исправлена неполадка в `TestRunnerKernel`, которая приводила к состоянию гонки при попытке создать `public://simpletest`.

## Form API

* [#990218](https://www.drupal.org/node/990218) Внесены улучшения в элемент `machine_name` исправляющие предупреждения.

## JSON:API

* [#3228000](https://www.drupal.org/node/3228000) Теперь, отправка `DELETE` на `/jsonapi/user/{UUID}` учитывает настройки того, что необходимо делать с аккаунтом при его удалении.
* [#3083561](https://www.drupal.org/node/3083561) Добавлено покрытие тестами для фильтрации значений по datetime поле.

## Layout Builder

* [#3020876](https://www.drupal.org/node/3020876) Исправлена неполадка, из-за которой контекстуальные ссылки для переиспользуемых блоков не отображались при использовании в Layout Builder.

## Media System

* [#3205866](https://www.drupal.org/node/3205866) `media_requirements()` теперь сообщаются об отсутствующих полях-источниках.
* [#3073294](https://www.drupal.org/node/3073294) Удалён устаревший `@todo Inserting media embed should enable undo.`.
* [#3208849](https://www.drupal.org/node/3208849) `OEmbedWidget` теперь выводит справочный текст поля.

## Menu UI

* [#2957953](https://www.drupal.org/node/2957953) Теперь, после добавления или редактирования ссылки, пользователь перенаправляется на страницу конкретного меню, а не списка всех типов меню.

## Migration System

* [#3219140](https://www.drupal.org/node/3219140) Исправлена неполадка, из-за которой `d7_language_content_comment_settings` вызывал исключение если источник использовал для бандла строку более 32 символов.
* [#3199578](https://www.drupal.org/node/3199578) Внесены улучшения в `EntityReferenceTranslationDeriver` для того чтобы поля корректно указывали на переводы.
* [#3187616](https://www.drupal.org/node/3187616) Внесены улучшения в `TermTranslation` запроса для миграций где используется `i18n_mode` отличный от 0 и 1.
* [#3232681](https://www.drupal.org/node/3232681) Исправлена неполадка из-за которой плагин-обработчик `FieldLink` обрабатывал внешние URL-адреса с относительным протоколом (`//example.com`) как внутренние пути.
* [#3088917](https://www.drupal.org/node/3088917) Для длинных полей с форматером `text_plain` теперь используется форматер `basic_string`.
* [#2784783](https://www.drupal.org/node/2784783) Исправлена неполадка при миграции материалов с CCK `nodereferrer` полями.
* [#3203009](https://www.drupal.org/node/3203009) Улучшена документация для методов `FieldableEntity`.
* [#3225227](https://www.drupal.org/node/3225227) В плагинах-источниках улучшена документация.

## Olivero

* [#3223281](https://www.drupal.org/node/3223281) Исправлена неполадка, из-за которой иконка поиска была невидимой в режиме принудительного выбора цветов ([forced-colors](https://developer.mozilla.org/en-US/docs/Web/CSS/@media/forced-colors)) в MS Edge.
* [#3223271](https://www.drupal.org/node/3223271) Исправлена контрастность иконок выпадающих меню для режима повышенной контрастности в Windows.
* [#3242456](https://www.drupal.org/node/3242456) Исправлена контрастность для `fieldset` элементов.
* [#3242469](https://www.drupal.org/node/3242469) Исправлена контрастность для неактивных вертикальных меток элементов форм.
* [#3212670](https://www.drupal.org/node/3212670) Исправлена неполадка с `z-index` строки поиска.

## Toolbar

* [#3227370](https://www.drupal.org/node/3227370) В тем хук `menu__toolbar` добавлена переменная `menu_name`, которая была по ошибке утеряна.

## Views

* [#3231861](https://www.drupal.org/node/3231861) Исправлена неполадка, из-за которой переопределение настроек запроса приводило к исключению.
* [#3116481](https://www.drupal.org/node/3116481) Тест `EntityViewsDataTest` конвертирован из Unit-теста в Kernel-тест.

## Ссылки

- [Drupal 9.2.8](https://www.drupal.org/project/drupal/releases/9.2.8) (англ.), drupal.org, 3 ноября 2021
