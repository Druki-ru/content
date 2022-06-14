---
title: 'Drupal 9.5.0'
slug: wiki/drupal/releases/9.5.0
core: 9
metatags:
title: 'Drupal 9.5.0: Список изменений'
description: 'Список изменений Drupal 9.5.0.'
authors:
- Niklan
- chesn0k
  category:
  area: 'Drupal 9.5.x'
  title: Drupal 9.5.0
  order: 0
---

**Дата релиза**: не позднее декабря 2022

<Aside type="warning">

Данная версия находится в разработке. Список изменений может меняться и дополняться, даты могут смещаться.

</Aside>

## Метод `Connection::lastInsertId()` теперь является частью публичного Database API

- [#3260007](https://www.drupal.org/node/3260007)

Метод `Drupal\Core\Database\Connection::lastInsertId()` больше не является `@internel` для того чтобы стать публичным API. Данный метод возвращает ID последней вставленной строки или последовательного значения.

## `drupal/core-recommended` теперь позволяет устанавливать обновления патч-версий

- [#3198340](https://www.drupal.org/node/3198340)

Метапакет `drupal/core-recommended` теперь позволяет устанавливать обновления патч-версий. Это означает что владельцы сайтов, использующие `drupal/core-recommended`, теперь могут обновления зависимостей ядра при помощи Composer самостоятельно, не дожидаясь официального релиза Drupal с обновлёнными версиями.

## Composer

- [#3280399](https://www.drupal.org/node/3280399) Пакет `drupal/core-bridge:9.5.x` объявлен заброшенным. 

## Прочие изменения

- [#3279192](https://www.drupal.org/node/3279192) Внесены улучшения в `DrupalKernel::handle()` для того чтобы Drupal мог работать со Swoole.
- [#3227431](https://www.drupal.org/node/3227431) Иконка для Tabledrag API теперь адаптируется по режим `forced-colors`.
- [#3276108](https://www.drupal.org/node/3276108) Добавлена возможность указывать заголовок для блока RSS.
- [#3257485](https://www.drupal.org/node/3257485) Удалён лишний код в `TestDiscoveryTest`.