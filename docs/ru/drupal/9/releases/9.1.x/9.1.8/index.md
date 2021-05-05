---
title: 'Drupal 9.1.8'
slug: 9/releases/9.1.8
core: 9
metatags:
  title: 'Drupal 9.1.8: Список изменений'
  description: 'Список изменений Drupal 9.1.8.'
---

**Дата релиза:** 5 мая 2021

## Aggregator

* [#3207654](https://www.drupal.org/project/drupal/issues/3207654) Добавлен вызов `::accessCheck()` для упущенных EntityQuery.

## Comment

* [#3208265](https://www.drupal.org/project/drupal/issues/3208265) `comment_user_predelete()` больше не проверяет права доступа.
* [#2496913](https://www.drupal.org/project/drupal/issues/2496913) При добавлении нового типа комментариев, теперь нельзя выбрать сущности у которых нет возможности подключать поля или ID не является числом.

## Database Update System

* [#1624278](https://www.drupal.org/project/drupal/issues/1624278) Улучшена очистка комментариев в `update_get_update_list()`.
* [#3210502](https://www.drupal.org/project/drupal/issues/3210502) `UpdateDescriptionTest` конвертирован в Kernel Test.

## Entity System

* [#2927500](https://www.drupal.org/project/drupal/issues/2927500) `EntityFieldManager::buildFieldStorageDefinitions()` теперь также передаёт название поля и id типа сущности.
* [#3208222](https://www.drupal.org/project/drupal/issues/3208222) `Tables::addNextBaseTable()` теперь использует квадратные скобки для SQL.

## Install System

* [#3207893](https://www.drupal.org/project/drupal/issues/3207893) В процессе установки теперь генерируется значение `system.css_js_query_string`.

## Layout Builder

* [#3208267](https://www.drupal.org/project/drupal/issues/3208267) `InlineBlockEntityOperations` больше не проверяет права доступа.

## Locale

* [#3199428](https://www.drupal.org/project/drupal/issues/3199428) Из `LocaleTranslationTest` удалены тесты внутреннего поведения `DependencySerializationTrait`.

## Mail System

* [#2414019](https://www.drupal.org/project/drupal/issues/2414019) `mb_strtoupper()` больше не используется для заголовков, так как приводит к проблемам со спам фильтрами.

## Menu

* [#2488302](https://www.drupal.org/project/drupal/issues/2488302) Улучшено сообщение об успешном сохранении перевода без изменений.

## Migration System

* [#3206939](https://www.drupal.org/project/drupal/issues/3206939) Добавлена отсутствующая документация для некоторых плагинов источников миграций из Drupal.
* [#2944846](https://www.drupal.org/project/drupal/issues/2944846) Улучшено описание ключевых концепций в `migrate.api.php`.

## Render System

* [#3186821](https://www.drupal.org/project/drupal/issues/3186821) Добавлены тесты, что HTML элементы `span` и `button` не имеют аттрибута `hreflang`.

## System

* [#3202434](https://www.drupal.org/project/drupal/issues/3202434) Исправлено описание для плагина условия `request_path`.

## Umami

* [#3199284](https://www.drupal.org/project/drupal/issues/3199284) Из `umami.libraries.yml` удалено подключение несуществующего `css/components/regions/page-title/page-title.css`.

## User

* [#3182970](https://www.drupal.org/project/drupal/issues/3182970) `user.logout` теперь доступен только авторизованным пользователям.

## Views

* [#2823914](https://www.drupal.org/project/drupal/issues/2823914) Views теперь сам добавляет кеш ключи на основе аргументов.

## Views UI

* [#3136107](https://www.drupal.org/project/drupal/issues/3136107) Исправлена неполадка приводящая к удалению обёртки разметки при переопределении полей.

## Тестирование

* [#3208225](https://www.drupal.org/project/drupal/issues/3208225) Из `FieldHelpTest` удален бесполезный код.
* [#3209048](https://www.drupal.org/project/drupal/issues/3209048) Темы из ядра теперь добавляются в автозагрузчик для тестирования.
* [#3203476](https://www.drupal.org/project/drupal/issues/3203476) Сравнения с использованием xpath на `div` конвертированы в WebAssert формат.
* [#3211164](https://www.drupal.org/project/drupal/issues/3211164) Внесены улучшения в `WebDriverTestBase`.

## Прочие изменения

* [#3207308](https://www.drupal.org/project/drupal/issues/3207308) Исправлены ошибки стандартов кодирования `Generic.Formatting.DisallowMultipleStatements`.
* [#3123058](https://www.drupal.org/project/drupal/issues/3123058) Исправлены ошибки стандартов кодирования `Drupal.Commenting.DocComment.ParamGroup`.
* [#3205026](https://www.drupal.org/project/drupal/issues/3205026) В файле `core/lib/Drupal/Core/KeyValueStore/DatabaseStorage.php` добавлен недостающий `use`.
* [#3204763](https://www.drupal.org/project/drupal/issues/3204763) Исправлены некорректные вызовы `sprintf()`.
* [#3210694](https://www.drupal.org/project/drupal/issues/3210694) Слова с интернационализированными префиксами теперь игнорируется проверкой правописания.
* [#3205037](https://www.drupal.org/project/drupal/issues/3205037) Из теста `Drupal\Tests\Component\Annotation\PluginIdTest` удалено тестирование несуществующего конструктора.