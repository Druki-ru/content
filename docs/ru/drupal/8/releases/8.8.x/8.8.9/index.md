---
title: 'Drupal 8.8.9'
slug: 8/releases/8.8.9
core: 8
metatags:
  title: 'Drupal 8.8.9: Список изменений'
  description: 'Список изменений Drupal 8.8.9.'
---

**Дата релиза**: 4 сентября 2020

2 сентября 2020, Symfony анонсировали обновление безопасности для компонента `symfony/http-client`. Также Symfony выпустили обновление совместимости для `symfony/http-kernel` компонента.

Drupal не подвержен данной уязвимости, так как не использует `symfony/http-client`. Некоторые пользователи Drupal сообщают, что их автоматические системы проверки безопасности пометили `symfony/http-client` на обновление, что привело к проблемам с  CI. Для упрощения решения проблемы данный релиз обновляет зависимость Drupal `symfony/http-kernel` до версии 3.4.44.

## Ссылки

- [CVE-2020-15094](https://symfony.com/blog/cve-2020-15094-prevent-rce-when-calling-untrusted-remote-with-cachinghttpclient), (англ.), symfony.com, 2 сентября.
- [Drupal 8.8.9](https://www.drupal.org/project/drupal/releases/8.8.9) (англ.), drupal.org, 4 сентября 2020