---
title: 'Drupal 9.0.5'
slug: wiki/drupal/releases/9.0.5
core: 9
metatags:
  title: 'Drupal 9.0.5: Список изменений'
  description: 'Список изменений Drupal 9.0.5.'
authors:
  - Niklan
---

**Дата релиза**: 4 сентября 2020

2 сентября 2020, Symfony анонсировали обновление безопасности для компонента `symfony/http-client`. Также Symfony выпустили обновление совместимости для `symfony/http-kernel` компонента.

Drupal не подвержен данной уязвимости, так как не использует `symfony/http-client`. Некоторые пользователи Drupal сообщают, что их автоматические системы проверки безопасности пометили `symfony/http-client` на обновление, что привело к проблемам с  CI. Для упрощения решения проблемы данный релиз обновляет зависимость Drupal `symfony/http-kernel` до версии 3.4.44.

## Прочие изменения

- Изменение [#3070375](https://www.drupal.org/project/drupal/issues/3070375) из [Drupal 8.9.4](release-8.9.4.md) было откачено для доработки, так как привело к [проблемам с модальными окнами модуля Paragraphs](https://www.drupal.org/project/paragraphs/issues/3168733).

## Ссылки

- [CVE-2020-15094](https://symfony.com/blog/cve-2020-15094-prevent-rce-when-calling-untrusted-remote-with-cachinghttpclient), (англ.), symfony.com, 2 сентября.
- [Drupal 9.0.5](https://www.drupal.org/project/drupal/releases/9.0.5) (англ.), drupal.org, 4 сентября 2020