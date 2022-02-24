---
title: 'Drupal 9.3.7'
slug: wiki/drupal/releases/9.3.7
core: 9
metatags:
  title: 'Drupal 9.3.7: Список изменений'
  description: 'Список изменений Drupal 9.3.7.'
authors:
  - Niklan
category:
  area: 'Drupal 9.3.x'
  title: Drupal 9.3.7 (в разработке)
  order: 7
---

<Aside type="warning" header="Разрабатываемая версия">

Данная версия находится в разработке и не предназначено для использования. Список изменений на странице не полный, могут появиться новые или отменены уже имеющиеся.

</Aside>

## Добавлено новое условие для CKEditor 5 плагинов — `requiresConfiguration`

- [#3224652](https://www.drupal.org/node/3224652) Добавлена поддержка ресайза изображений.

Для плагинов CKEditor 5 добавлено новое условие — `requiresConfiguration`.

Данное условие позволяет задать требование, при котором плагин активируется принудительно, если определённая настройка редактора имеет конкретной значение.

**Например:**

```yaml
    conditions:
      requiresConfiguration:
        allow_resize: true
      plugins: [ckeditor5_image]
```

Это означает, что плагин, у которого объявлена такая конструкция, активируется если включён плагин `ckeditor5_image`, а его настройка `allow_resize` имеет значение `true`.

## CKEditor 5

- [#3248469](https://www.drupal.org/node/3248469) Оптимизирована работа CKEditor 5 в off-canvas Drupal.
- [#3249592](https://www.drupal.org/node/3249592) Добавлена поддержка указания ширины картинки в процентах для HTML 4.
- [#3231321](https://www.drupal.org/node/3231321) Улучшена навигация в редакторе при помощи клавиатуры.
- [#3250191](https://www.drupal.org/node/3250191) Исправлена неполадка, из-за которой могли некорректно работать переводы для кнопки тулбара.
- [#3264153](https://www.drupal.org/node/3264153) `CKEditor5ImageController` теперь корректно указывает путь до файла, если он уже загружается.
- [#3261942](https://www.drupal.org/node/3261942) Улучшена поддержка инлайновых ошибок в формах.
- [#3264451](https://www.drupal.org/node/3264451) Исправлена ошибка в селекторе для теста `ImageTest`.
- [#3228334](https://www.drupal.org/node/3228334) `HTMLRestrictionsUtilities` был переделан `HTMLRestrictions`.
- [#3262492](https://www.drupal.org/node/3262492) `isMediaUrl` переделан на более общий API, который позволяет передавать информацию от любого Media.
- [#3228464](https://www.drupal.org/node/3228464) Добавлен API для загрузки переводов доступный сторонним модулям.
- [#3265230](https://www.drupal.org/node/3265230) Внесены множественные улучшения в `ie11.filter.warnings.es6.js`.
- [#3248440](https://www.drupal.org/node/3248440) Исправлена неполадка, из-за которой аттрибуты встроенного элемента могли оказаться на обёртке, отображающей превью.
- [#3259493](https://www.drupal.org/node/3259493) Добавлены новые ограничения для HTML внутри редактора. Например, больше нельзя использовать аттрибут `style` в целях безопасности.
- [#3246385](https://www.drupal.org/node/3246385) Добавлена поддержка подписей для `<drupal-media>`.
- [#3264512](https://www.drupal.org/node/3264512) Ассеты для CKEditor 5 теперь будут аггрегироваться.

## Claro

- [#3261049](https://www.drupal.org/node/3261049) Удалены дублирующие свойства `margin` из `typography.pcss.css`.
- [#3248239](https://www.drupal.org/node/3248239) Улучшено визуальное отображение подсказок от модуля Tour.
- [#3123811](https://www.drupal.org/node/3123811) Улучшена контрастность для серого цвета, для соответствия A11Y.
- [#3213556](https://www.drupal.org/node/3213556) Исправлена неполадка, из-за которой описание поле могло быть невидимым в диалоговом окне Layout Builder.

## Entity System

- [#3264050](https://www.drupal.org/node/3264050) Улучшена обработка тегов в `EntityAutocompleteController::handleAutocomplete()`.
- [#2821009](https://www.drupal.org/node/2821009) Улучшено описание для метода `EntityAccessCheck::access()`.
- [#3261538](https://www.drupal.org/node/3261538) Исправлена неполадка, из-за которой могло выбрасываться исключение `BundleClassInheritanceException` даже если значение не указано.

## File System

- [#3254727](https://www.drupal.org/node/3254727) Исправлена неполадка, из-за которой URI-адрес файла не мог содержать query-параметры.

## JavaScript

- [#3262160](https://www.drupal.org/node/3262160) Внесены улучшения в `core/scripts/js/assets.js` файл, для упрощения его кода.

## JSON:API

- [#3154962](https://www.drupal.org/node/3154962) Исправлена неполадка, из-за которой могло быть отказано в доступе на загрузку файла, если у поля не указано к какому бандлу она принадлежит.

## Media

- [#3260554](https://www.drupal.org/node/3260554) Добавлена поддержка выравнивания для `<drupal-media>`.
- [#3255887](https://www.drupal.org/node/3255887) (отменено) В конструктор `MediaThumbnailFormatter` добавлен аргумент `$file_url_generator`.

## Render System

- [#3172166](https://www.drupal.org/node/3172166) Улучшена проверка в `Element::properties()`, которая приводила к «Notice: Trying to access array offset on value of type int» при попытке вызова метода с массивом в качестве аргумента, где
  ключи являлись числами.

## Theme System

- [#3262500](https://www.drupal.org/node/3262500) Функция `drupal_find_theme_functions()` помечена для внутреннего использования и будет удалена в [Drupal 10](../../../../10/index.md).

## Toolbar

- [#2949457](https://www.drupal.org/node/2949457) Улучшено кеширование ссылок в тулбаре содержащие CSRF-токены. Это позволит тулбару корректно кешировать пункты меню и увеличит производительность для пользователей имеющий доступ к данной панели.
- [#3255809](https://www.drupal.org/node/3255809) Добавлены Nightwatch тесты для тулбара.
- [#3259807](https://www.drupal.org/node/3259807) Использование рендер элемента `toolbar_item` без заголовка больше не будет приводить к ошибкам на PHP 8.1.

## User

- [#3265802](https://www.drupal.org/node/3265802) Обновление `user_update_9301()` больше не запускается на Microsoft SQL Server, так как приводит к потере данных и сломанному сайту.
- [#3255419](https://www.drupal.org/node/3255419) Исправлена неполадка, из-за которой обновление до Drupal 9.3 приводило к ошибке если MySQL переменная `sql_require_primary_key` задана как `ON`.

## Views

- [#2730227](https://www.drupal.org/node/2730227) Улучшена документация для `hook_field_views_data()`.

## Тестирование

- [#3263886](https://www.drupal.org/node/3263886) Добавлены фикстуры с Drupal 9.3.0 из [Drupal 10](../../../../10/index.md).
- [#2861376](https://www.drupal.org/node/2861376) Внесены улучшения в `ToolkitGdTest`, в котором было некорректное использование `::checkRequirements()`.
- [#3221507](https://www.drupal.org/node/3221507) Исправлено состояние гонки в `ClassWriter`.
- [#3265376](https://www.drupal.org/node/3265376) Внесены улучшения в `UpdateScriptTest`, который мог проваливаться из-за `MINIMUM_SUPPORTED_PHP`.
- [#3265378](https://www.drupal.org/node/3265378) Внесены улучшения в `NoPreExistingSchemaUpdateTest`, который мог проваливаться из-за `MINIMUM_SUPPORTED_PHP`.
- [#3265377](https://www.drupal.org/node/3265377) Внесены улучшения в `LocaleTranslatedSchemaDefinitionTest`, который мог проваливаться из-за `MINIMUM_SUPPORTED_PHP`.
- [#3265291](https://www.drupal.org/node/3265291) В тесте `QuickStartTest` переработано ожидание результата процесса.

## Прочие изменения

- [#3065574](https://www.drupal.org/node/3065574) Улучшена документация для `TranslatableInterface::getUntranslated()`.
- [#3166449](https://www.drupal.org/node/3166449) Улучшены описания `twig.cache` опций в сервис файле.
- [#3265419](https://www.drupal.org/node/3265419) Улучшено сообщение об устаревшем классе `RequestStack`.
- [#3264862](https://www.drupal.org/node/3264862) Классу `ContextAwarePluginBase` добавлена информации о [депрекации](../../../../../deprecation/index.md).