---
id: release-9.0.0
title: 'Drupal 9.0.0'
path: /9/releases/9.0.0
core: 9
metatags:
  title: 'Drupal 9.0.0: Список изменений'
  description: 'Список изменений Drupal 9.0.0.'
---

**Дата релиза**: 3 июня 2020 г.

Данный релиз является переходным с [Drupal 8](../../8/drupal-8.md) на [Drupal 9](../drupal-9.md).

**Drupal 9.0.0** API совместим с **Drupal 8.9.0**. Главное отличие между версиями — Drupal 9.0.0 не содержит код, который помечен `@deprecated` в Drupal 8.9.0.

Изменения в данной версии минимальны, по причине того, что он разрабатывается одновременно с Drupal 9 и все силы направлены на чистку устаревшего кода Drupal 8.

Новый функционал и возможности будут добавляться начиная с Drupal 9.1.0.

## Прочие изменения

- [#3089166](https://www.drupal.org/node/3089166) Минимальная версия PHP 7.2.3, рекомендуемая — 7.3.
- [#3088712](https://www.drupal.org/node/3088712) Drupal ядро теперь запрашивает Symfony 4.4.
- [#3089508](https://www.drupal.org/node/3089508) Полифилы (html5shiv, matchMEdia, domready, classList) — удалены.
- [#3095199](https://www.drupal.org/node/3095199) Twig проапгрейжен до 2.х версии.
- [#2955931](https://www.drupal.org/node/2955931) Зависимость `easyrdf/easyrdf` перенесена в `require-dev`.
- [#3096454](https://www.drupal.org/node/3096454) Функция `twig_without()` — удалена.
