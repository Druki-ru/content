---
title: 'Drupal 8.7.8'
slug: wiki/drupal/releases/8.7.8
core: 8
metatags:
  title: 'Drupal 8.7.8: Список изменений'
  description: 'Список изменений Drupal 8.7.8.'
authors:
  - Niklan
---

**Дата релиза**: 2 октября 2019 г.

## Поддержка указания версий ядра

- [#3070687](https://www.drupal.org/node/3070687)

В версии [Drupal 8.7.7](../8.7.7/index.md) была добавлена поддержка ключа `core_version_requirement` для `*.info.yml` файлов, позволяющая [модулям](../../../../8/modules/index.md) указывать совместимость с версией ядра. Это изменение также позволяет указывать, что модуль совместим с [Durpal 8](../../../../8/index.md) и предстоящим [Drupal 9](../../../../9/index.md) релизом.

## Обновление зависимостей

- [#3059356](https://www.drupal.org/node/3059356) Были обновлены следующие JavaScript библиотеки:
  - `nightwatch` обновлён до версии 1.2.1
  - `chromedriver` обновлён до версии 75.1.0
  - `stylelint-no-browser-hacks` обновлён до 1.2.1
- [#3045483](https://www.drupal.org/node/3045483) В связи с проблемами совместимости между `zend-diactoros` 1.8.5 и `psr-http-message-bridge` версиями до 1.1.2, Drupal повышает минимальное требование к `psr-http-message-bridge` с 1.0 до 1.1.2.

## Тесты

- [#3083183](https://www.drupal.org/node/3083183) Исправлены ошибки тестов `FunctionalJavaScript` происходящие из-за новых версий `curl` и `selenium`, которые стали требовать новые заголовки.
- [#3079805](https://www.drupal.org/node/3079805) Использование `expectException()` и `expectExceptionMessage()` заменены на старый вариант `setExpectedException()`, так как это вызывает проблемы с обратной совместимостью PHP 5.

## Migrate API

- [#2890514](https://www.drupal.org/node/2890514) Исправлена ошибка в миграции `upgrade_d6_imagecache_presets` при отсутствующем `action` для стиля изображения.
- [#3012001](https://www.drupal.org/node/3012001) Исправлена ошибка, из-за которой могли появляться дубли в `source`.

## Entity API

- [#3068733](https://www.drupal.org/node/3068733) Исправлена ошибка в `Drupal\workspaces\EntityOperations::entityPreload` приводящая к загрузке ненужных сущностей из статического кеша.
- [#3074949](https://www.drupal.org/node/3074949) Добавлена очистка ранее загруженных сущностей из памяти при обновлении схем БД.

## Media API

- [#3055516](https://www.drupal.org/node/3055516) Исправлена ошибка в `MediaLibraryWidget`, которая выводила сообщение об ошибке при создании нового поля где ещё не выбран `target_bundles`.
- [#3076259](https://www.drupal.org/node/3076259) Добавлена сортировка по Media ID для Media Library чтобы хэш всегда был идентичен при одинаковых ID.

## Прочие изменения

- [#3048348](https://www.drupal.org/node/3048348) Исправлена ошибка денормализации `NULL` значения для опциональных полей.
- [#3082145](https://www.drupal.org/node/3082145) Улучшена чистка после установки зависимости `twig/twig`. Теперь помимо папки `test`, также подчищается папка `tests`.
- [#2846770](https://www.drupal.org/node/2846770) Исправлена ошибка в `AdminRouteSubscriber`, при которой он помечал пути и синонимы начинающиеся с `/admin*` как административные страницы.
- [#2413191](https://www.drupal.org/node/2413191) Исправлена неполадка, при которой профиль, содержащий в себе отличный от английского язык, но не имеющий явной зависимости на locale модуль пытался установить переводы, что приводило к ошибке.
- [#3028675](https://www.drupal.org/node/3028675) Вертикальные вкладки теперь не имеют `overflow: hidden` значения на элементе содержимого.
- [#3080689](https://www.drupal.org/node/3080689) Исправлена опечатка в Nightwatch тесте `drupalCreateUser`.
- [#3082287](https://www.drupal.org/node/3082287) Улучшен возвращаемый результат для `\Drupal\user\Plugin\views\access\Role::access()`. Теперь он всегда типа `boolean`, как это и указано в документации.
- [#3081679](https://www.drupal.org/node/3081679) Исправлена документация для возвращаемого значения `AjaxResponse::getCommands`.
- [#3081080](https://www.drupal.org/node/3081080) Исправлена опечатка в `umami_theme_suggestions_block_alter()`.
- [#3078633](https://www.drupal.org/node/3078633) Исправлена ошибка в примере для `Date` рендер-элемента.
- [#2913819](https://www.drupal.org/node/2913819) Исправлена неполадка в `run-tests.sh`, при которой прежнее регулярное выражение пропускало `final` классы.
- [#3078676](https://www.drupal.org/node/3078676) Улучшено регулярное выражение в Nightwatch тесте `drupalUserIsLoggedIn`, позволяющее корректно обрабатывать https запросы.
- [#2885441](https://www.drupal.org/node/2885441) `EntityReferenceAutocompleteWidget` теперь имеет значение по умолчанию для `size` настройки типа `integer` вместо `string`, что могло приводить к ошибкам в тестах.
- [#3079444](https://www.drupal.org/node/3079444) Добавлена документация для возвращаемого типа `ExtensionList::getExtensionDiscovery`.
- [#3075933](https://www.drupal.org/node/3075933) [kim.pepper](https://www.drupal.org/u/kimpepper) был добавлен в список мейнтенеров file модуля.
- [#3076644](https://www.drupal.org/node/3076644) Исправлена и дополнена документация для `Random::image`.
- [#3021452](https://www.drupal.org/node/3021452) Добавлен `title` атрибут для oEmebed `iframe` элемента.

## Ссылки

- [Drupal 8.7.8](https://www.drupal.org/project/drupal/releases/8.7.8) (англ.), drupal.org, 2 октября 2019
