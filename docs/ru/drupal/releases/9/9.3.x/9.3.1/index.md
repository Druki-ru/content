---
title: 'Drupal 9.3.1'
slug: wiki/drupal/releases/9.3.1
core: 9 
metatags:
  title: 'Drupal 9.3.1: Список изменений'
  description: 'Список изменений Drupal 9.3.1.'
---

> [!WARNING]
> Данная версия находится в разработке.

## Composer

* [#3253683](https://www.drupal.org/node/3253683) Улучшена совместимость с [Composer](../../../../../composer/index.md) 2.2.

## Database System

* [#3250648](https://www.drupal.org/node/3250648) Внесены улучшения в `SelectSubqueryTest` для избежания сравнений бинарных значений.

## Migration System

* [#3253824](https://www.drupal.org/node/3253824) Исправлена опечатка в комментарии файла `d7_field_formatter_settings.yml`.

## Update

* [#3252067](https://www.drupal.org/node/3252067) Уменьшено количество повторных вызовов в тестах модуля.

## User System

* [#3253889](https://www.drupal.org/node/3253889) Исправлена неполадка, которая приводила к ошибке переадресации при авторизации с использованием query-параметра `?check_logged_in=1`.

## Views

* [#3253568](https://www.drupal.org/node/3253568) Исправлена неполадка, которая приводила к ошибке при использовании шаблона поля.

## Прочие изменения

* [#3239287](https://www.drupal.org/node/3239287) Внесены улучшения в `\Drupal\Core\Extension\ModuleDependencyMessageTrait` для совместимости с PHP 8.1.
* [#3246157](https://www.drupal.org/node/3246157) [Chris Drake](https://www.drupal.org/u/chrisdarke) добавлен в качестве координатора направления наставничества.
* [#3246158](https://www.drupal.org/node/3246158) [AmyJune Hineline](https://www.drupal.org/u/volkswagenchick) добавлена в качестве координатора направления наставничества.
* [#3246156](https://www.drupal.org/node/3246156) [Brian Gilber](https://www.drupal.org/u/realityloop) добавлен в качестве координатора направления наставничества.