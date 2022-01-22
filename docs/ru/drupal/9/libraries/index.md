---
title: Библиотеки
slug: wiki/9/libraries
core: 9
search-keywords:
 - где как подключать JS JavaScript CSS добавить на страницу
 - куда вставить JS CSS скрипт
metatags:
  title: 'Drupal 9: Библиотеки'
  description: 'Подключаем JavaScript и CSS в Drupal 9 правильно!'
category:
  area: Библиотеки
  title: Введение
  order: 1
authors:
  - Niklan
---

В Drupal 9 CSS и JS загружаются по принципу ассетов. Библиотека может содержать один и более CSS файлов, один и более JS файлов, а также JS настройки.

Drupal загружает только те библиотеки на странице, которые были запрошены в момент её построения. Таким образом, Drupal не загружает все существующие библиотеки на одной странице, так как это вредит производительности.

## Ссылки

- [Drupal 8: Libraries API (Добавление CSS/JS на страницы)](https://niklan.net/blog/72), Niklan, 2015
