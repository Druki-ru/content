---
title: 'Drupal 9.3.6'
slug: wiki/drupal/releases/9.3.6
core: 9
metatags:
  title: 'Drupal 9.3.6: Список изменений'
  description: 'Список изменений Drupal 9.3.6.'
authors:
  - Niklan
category:
  area: 'Drupal 9.3.x'
  title: Drupal 9.3.6 (в разработке)
  order: 6
---

<Aside type="warning" header="Разрабатываемая версия">

Данная версия находится в разработке и не предназначено для использования. Список изменений на странице не полный, могут появиться новые или отменены уже имеющиеся.

</Aside>

## CKEditor 5

- [#3248469](https://www.drupal.org/node/3248469) Оптимизирована работа CKEditor 5 в off-canvas Drupal.
- [#3249592](https://www.drupal.org/node/3249592) Добавлена поддержка указания ширины картинки в процентах для HTML 4.
- [#3231321](https://www.drupal.org/node/3231321) Улучшена навигация в редакторе при помощи клавиатуры.
- [#3250191](https://www.drupal.org/node/3250191) Исправлена неполадка, из-за которой могли некорректно работать переводы для кнопки тулбара.
- [#3264153](https://www.drupal.org/node/3264153) `CKEditor5ImageController` теперь корректно указывает путь до файла, если он уже загружается.
- [#3261942](https://www.drupal.org/node/3261942) Улучшена поддержка инлайновых ошибок в формах.

## Entity System

- [#3264050](https://www.drupal.org/node/3264050) Улучшена обработка тегов в `EntityAutocompleteController::handleAutocomplete()`.

## File System

- [#3254727](https://www.drupal.org/node/3254727) Исправлена неполадка, из-за которой URI-адрес файла не мог содержать query-параметры.

## JavaScript

- [#3262160](https://www.drupal.org/node/3262160) Внесены улучшения в `core/scripts/js/assets.js` файл, для упрощения его кода.

## Media

- [#3260554](https://www.drupal.org/node/3260554) Добавлена поддержка выравнивания для `<drupal-media>`.

## Render System

- [#3172166](https://www.drupal.org/node/3172166) Улучшена проверка в `Element::properties()`, которая приводила к «Notice: Trying to access array offset on value of type int» при попытке вызова метода с массивом в качестве аргумента, где
  ключи являлись числами.

## Theme System

- [#3262500](https://www.drupal.org/node/3262500) Функция `drupal_find_theme_functions()` помечена для внутреннего использования и будет удалена в [Drupal 10](../../../../10/index.md).

## Toolbar

- [#2949457](https://www.drupal.org/node/2949457) Улучшено кеширование ссылок в тулбаре содержащие CSRF-токены. Это позволит тулбару корректно кешировать пункты меню и увеличит производительность для пользователей имеющий доступ к данной панели.
- [#3255809](https://www.drupal.org/node/3255809) (отменено) Добавлены Nightwatch тесты для тулбара.
- [#3259807](https://www.drupal.org/node/3259807) Использование рендер элемента `toolbar_item` без заголовка больше не будет приводить к ошибкам на PHP 8.1.

## Прочие изменения

- [#3065574](https://www.drupal.org/node/3065574) Улучшена документация для `TranslatableInterface::getUntranslated()`.
- [#3166449](https://www.drupal.org/node/3166449) Улучшены описания `twig.cache` опций в сервис файле.