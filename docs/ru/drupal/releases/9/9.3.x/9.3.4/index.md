---
title: 'Drupal 9.3.4'
slug: wiki/drupal/releases/9.3.4
core: 9 
metatags:
  title: 'Drupal 9.3.4: Список изменений'
  description: 'Список изменений Drupal 9.3.4.'
---

> [!WARNING]
> Данная версия находится в разработке и не предназначена для использования на живых сайтах.

## CKEditor 5

* [#3255077](https://www.drupal.org/node/3255077) Исправлены опечатки в описании для CKEditor 5 фильтра.
* [#3228778](https://www.drupal.org/node/3228778) Плагины для CKEditor 5 теперь могут использовать `Drupal.t()` для перевода строк.
* [#3238257](https://www.drupal.org/node/3238257) Если CKEditor заменил `<textarea>` на который вёл якорь, то теперь CKEditor перехыватает такой якорь и перенаправляет фокусировку на себя.

## File System

* [#3254553](https://www.drupal.org/node/3254553) Исправлена неполадка, из-за которой `FileUrlGenerator::generate()` не работал со Stream Wrappers ведущие на внешние ресурсы.

## JavaScript

* [#3258371](https://www.drupal.org/node/3258371) Исправлена неполадка в команде `yarn vendor-update`.

## Render System

* [#3254328](https://www.drupal.org/node/3254328) Кеш-контексты и кеш-теги заполнителей (`placeholder`) теперь сортируются, прежде будут переданы на дальнейшую обработку. Это исправляет ошибку, из-за которой рендер некоторых блоков мог производиться дважды при использовании ленивых построителей.

## Прочие изменения

* [#2612876](https://www.drupal.org/node/2612876) Исправлены ошибки в документации к `\Drupal\Core\Asset\CssOptimizer::processFile()`.
* [#3257654](https://www.drupal.org/node/3257654) Внесены изменения для исправления ошибок выявленных PHPStan L0.
* [#3106216](https://www.drupal.org/node/3106216) Из ядра удалены неиспользуемые переменные.
* [#3255245](https://www.drupal.org/node/3255245) Изменение [#3231683](https://www.drupal.org/node/3231683) из [Drupal 9.3.0](../9.3.0/index.md) было откачено.