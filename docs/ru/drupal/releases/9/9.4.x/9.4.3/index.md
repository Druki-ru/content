---
title: 'Drupal 9.4.3'
slug: wiki/drupal/releases/9.4.3
core: 9
metatags:
  title: 'Drupal 9.4.3: Список изменений'
  description: 'Список изменений Drupal 9.4.3.'
authors:
  - Niklan
category:
  area: 'Drupal 9.4.x'
  title: Drupal 9.4.3
  order: 3
---

<Aside type="warning">

Данная версия находится в разработке и не предназначена для использования на 
проектах.

</Aside>

## Block Content

- [#2859197](https://www.drupal.org/node/2859197) В 
  `BlockContentViewBuilder` добавлено уточнение, что Блок сущности не 
  предназначены для вывода за пределами блоков.

## CKEditor 4

- [#3291744](https://www.drupal.org/node/3291744) Внесены улучшения в
  сохраняемые конфигурации редактора. Теперь они хранят только информацию
  только об активных плагинах.

## CKEditor 5

- [#3246246](https://www.drupal.org/node/3246246) Внесены улучшения в 
  документацию в CKEditor 5 API.

## Composer

- [#3292380](https://www.drupal.org/node/3292380) Из `composer.json` 
  удалена секция `replace`.

## Content Translation

- [#2994000](https://www.drupal.org/node/2994000) Улучшено сообщение об 
  ошибке при попытке добавить перевод для заблокированного языка.

## Field UI

- [#3121325](https://www.drupal.org/node/3121325) Исправлена неполадка в 
  `FieldUiTestTrait`, которая могла приводить к случайным ошибкам в 
  JavaScript тестах.

## Filter

- [#3268983](https://www.drupal.org/node/3268983) Исправлена 
  неполадка, из-за которой использование `*` совместно с названием аттрибута 
  (`attribute*`) приводило к исключению.

## Locale

- [#3266123](https://www.drupal.org/node/3266123) Исправлена неполадка с 
  отображением фокусировки на элементах административных страниц.

## Media

- [#3294883](https://www.drupal.org/node/3294883) Исправлена ошибка в 
  комментарии метода `\Drupal\media\Entity\Media::prepareSave()`.

## Menu UI

- [#3207771](https://www.drupal.org/node/3207771) Исправлена документация 
  для функции `menu_ui_form_node_type_form_alter()`.

## MySQL

- [#3291949](https://www.drupal.org/node/3291949) Улучшено предупреждение 
  для проверки уровня изоляции транзакций MySQL.

## Taxonomy

- [#2494617](https://www.drupal.org/node/2494617) Исправлена неполадка в 
  валидаторе аргумента Views `taxonomy_term_name`. 
- [#3293996](https://www.drupal.org/node/3293996) Исправлен пример в 
  документации функции `taxonomy_term_load_multiple_by_name()`.

## User

- [#3137119](https://www.drupal.org/node/3137119) Метод 
  `User::setExistingPassword()` теперь возвращает `$this`.
- [#3270564](https://www.drupal.org/node/3270564) `data` из Drupal 7 теперь 
  корректно передаётся в миграцию.

## Views

- [#2916682](https://www.drupal.org/node/2916682) Исправлена неполадка, 
  из-за которой значения по умолчанию не работали для сгруппированных фильтров.

## Прочие изменения

- [#3284665](https://www.drupal.org/node/3284665) Исправлены опечатки в `@see`.
- [#3293122](https://www.drupal.org/node/3293122) В документацию 
  `TwigExtension::getPath()` добавлено что параметр `$parameters` опциональный.
- [#3295421](https://www.drupal.org/node/3295421) `TranslationWrapper`, 
  который объявлен устаревшим, будет удалён в Drupal 11 вместо Drupal 10..
- [#3291887](https://www.drupal.org/node/3291887) Удалена устаревшая отсылка 
  к функции `form_type_checkboxes_value()`.