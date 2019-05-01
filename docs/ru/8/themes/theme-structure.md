---
id: theme-structure
core: 8
title: Структура темы
path: /docs/8/themes/structure
metatags:
  title: 'Темы оформления Drupal 8: Структура темы'
  description: 'Из каких папок может состоять тема оформления и что в них должно находиться.'
search-keywords:
  - как создать свою тему
  - где верстать
  - где держать верстку
  - куда ложить верстку
  - как поменять оформление сайта
category-area: 'Темы оформления'
category-title: 'Структура темы'
category-order: 2
---

Каждая тема имеет свою собственную папку соответствующую названию темы, например **my_theme_name/**. А в данной папке находятся различные файлы и другие папки, необходимые для данной темы оформления.

Давайте посмотрим на пример структуры темы:

```
  |-my_theme_name.breakpoints.yml
  |-my_theme_name.info.yml
  |-my_theme_name.libraries.yml
  |-my_theme_name.theme
  |-config
  |  |-install
  |  |  |-my_theme_name.settings.yml
  |  |-schema
  |  |  |-my_theme_name.schema.yml
  |-css
  |  |-style.css
  |-js
  |  |-my-custom-script.js
  |-images
  |  |-buttons.png
  |-logo.svg
  |-screenshot.png
  |-templates
  |  |-maintenance-page.html.twig
  |  |-node.html.twig
```

Ниже представлено описание общих файлов, которые можно найти в теме оформления.

## *.info.yml

Тема оформления должна содержать файл описывающий данную тему для Drupal. Это единственный обязательный файл для тем оформления. С его структурой и примерами можете ознакомиться в разделе {создания собственной темы оформления}(create-theme:8).

## *.libraries.yml

Файл .libraries.yml используется для объявления ассетов темы оформления, которые могут быть как JavaScript, так и CSS файла. В теме объявляются библиотеки, необходимые для данной темы оформления. Ознакомьтесь с информацией по работе с {библиотеками}(libraries-api:8).

## Ссылки

- [Drupal 8 Theme folder structure](https://www.drupal.org/docs/8/theming-drupal-8/drupal-8-theme-folder-structure) (англ), Drupal.org.