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

## Views

* [#3253568](https://www.drupal.org/node/3253568) Исправлена неполадка, которая приводила к ошибке при использовании шаблона поля.

## Прочие изменения

* [#3239287](https://www.drupal.org/node/3239287) Внесены улучшения в `\Drupal\Core\Extension\ModuleDependencyMessageTrait` для совместимости с PHP 8.1.