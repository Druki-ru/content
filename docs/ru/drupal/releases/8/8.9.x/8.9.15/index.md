---
title: 'Drupal 8.9.15'
slug: wiki/drupal/releases/8.9.15
core: 8
metatags:
  title: 'Drupal 8.9.15: Список изменений'
  description: 'Список изменений Drupal 8.9.15.'
---

**Дата релиза:** 5 мая 2021

## Composer

* [#3211805](https://www.drupal.org/project/drupal/issues/3211805) Обновлены метапакеты для `composer/composer`. 

## Cron System

- [#3201470](https://www.drupal.org/project/drupal/issues/3201470) Реализации `hook_cron()` с использованием `EntityQuery` теперь явно указывают что права доступа проверять не нужно.

## CSS

* [#2958588](https://www.drupal.org/project/drupal/issues/2958588) Исправлены неполадки в стилях для «Off canvas» элемента приводящие к некорректному отображению.

## Editor

- [#2857444](https://www.drupal.org/project/drupal/issues/2857444) Улучшено отслеживание файлов в полях отличных от `text`, `text_long` и `text_with_summary`. Теперь данные файлы отслеживаются для всех полей что расширяют `TextItemBase`.

## Field UI

* [#3202440](https://www.drupal.org/project/drupal/issues/3202440) `FieldStorageConfigEditForm::validateCardinality()` больше не учитывает права доступа.

## Form System

- [#3183301](https://www.drupal.org/project/drupal/issues/3183301) Добавлены тесты покрывающий [SA-CORE-2020-009](../../../../security/sa-core/2020-009/index.md).

## JavaScript

* [#3212177](https://www.drupal.org/project/drupal/issues/3212177) `caniuse-lite` обновлён до актуальной версии.
* [#3211810](https://www.drupal.org/project/drupal/issues/3211810) Обновлена зависимость `nightwatch`, а также зависимости с обновлениями безопасности.
* [#3209456](https://www.drupal.org/project/drupal/issues/3209456) Underscore.js обновлён до версии 1.13.1.

## Locale

- [#3128389](https://www.drupal.org/project/drupal/issues/3128389) Для `LocaleTranslation` добавлен `DependencySerializationTrait` чтобы он корректно сериализовался.

## Media System

- [#3192260](https://www.drupal.org/project/drupal/issues/3192260) Исправлена настройка DrupalCI для `CKEditorIntegrationTest` который мог случайно проваливаться.

## Migration System

[#3184650](https://www.drupal.org/project/drupal/issues/3184650) В миграцию ContentEntity добавлена возможность указывать ключ источника в котором хранится ID ревизии. Для более подробного описания изменения смотрите [список изменений Drupal 9.1.5](../../../9/9.1.x/9.1.5/index.md).

## Routing System

* [#3120301](https://www.drupal.org/project/drupal/issues/3120301) Маршруты созданные JSON:API более не предзагружаются.

## Serialization

- [#3054510](https://www.drupal.org/project/drupal/issues/3054510) Исправлена документация для `NormalizerBase::supportsDenormalization()`.

## Transliteration System

- [#3169212](https://www.drupal.org/project/drupal/issues/3169212) Улучшена транслитерация Украинских символов.

## User

* [#3206540](https://www.drupal.org/project/drupal/issues/3206540) `user_is_blocked()` больше не учитывает права доступа.

## Views

- [#2969107](https://www.drupal.org/project/drupal/issues/2969107) Аргументы для даты больше не возвращают HTTP 500 при некорректном формате.
- [#3201393](https://www.drupal.org/project/drupal/issues/3201393) Стандартное представления глоссария больше не показывает неопубликованные материалы.

## Тестирование

* [#3207086](https://www.drupal.org/project/drupal/issues/3207086) Исправлены проблемы с тестом `MonthDatePluginTest`.
- [#2571475](https://www.drupal.org/project/drupal/issues/2571475) `KernelTestBase` теперь могу производить внешние HTTP запросы.
* [#3211164](https://www.drupal.org/project/drupal/issues/3211164) Внесены улучшения в `WebDriverTestBase`.

## Прочие изменения

- [#3192231](https://www.drupal.org/project/drupal/issues/3192231) Исправлена работа теста `UnroutedUrlTest` на dev версиях PHP.
- [#3199205](https://www.drupal.org/project/drupal/issues/3199205) `Archive_Tar` обновлён до 1.4.13.

## Ссылки

- [Drupal 8.9.15](https://www.drupal.org/project/drupal/releases/8.9.15) (англ.), drupal.org, 5 мая 2021