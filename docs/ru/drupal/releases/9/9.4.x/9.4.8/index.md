---
title: 'Drupal 9.4.8'
slug: wiki/drupal/releases/9.4.8
core: 9
metatags:
  title: 'Drupal 9.4.8: Список изменений'
  description: 'Список изменений Drupal 9.4.8.'
authors:
  - Niklan
category:
  area: 'Drupal 9.4.x'
  title: Drupal 9.4.8
  order: 8
---

<Aside type="warning">

Данная версия находится в разработке и не предназначена для использования.

</Aside>

## CKEditor 5

- [#3306153](https://www.drupal.org/node/3306153) CKEditor 5 обновлён до версии 35.1.0.
- [#3268306](https://www.drupal.org/node/3268306) Исправлена неполадка, из-за которой могли не сохранятся
  собственные HTML-теги, например: `<drupal-media>`, `<drupal-entity>`, `<foobar>`.
- [#3270734](https://www.drupal.org/node/3270734) Тесты модулей Editor и CKEditor 5 больше не используют CKEditor 4.

## Comment

- [#3166561](https://www.drupal.org/node/3166561) Исправлена неполадка, из-за которой при удалении пользователя, его комментарии могли быть удалены, вместо того чтобы быть переназначены на анонимного пользователя.

## Config System

- [#3312001](https://www.drupal.org/node/3312001) Исправлено состояние гонки в `ConfigImporter`.

## Database

- [#3294695](https://www.drupal.org/node/3294695) Исправлена неполадка, из-за которой слой обратной совместимости драйверов баз данных мог не работать для реплик.

## Прочие изменения

- [#3309719](https://www.drupal.org/node/3309719) Исправлен некорректный тайпхинт для переменной в `EntityAutocomplete`.
- [#3060716](https://www.drupal.org/node/3060716) Исправлен некорректный `@oversDefaultClsas`.
- [#3311476](https://www.drupal.org/node/3311476) Тест `DownloadTest` будет пропускаться на SQLite базах данных, так как сейчас это приводит к блокировке БД.