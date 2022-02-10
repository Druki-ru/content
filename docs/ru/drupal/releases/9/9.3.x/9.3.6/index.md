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

## File System

- [#3254727](https://www.drupal.org/node/3254727) Исправлена неполадка, из-за которой URI-адрес файла не мог содержать query-параметры.

## Media

- [#3260554](https://www.drupal.org/node/3260554) Добавлена поддержка выравнивания для `<drupal-media>`.

## Render System

- [#3172166](https://www.drupal.org/node/3172166) Улучшена проверка в `Element::properties()`, которая приводила к «Notice: Trying to access array offset on value of type int» при попытке вызова метода с массивом в качестве аргумента, где
  ключи являлись числами.

## Theme System

- [#3262500](https://www.drupal.org/node/3262500) Функция `drupal_find_theme_functions()` помечена для внутреннего использования и будет удалена в [Drupal 10](../../../../10/index.md).

## Прочие изменения

- [#3065574](https://www.drupal.org/node/3065574) Улучшена документация для `TranslatableInterface::getUntranslated()`.