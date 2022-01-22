---
title: 'Drupal 9.3.1'
slug: wiki/drupal/releases/9.3.1
core: 9 
metatags:
  title: 'Drupal 9.3.1: Список изменений'
  description: 'Список изменений Drupal 9.3.1.'
authors:
  - Niklan
---

**Дата релиза**: 5 января 2022

## Composer

* [#3253683](https://www.drupal.org/node/3253683) Улучшена совместимость с [Composer](../../../../../composer/index.md) 2.2.

## Database System

* [#3250648](https://www.drupal.org/node/3250648) Внесены улучшения в `SelectSubqueryTest` для избежания сравнений бинарных значений.
* [#3136388](https://www.drupal.org/node/3136388) Улучшена документация для `core/lib/Drupal/Core/Database/Install/Tasks.php`.

## Field System

* [#3064890](https://www.drupal.org/node/3064890) Улучшена проверка на наличие ключа  `name` в `FieldUiTable`.
* [#2916142](https://www.drupal.org/node/2916142) Улучшена генерация «тестовых значений» для `decimal` и `float` типов полей.

## JavaScript

* [#3255504](https://www.drupal.org/node/3255504) Из `date.js` (библиотека `core/drupal.date`) удалена зависимость jQuery.

## Link

* [#2879293](https://www.drupal.org/node/2879293) В `LinkWidget` внесены изменения, что если для «текста ссылки» стоит обязательный параметр, то URI ссылки принудительно становится обязательным.

## Media Library

* [#3190261](https://www.drupal.org/node/3190261) Исправлена неполадка, из-за которой виджет Media Library мог приводить к AJAX ошибке.

## Migration System

* [#3253824](https://www.drupal.org/node/3253824) Исправлена опечатка в комментарии файла `d7_field_formatter_settings.yml`.
* [#3247039](https://www.drupal.org/node/3247039) Методу `MigrateDestinationInterface::import()` добавлена информацию том, что он может выбросить исключение `MigrateException`.
* [#3092430](https://www.drupal.org/node/3092430) Исправлена неполадка, приводящая к ошибке «TypeError: Argument 1 passed to Drupal\search\Plugin\ConfigurableSearchPluginBase::setConfiguration() must be of the type array, null given».
* [#2675006](https://www.drupal.org/node/2675006) Добавлен тест для трейта `MigrationConfigurationTrait`.
* [#3251835](https://www.drupal.org/node/3251835) Исправлена некорректная документация для `Row::getSource()`.
* [#3014629](https://www.drupal.org/node/3014629) Некоторым миграциям из Drupal 6 и Drupal 7 добавлены комментарии, что они могут зависеть от миграций содержимого. 

## Olivero

* [#3247269](https://www.drupal.org/node/3247269) Исправлена неполадка, из-за которой выравнивание индикации наведения и выпадающих меню было некорректно.

## Stable Theme

* [#3212470](https://www.drupal.org/node/3212470) Исправлен некорректный селектор для псевдо-элементов `placeholder` в Off Canvas элементах.

## System

* [#3254403](https://www.drupal.org/node/3254403) Оптимизировано обновление `system_post_update_sort_all_config()`, которое могло приводить к проблемам нехватки памяти в процессе выполнения.
* [#3248309](https://www.drupal.org/node/3248309) `AssertBreadcrumbTrait` теперь не опирается на разметку предоставляемую темой Classy.

## Update

* [#3252067](https://www.drupal.org/node/3252067) Уменьшено количество повторных вызовов в тестах модуля.
* [#3205909](https://www.drupal.org/node/3205909) Из `UpdateSemverTestBase` удалены права доступа, что не требуются всем тестам, что расширяют базовый тест. Удалённые права доступа добавлены непосредственно в те тесты, где они требуются.

## User System

* [#3253889](https://www.drupal.org/node/3253889) Исправлена неполадка, которая приводила к ошибке переадресации при авторизации с использованием query-параметра `?check_logged_in=1`.

## Views

* [#3253568](https://www.drupal.org/node/3253568) Исправлена неполадка, которая приводила к ошибке при использовании шаблона поля.
* [#2793169](https://www.drupal.org/node/2793169) Обновлена документация для `hook_views_post_render()`.
* [#3247619](https://www.drupal.org/node/3247619) Исправлена неполадка, из-за которой не работала опция «Показывать двоеточие после метки» для сгруппированного поля.

## Тестирование

* [#3251125](https://www.drupal.org/node/3251125) `InstallerExistingConfigTestBase` теперь не будет деинсталировать модуль, который предоставлять драйвер баз данных.
* [#3245383](https://www.drupal.org/node/3245383) Модули, что предоставляю драйвера баз данных, теперь проверяются на то что они активны в момент запуска тестов.
* [#3131348](https://www.drupal.org/node/3131348) Вызовы с использованием `empty()` заменены на соответствующие `::assertEmpty()`, `::assertNotEmpty()` и `::assertArrayNotHasKey()`.
* [#3207907](https://www.drupal.org/node/3207907) В функциональные тесты, где не используется сборщик писем, внесены улучшения для его использования.

## Прочие изменения

* [#3239287](https://www.drupal.org/node/3239287) Внесены улучшения в `\Drupal\Core\Extension\ModuleDependencyMessageTrait` для совместимости с PHP 8.1.
* [#3246157](https://www.drupal.org/node/3246157) [Chris Drake](https://www.drupal.org/u/chrisdarke) добавлен в качестве координатора направления наставничества.
* [#3246158](https://www.drupal.org/node/3246158) [AmyJune Hineline](https://www.drupal.org/u/volkswagenchick) добавлена в качестве координатора направления наставничества.
* [#3246156](https://www.drupal.org/node/3246156) [Brian Gilber](https://www.drupal.org/u/realityloop) добавлен в качестве координатора направления наставничества.
* [#3080819](https://www.drupal.org/node/3080819) В `InfoParserInterface` добавлена документация о параметре `core_version_requirements`.
* [#3249859](https://www.drupal.org/node/3249859) Исправлен некорректный пример в документации для `NestedArray::unsetValue()`.
* [#3174570](https://www.drupal.org/node/3174570) Исправлена некорректная документация для свойства `MainContentViewSubscriber::$classResolver`.
* [#3175287](https://www.drupal.org/node/3175287) Удалено повторение слов в комментариях кода.
* [#2853183](https://www.drupal.org/node/2853183) Улучшены отсылки на Symfony Framework.
* [#3256581](https://www.drupal.org/node/3256581) Внесены улучшения в документацию `update.authorize.inc`.
* [#3213928](https://www.drupal.org/node/3213928) Внесены улучшения в документацию `LoggerChannelInterface`.
* [#3256591](https://www.drupal.org/node/3256591) Внесено исправление в документацию для `table` рендер-элемента.

## Ссылки

- [Drupal 9.3.1](https://www.drupal.org/project/drupal/releases/9.3.1) (англ.), drupal.org, 5 января 2022
