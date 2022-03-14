---
title: 'Drupal 9.2.14'
slug: wiki/drupal/releases/9.2.14
core: 9
metatags:
  title: 'Drupal 9.2.14: Список изменений'
  description: 'Список изменений Drupal 9.2.14.'
authors:
  - Niklan
category:
  area: 'Drupal 9.2.x'
  title: Drupal 9.2.14
  order: 14
---

## Layout Builder

- [#3267124](https://www.drupal.org/node/3267124) Некоторые тесты временно отключены от выполнения из-за ошибок.

## Quick Edit

- [#3267823](https://www.drupal.org/node/3267823) Исправлена неполадка, из-за которой тест `QuickEditIntegrationTest::testCustomBlock()` не работал на последней версии `chromedriver`.

## Тестирование

- [#3259744](https://www.drupal.org/node/3259744) Сообщение об устаревшем `Drupal\Tests\Listeners\DrupalListener` добавлено в исключения (не будет проваливать тест) для PHPUnit.
- [#3267754](https://www.drupal.org/node/3267754) Исправлена ошибка в `AjaxTest` приводящая к провалу теста.
- [#3268070](https://www.drupal.org/node/3268070) Расширен список тестов которые временно пропускаются.

## Ссылки

- [Drupal 9.2.14](https://www.drupal.org/project/drupal/releases/9.2.14) (англ.), drupal.org, 11 марта 2022
