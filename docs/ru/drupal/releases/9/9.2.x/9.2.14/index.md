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
  title: Drupal 9.2.14 (в разработке)
  order: 14
---

<Aside type="warning" header="Будущая версия">

Данная версия находится в разработке: список изменений может меняться, даты могут смещаться.

</Aside>

## Quick Edit

- [#3267823](https://www.drupal.org/node/3267823) Исправлена неполадка, из-за которой тест `QuickEditIntegrationTest::testCustomBlock()` не работал на последней версии `chromedriver`.

## Тестирование

- [#3259744](https://www.drupal.org/node/3259744) Сообщение об устаревшем `Drupal\Tests\Listeners\DrupalListener` добавлено в исключения (не будет проваливать тест) для PHPUnit.
- [#3267754](https://www.drupal.org/node/3267754) Исправлена ошибка в `AjaxTest` приводящая к провалу теста.
