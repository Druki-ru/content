---
title: 'Drupal 9.3.5'
slug: wiki/drupal/releases/9.3.5
core: 9
metatags:
  title: 'Drupal 9.3.5: Список изменений'
  description: 'Список изменений Drupal 9.3.5.'
authors:
  - Niklan
category:
  area: 'Drupal 9.3.x'
  title: Drupal 9.3.5
  order: 5
---

**Дата релиза**: 5 февраля 2022

## Обзор

Drupal использует шаблонизатор Twig, для которого вышло обновление безопасности. В этом патч-релизе для Drupal была обновлена зависимость `twig/twig`.

В Twig 2.12 была добавлена возможность передавать функцию для сортировки в фильтр `sort`. Эту возможность можно использовать для вызова PHP функций (например `eval`). В исправленной версии `sort` может принимать только анонимные функции, чтобы предотвратить подобные вызовы.

Данная особенность не затрагивает Drupal, в связи с чем этот патч-релиз не является обновлением безопасности.

## Composer

* [#3262583](https://www.drupal.org/node/3262583) Зависимость `twig/twig` обновлена до версии 2.14.11.

## Ссылки

- [Drupal 9.3.5](https://www.drupal.org/project/drupal/releases/9.3.5) (англ.), drupal.org, 5 февраля 2022
- [Twig Security Release: Disallow Non Closures In The Sort Filter](https://www.symfony-news.com/news/details/twig-security-release-disallow-non-closures-in-the-sort-filter) (англ.), 5 февраля 2022