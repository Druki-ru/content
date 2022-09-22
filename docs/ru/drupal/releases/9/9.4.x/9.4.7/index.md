---
title: 'Drupal 9.4.7'
slug: wiki/drupal/releases/9.4.7
core: 9
metatags:
  title: 'Drupal 9.4.7: Список изменений'
  description: 'Список изменений Drupal 9.4.7.'
authors:
  - Niklan
category:
  area: 'Drupal 9.4.x'
  title: Drupal 9.4.7
  order: 7
---

<Aside type="warning">

Данная версия находится в разработке и не предназначена для использования.

</Aside>

## CKEditor 5

- [#3306153](https://www.drupal.org/node/3306153) CKEditor 5 обновлён до версии 35.1.0.

## Comment

- [#3166561](https://www.drupal.org/node/3166561) Исправлена неполадка, из-за которой при удалении пользователя, его комментарии могли быть удалены, вместо того чтобы быть переназначены на анонимного пользователя.

## Database

- [#3294695](https://www.drupal.org/node/3294695) Исправлена неполадка, из-за которой слой обратной совместимости драйверов баз данных мог не работать для реплик.

## Прочие изменения

- [#3309719](https://www.drupal.org/node/3309719) Исправлен некорректный тайпхинт для переменной в `EntityAutocomplete`.
- [#3060716](https://www.drupal.org/node/3060716) Исправлен некорректный `@oversDefaultClsas` 