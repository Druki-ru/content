---
title: 'Drupal 9.1.2'
slug: wiki/drupal/releases/9.1.2
core: 9
metatags:
  title: 'Drupal 9.1.2: Список изменений'
  description: 'Список изменений Drupal 9.1.2.'
authors:
  - Niklan
---

**Дата релиза**: 7 января 2020

## CKeditor

- [#3183749](https://www.drupal.org/project/drupal/issues/3183749) CKEditor обновлён до 4.15.1 содержащий исправление безопасности. Данное исправление не сказывается на Drupal.

## Content Moderation

- [#3048962](https://www.drupal.org/project/drupal/issues/3048962) Для полей состояния добавлена реализация `::generateSampleItems()` которая ничего не делает.

## CSS

- [#3117698](https://www.drupal.org/project/drupal/issues/3117698) В ядро добавлена зависимость `postcss-pxtorem`.

## Database

- [#3186999](https://www.drupal.org/project/drupal/issues/3186999) `::getServerVersion()` теперь `private`.

## Entity System

- [#3132759](https://www.drupal.org/project/drupal/issues/3132759) Ошибка об отсутствующей аннотации для конфигурационных сущностей теперь указывает о какой аннотации речь.
- [#3189174](https://www.drupal.org/project/drupal/issues/3189174) При построении запроса для поля со множественными свойствами, теперь используется свойство по умолчанию, если в запросе указано лишь название поля.

## Field UI

- [#3089525](https://www.drupal.org/project/drupal/issues/3089525) Настройки сортировки для entity reference полей теперь не показывают поля недоступные для выбранных подтипов сущностей.

## JavaScript

- [#3191164](https://www.drupal.org/project/drupal/issues/3191164) `chromedriver` обновлён до 87.0.5.

## Help Topics

- [#3065632](https://www.drupal.org/project/drupal/issues/3065632) Добавлена общая документация о том, как пользоваться Help Topics.
- [#3095734](https://www.drupal.org/project/drupal/issues/3095734) Добавлена документация по конфигурациям.
- [#3047711](https://www.drupal.org/project/drupal/issues/3047711) Документация для модулей file, image, media, media_library и responsive_image переделана в Help Topics.
- [#3073476](https://www.drupal.org/project/drupal/issues/3073476) Документация для модулей migrate, migrate_drupal, migrate_drupal_multilingual и migrate_drupal_ui переделана в Help Topics.

## Install System

- [#2625820](https://www.drupal.org/project/drupal/issues/2625820) Исправлена неполадка из-за которой функция `install_check_translations()` иногда возвращала `NULL` вместо массива.

## Layout Builder

- [#3103812](https://www.drupal.org/project/drupal/issues/3103812) Формы `ConfigureSectionForm` теперь корректно отображаются ошибки валидации.

## Menu System

- [#3018912](https://www.drupal.org/project/drupal/issues/3018912) Исправлен тип возвращаемых данных в документации `DefaultMenuLinkTreeManipulators::collectNodeLinks()`.
- [#2787107](https://www.drupal.org/project/drupal/issues/2787107) Там где возможно ссылки будут содержать в себе название маршрута.

## Migration system

- [#3151980](https://www.drupal.org/project/drupal/issues/3151980) Миграция `d7_system_mail` теперь пропускается для элементов без `mail_system`.
- [#3184545](https://www.drupal.org/project/drupal/issues/3184545) Исправлена документация о возвращаемом типе `Migration::getMigrationPluginManager()`.
- [#3151993](https://www.drupal.org/project/drupal/issues/3151993) Миграция `d7_search_settings` теперь использует плагин `variables_no_row_if_missing` вместо `variables`.
- [#3187794](https://www.drupal.org/project/drupal/issues/3187794) Удалены тесты `MigrateLookupTest::testInvalidMigrationLookup()` и `MigrateLookupTest::testErrorOnMigrationNotFound()` так как они повторяют проверка из `::testExceptionOnMigrationNotFound()` и `::testCreateStub()`.
- [#3185528](https://www.drupal.org/project/drupal/issues/3185528) Обновлены примеры для плагина `callback`.
- [#2941323](https://www.drupal.org/project/drupal/issues/2941323) Для плагина `StaticMap` добавлен пример как настроить соответствие для `NULL`.
- [#3187418](https://www.drupal.org/project/drupal/issues/3187418) Миграция `d7_system_site_translation` больше не переносит непереводимые значения.
- [#3187433](https://www.drupal.org/project/drupal/issues/3187433) Плагины источников теперь проверяют результат вызовов `parent::prepareRow()` и реагируют на `FALSE` результат. Такие плагины теперь смогут пропускать свою обработку если родительский не вернул данные.
- [#3187386](https://www.drupal.org/project/drupal/issues/3187386) Вызов исключения `PluginNotFoundException` теперь корректно подготавливает сообщение об ошибке, как для одного плагина, так и для нескольких.
- [#3187463](https://www.drupal.org/project/drupal/issues/3187463) Исправлена неполадка в плагине обработчике `d7_field_option_translation`.
- [#3187320](https://www.drupal.org/project/drupal/issues/3187320) Добавлена новая миграция `d7_user_settings`, которая мигрирует настройки пользователей из Drupal 7.
- [#3183226](https://www.drupal.org/project/drupal/issues/3183226) Плагины источников для файлов теперь задают синоним таблицы для запросов.
- [#3096676](https://www.drupal.org/project/drupal/issues/3096676) Миграции `system_maintenance` теперь используют корректную переменную.

## Olivero

- [#3183112](https://www.drupal.org/project/drupal/issues/3183112) Исправлено значение высоты с «px» на «em».
- [#3176901](https://www.drupal.org/project/drupal/issues/3176901) Изменены метки для регионов `content_below`, `footer_top`.
- [#3142857](https://www.drupal.org/project/drupal/issues/3142857) Переменная `layout` для `node--article--full.html.twig` теперь подготавливается в препроцессоре.
- [#3177918](https://www.drupal.org/project/drupal/issues/3177918) Для целостности с ядром, значения `z-index` приведены к общим значениям.

## Queue

- [#3177922](https://www.drupal.org/project/drupal/issues/3177922) Изменена сигнатура конструктора `DelayedRequeueException`. Теперь туда можно передавать привычные для исключений параметры.

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
- [#3161207](https://www.drupal.org/project/drupal/issues/3161207) Метки для фильтров теперь перерисовываются при удалении одного из них.
- [#2754985](https://www.drupal.org/project/drupal/issues/2754985) Добавлены тесты проверяющие работоспособность добавления раскрытых фильтров Views.
- [#3047216](https://www.drupal.org/project/drupal/issues/3047216) Прикрепленные (attached) отображения теперь показываются только если у пользователя есть права на их просмотр.

## Тестирование

- [#3186443](https://www.drupal.org/project/drupal/issues/3186443) Исправлена ошибка «Call to undefined method ::getAnnotations()» при использовании PHPUnit 9.5.
- [#3184493](https://www.drupal.org/project/drupal/issues/3184493) Исправлены вызовы `t()` при конкатенации строк.
- [#3184632](https://www.drupal.org/project/drupal/issues/3184632) Сравнения с xpath для отправленных данных с форм заменены на WebAssert.
- [#3177120](https://www.drupal.org/project/drupal/issues/3177120) Удалены отсылки к `WebTestBase` которого больше нет в Drupal 9.

## Прочие изменения

- [#3185917](https://www.drupal.org/project/drupal/issues/3185917) Улучшена производительность `TaggedHandlerPass`.
- [#3172757](https://www.drupal.org/project/drupal/issues/3172757) В `SessionManager::destroy()` добавлена проверка на то что вызов из CLI.
- [#3151800](https://www.drupal.org/project/drupal/issues/3151800) Улучшена документация для `DataDefinitionInterface::setInternal()`.
- [#3187730](https://www.drupal.org/project/drupal/issues/3187730) Добавлена пометка, что аргументы `--class` и `--file` скрипта `run-tests.sh` должны быть последние.
- [#2409657](https://www.drupal.org/project/drupal/issues/2409657) Форма восстановления пароля теперь перенаправляет на главную страницу вместо страницы пользователя, которая недоступна для анонимов и поэтому открывает страницу авторизации.
- [#3014121](https://www.drupal.org/project/drupal/issues/3014121) Улучшено форматирование примера для `core/lib/Drupal/Core/Template/Attribute.php`.
- [#3187239](https://www.drupal.org/project/drupal/issues/3187239) Chris Darke добавлен в список мейнтенеров
- [#3187240](https://www.drupal.org/project/drupal/issues/3187240) AmyJune Hineline (volkswagenchick) добавлена в список мейнтейнеров.
- [#3038234](https://www.drupal.org/project/drupal/issues/3038234) Из MAINTAINERS.txt удалён раздел с PHPUnit инициативой, так как теперь она считается завершённой.
- [#2917584](https://www.drupal.org/project/drupal/issues/2917584) Улучшены тесты ядра, которые проходили только если UID был равен 1.

## Ссылки

- [Drupal 9.1.2](https://www.drupal.org/project/drupal/releases/9.1.2) (англ.), drupal.org, 7 января 2021