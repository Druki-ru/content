---
id: release-9.1.1
title: 'Drupal 9.1.1'
path: /9/releases/9.1.1
core: 9
metatags:
  title: 'Drupal 9.1.1: Список изменений'
  description: 'Список изменений Drupal 9.1.1.'
---

**Дата релиза**: 6 января 2020

> [!WARNING]
> Данный релиз находится в разработке, актуальная версия [9.1.0](release-9.1.0.md).

> [!NOTE]
> Данный релиз также содержит изменения внесённые в [Drupal 8.9.12](../../8/releases/release-8.9.12.md).

## Content Moderation

- [#3048962](https://www.drupal.org/project/drupal/issues/3048962) Для полей состояния добавлена реализация `::generateSampleItems()` которая ничего не делает.

## CSS

- [#3117698](https://www.drupal.org/project/drupal/issues/3117698) В ядро добавлена зависимость `postcss-pxtorem`.

## Database

- [#3186999](https://www.drupal.org/project/drupal/issues/3186999) `::getServerVersion()` теперь `private`.

## Entity System

- [#3132759](https://www.drupal.org/project/drupal/issues/3132759) Ошибка об отсутствующей аннотации для конфигурационных сущностей теперь указывает о какой аннотации речь.

## Field UI

- [#3089525](https://www.drupal.org/project/drupal/issues/3089525) Настройки сортировки для entity reference полей теперь не показывают поля недоступные для выбранных подтипов сущностей.

## Layout Builder

- [#3103812](https://www.drupal.org/project/drupal/issues/3103812) Формы `ConfigureSectionForm` теперь корректно отображаются ошибки валидации.

## Migration system

- [#3151980](https://www.drupal.org/project/drupal/issues/3151980) Миграция `d7_system_mail` теперь пропускается для элементов без `mail_system`.
- [#3184545](https://www.drupal.org/project/drupal/issues/3184545) Исправлена документация о возвращаемом типе `Migration::getMigrationPluginManager()`.
- [#3151993](https://www.drupal.org/project/drupal/issues/3151993) Миграция `d7_search_settings` теперь использует плагин `variables_no_row_if_missing` вместо `variables`.
- [#3187794](https://www.drupal.org/project/drupal/issues/3187794) Удалены тесты `MigrateLookupTest::testInvalidMigrationLookup()` и `MigrateLookupTest::testErrorOnMigrationNotFound()` так как они повторяют проверка из `::testExceptionOnMigrationNotFound()` и `::testCreateStub()`.
- [#3185528](https://www.drupal.org/project/drupal/issues/3185528) Обновлены примеры для плагина `callback`.
- [#2941323](https://www.drupal.org/project/drupal/issues/2941323) Для плагина `StaticMap` добавлен пример как настроить соответствие для `NULL`.
- [#3187418](https://www.drupal.org/project/drupal/issues/3187418) Миграция `d7_system_site_translation` больше не переносит непереводимые значения.

## Olivero

- [#3183112](https://www.drupal.org/project/drupal/issues/3183112) Исправлено значение высоты с «px» на «em».
- [#3176901](https://www.drupal.org/project/drupal/issues/3176901) Изменены метки для регионов `content_below`, `footer_top`.
- [#3142857](https://www.drupal.org/project/drupal/issues/3142857) Переменная `layout` для `node--article--full.html.twig` теперь подготавливается в препроцессоре.

## Seven

- [#3151119](https://www.drupal.org/project/drupal/issues/3151119) Исправлено отображение инпутов для множественных полей и узких экранов.

## System

- [#3184170](https://www.drupal.org/project/drupal/issues/3184170) Реализации интерфейса `DelayableQueueInterface` в ядре теперь отдают результаты требуемого типа.

## Theme system

- [#3120567](https://www.drupal.org/project/drupal/issues/3120567) Исправлена опечатка в описании переменной `secondary` в файлах `menu-local-tasks.html.twig`.

## User

- [#86287](https://www.drupal.org/project/drupal/issues/86287) Письмо с одноразовой ссылкой на вход теперь учитывает язык выбранный пользователем и отсылает письмо на данном языке.
- [#3109109](https://www.drupal.org/project/drupal/issues/3109109) Токен для восстановления пароля `pass-reset-token` теперь проверяется только из GET параметра.
- [#2991677](https://www.drupal.org/project/drupal/issues/2991677) `user_mail()` теперь использует корректный `$langcode`.

## Views

- [#2969107](https://www.drupal.org/project/drupal/issues/2969107) Аргументы для даты больше не возвращают HTTP 500 при некорректном формате.
- [#3069925](https://www.drupal.org/project/drupal/issues/3069925) Views теперь проверяет значение `target_bundles` для поля и если не задано, не выводит ошибку.
- [#3183106](https://www.drupal.org/project/drupal/issues/3183106) Настройки типа отображения при добавлении нового представления теперь сортируются.
- [#2628130](https://www.drupal.org/project/drupal/issues/2628130) Исправлена неполадка приводящая к SQL ошибке при экспорте ревизий.

## Тестирование

- [#3186443](https://www.drupal.org/project/drupal/issues/3186443) Исправлена ошибка «Call to undefined method ::getAnnotations()» при использовании PHPUnit 9.5.
- [#3184493](https://www.drupal.org/project/drupal/issues/3184493) Исправлены вызовы `t()` при конкатенации строк.
- [#3184632](https://www.drupal.org/project/drupal/issues/3184632) Сравнения с xpath для отправленных данных с форм заменены на WebAssert.

## Прочие изменения

- [#3185917](https://www.drupal.org/project/drupal/issues/3185917) Улучшена производительность `TaggedHandlerPass`.
- [#3172757](https://www.drupal.org/project/drupal/issues/3172757) В `SessionManager::destroy()` добавлена проверка на то что вызов из CLI.
- [#3151800](https://www.drupal.org/project/drupal/issues/3151800) Улучшена документация для `DataDefinitionInterface::setInternal()`.
- [#3187730](https://www.drupal.org/project/drupal/issues/3187730) Добавлена пометка, что аргументы `--class` и `--file` скрипта `run-tests.sh` должны быть последние.
- [#2409657](https://www.drupal.org/project/drupal/issues/2409657) Форма восстановления пароля теперь перенаправляет на главную страницу вместо страницы пользователя, которая недоступна для анонимов и поэтому открывает страницу авторизации.
