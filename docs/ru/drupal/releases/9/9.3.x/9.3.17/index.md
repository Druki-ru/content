---
title: 'Drupal 9.3.17'
slug: wiki/drupal/releases/9.3.17
core: 9
metatags:
  title: 'Drupal 9.3.17: Список изменений'
  description: 'Список изменений Drupal 9.3.17.'
authors:
  - Niklan
category:
  area: 'Drupal 9.3.x'
  title: Drupal 9.3.17
  order: 17
---

**Дата релиза:** 22 июня 2022

## Composer

- [#3291780](https://www.drupal.org/node/3291780) Минорная версия для `guzzlehttp/psr7` увеличена до 1.9.

## CKEditor 5

- [#3273983](https://www.drupal.org/node/3273983) Плагины теперь должны явно указывать какие теги они создают. Например, указав лишь `<tag attr>` теперь не означает что плагин также поддерживает `<tag>`, если он явно не указан.
- [#3247683](https://www.drupal.org/node/3247683) По умолчанию отключены все декораторы ссылок. Для этого должны использоваться фильтры Drupal'а. Если хотя бы один подобный декоратор включён, то будет невозможно использовать плагин Drupal Media.

## Тестирование

- [#3283795](https://www.drupal.org/node/3283795) Внесены улучшения в `ComposerHookTest`.

## Прочие изменения

- [#3285193](https://www.drupal.org/node/3285193) В ряд тестов добавлен пропуск до устранения случайных фейлов в этих тестах.

## Ссылки

- [Drupal 9.3.17](https://www.drupal.org/project/drupal/releases/9.3.17) (англ.), drupal.org, 22 июня 2022