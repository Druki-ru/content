---
title: 'Drupal 9.2.1'
slug: 9/releases/9.2.1
core: 9
metatags:
  title: 'Drupal 9.2.1: Список изменений'
  description: 'Список изменений Drupal 9.2.1.'
---

**Дата релиза**: 7 июля 2021

## Database System

* [#3216556](https://www.drupal.org/project/drupal/issues/3216556) Добавлено уточнение для `Connection::select()` что в качестве аргумента для параметра `$table` можно передать подзапрос.

## File System

* [#2228087](https://www.drupal.org/project/drupal/issues/2228087) В интерфейс `PhpStreamWrapperInterface` добавлена документация к методам, а классы, реализующие его теперь ссылаются на его документацию.

## Help

* [#3067727](https://www.drupal.org/project/drupal/issues/3067727) Документация для модулей `comment`, `node`, `path` и `taxonomy` была конвертирована из `hook_help()` в Help Topic.
* [#3094482](https://www.drupal.org/project/drupal/issues/3094482) Документация для модуля `action` была конвертирована из `hook_help()` в Help Topic.
* [#3095739](https://www.drupal.org/project/drupal/issues/3095739) Документация для модулей `contextual`, `help`, `inline_form_errors`, `quickedit`, `settings_tray`, `shortcut`, `toolbar`  и `tour` была конвертирована из `hook_help()` в Help Topic.

## Help Topic

* [#3218660](https://www.drupal.org/project/drupal/issues/3218660) Исправлена неполадка приводящая к ошибке на сайте после удаления модуля.

## JSON:API

* [#3220184](https://www.drupal.org/project/drupal/issues/3220184) [Björn Brala (bbrala)](https://www.drupal.org/u/bbrala) добавлен в список мейнтенеров JSON:API.

## Layout Builder

* [#3218766](https://www.drupal.org/project/drupal/issues/3218766) `SystemMainBlock` больше нельзя добавлять в регионы так как это приводит к фатальным ошибкам и не предусмотрено блоком.

## Link

* [#3202166](https://www.drupal.org/project/drupal/issues/3202166) Виджет для ссылок теперь позволяет сохранять ссылки с `:route<button>`.

## Media System

* [#3097416](https://www.drupal.org/project/drupal/issues/3097416) При встраивании медиа при помощи CKEditor, теперь пользователь может выбрать только тот режим отображения, которые включены для данного типа медиа.
* [#3220450](https://www.drupal.org/project/drupal/issues/3220450) Исправлена неполадка в `ProviderRepositoryTest` приводящая к провалу теста.

## Menu System

* [#3219881](https://www.drupal.org/project/drupal/issues/3219881) Исправлена опечатка в описании для `MenuLinkContentAccessControlHandler`.

## Migration System

* [#3213621](https://www.drupal.org/project/drupal/issues/3213621) Исправлена фикстура для миграций Drupal 7.
* [#3053167](https://www.drupal.org/project/drupal/issues/3053167) Мультиязычные свойства `migrate_drupal.migrate_drupal.yml` были вынесены в соответствующие миграции.
* [#3164520](https://www.drupal.org/project/drupal/issues/3164520) `FieldableEntity::getFieldValues()` теперь гарантирует что значения поля будут отсортированы по дельте хранения. Ранее, отсутствие данной сортировки, могло приводить к миграции значений поле в обратном порядке.
* [#3209353](https://www.drupal.org/project/drupal/issues/3209353) Добавлена отсутствующая документация к плагинам источникам предоставляемые модулями `node` и `taxonomy`.
* [#3196583](https://www.drupal.org/project/drupal/issues/3196583) `MigrationLookup` теперь может сам определить ID источников в перечисленных миграциях.
* [#3103031](https://www.drupal.org/project/drupal/issues/3103031) В плагин источник `FieldOptionTranslation` значение `bundle` используется как ID источника.
* [#3199741](https://www.drupal.org/project/drupal/issues/3199741) Добавлена отсутствующая документация к плагинам источника предоставляемые модулями `action`, `aggregator`, `book`, `migrate_drupal`, `comment`, `field`, `filter`, `image`, `path`, `rdf`, `search`, `shortcut`, `system` и `tracker`.

## Node System

* [#3048848](https://www.drupal.org/project/drupal/issues/3048848) Блок `SyndicateBlock` теперь генерирует корректный URL для `rss.xml` с использованием `Url::fromUri()`, а не хардкод значение.

## Olivero

* [#3210199](https://www.drupal.org/project/drupal/issues/3210199) Улучшено отображение сайдбар региона.
* [#3173022](https://www.drupal.org/project/drupal/issues/3173022) Улучшена разметка для меню, выводимого в сайдбаре.
* [#3173008](https://www.drupal.org/project/drupal/issues/3173008) CSS селектор для изображения статьи заменён на универсальный `.wide-image`.
* [#3208372](https://www.drupal.org/project/drupal/issues/3208372) Произведён рефакторинг `comments.es6.js`.
* [#3217175](https://www.drupal.org/project/drupal/issues/3217175) Добавлена поддержка закрытия вложенного меню при помощи клавиши Escape для IE 11.
* [#3212981](https://www.drupal.org/project/drupal/issues/3212981) Произведён рефакторинг `navigation.es6.js` для соответствия на Drupal JavaScript Coding Standards.
* [#3213074](https://www.drupal.org/project/drupal/issues/3213074) Произведён рефакторинг `second-level-navigation.es6.js` для соответствия на Drupal JavaScript Coding Standards.
* [#3212073](https://www.drupal.org/project/drupal/issues/3212073) Кнопка открытия основного меню теперь вертикальн отцентровано на мобильных устройствах.
* [#3211907](https://www.drupal.org/project/drupal/issues/3211907) Метка для меток материала теперь выравнена с самими метками.
* [#3213118](https://www.drupal.org/project/drupal/issues/3213118) Убран лишний отступ в мобильной навигации между пунктами меню у которых есть вложенные пункты.
* [#3210902](https://www.drupal.org/project/drupal/issues/3210902) Исправлена неполадка из-за которой цитаты могли "наезжать" на сайдбар.
* [#3211889](https://www.drupal.org/project/drupal/issues/3211889) Исправлена неполадка генерации стилей с использованием CSS Grid для IE11.
* [#3173010](https://www.drupal.org/project/drupal/issues/3173010) Добавлены два новых цвета `--color--gray-5` и `--color--gray-8`.
* [#3212702](https://www.drupal.org/project/drupal/issues/3212702) Исправлена неполадка отображения некорректно выравненых картинки профиля и комментария на IE11.
* [#3212700](https://www.drupal.org/project/drupal/issues/3212700) Исправлена неполадка отображения состояния фокусировки на вкладке, которое было без правой рамки на IE11.
* [#3211613](https://www.drupal.org/project/drupal/issues/3211613) Исправлена неполадка отображения состояния фокусировки на кнопке закрытия системных сообщений, которое было несимметричным.
* [#3173832](https://www.drupal.org/project/drupal/issues/3173832) Документация в JavaScript файлах приведена в соотствитие Drupal JavaScript Coding Standards.

## Umami

* [#3213957](https://www.drupal.org/project/drupal/issues/3213957) Исправлены оформление кнопок при наведении в Quick Edit.

## Update System

* [#1478294](https://www.drupal.org/project/drupal/issues/1478294) Фикстуры для Update Manager теперь содержат ссылки на [релизы Drupal 8](../../../../8/releases/index.md), а не Drupal 7.

## Views

* [#3195178](https://www.drupal.org/project/drupal/issues/3195178) Исправлена неполадка приводящая к SQL ошибкам на некоторых движках БД при использовании сортировки с distinct по результатам.
* [#3222009](https://www.drupal.org/project/drupal/issues/3222009) Исправлена документация для `hook_views_query_alter()` в которой тайпхинт для параметра `$query` был без указания пространства имён.

## Тестирование

* [#3213734](https://www.drupal.org/project/drupal/issues/3213734) В `AssertButtonsTrait` исправлен некорректный синтаксис.
* [#2879159](https://www.drupal.org/project/drupal/issues/2879159) Исправлены вызовы `::assertEquals()` с некорректным порядком передачи аргументов.
* [#3156396](https://www.drupal.org/project/drupal/issues/3156396) Для сравнений для подсчёта размера и количества теперь используется `::assertSameSize()`.
* [#3215143](https://www.drupal.org/project/drupal/issues/3215143) Комментарии ссылающиеся на несуществующий `::assertEqual()` были обновлены.
* [#3175718](https://www.drupal.org/project/drupal/issues/3175718) Исправлена неполадка из-за которой проверка на `theme_token` могла случайным образом проваливаться. Это связано с тем что `\Behat\Mink\Element\DocumentElement::getText()` возвращает текст очищенный от script тегов внутри `<body>` элемента, которые используются Drupal для передачи Drupal Settings.
* [#3217374](https://www.drupal.org/project/drupal/issues/3217374) При указании неверной схемы для значения `SIMPLETEST_BASE_URL` теперь выбрасывается исключение.
* [#3220183](https://www.drupal.org/project/drupal/issues/3220183) Использование сравнений с `xpath` заменено на методы WebAssert.
* [#3217717](https://www.drupal.org/project/drupal/issues/3217717) Использование устаревшего `::at()` заменено на соответствующие `::once()`, `::exactly()` или `::any()`.

## Прочие изменения

* [#2719649](https://www.drupal.org/project/drupal/issues/2719649) Внесены исправления в код для исправления ошибок стандарта `Drupal.Commenting.InlineComment.SpacingBefore`.
* [#3220922](https://www.drupal.org/project/drupal/issues/3220922) [Gabe Sullice (gabesullice)](https://www.drupal.org/u/gabesullice) удалён из списка мейнтенеров для Decoupled Menu.

## Ссылки

- [Drupal 9.2.1](https://www.drupal.org/project/drupal/releases/9.2.1) (англ.), drupal.org, 7 июля 2021