---
title: drupal/recommended-project
slug: drupal-recommended-project
search-keywords:
  - Установка Drupal при помощи композера
  - composer
metatags:
  description: 'Composer шаблон с ядром Drupal.'
---

**drupal/recommended-project** — рекомендуемый шаблон для создания новых сайтов на Drupal, при котором корень проекта находится на уровень выше.

Это означает, что файл "index.php", папка "core" и т.д. расположены в директории "web", вместо того чтобы располагаться рядом с "composer.json" и "vendor" директорией в корне проекта. Данная структура рекомендуется, потому что она позволяет настроить веб-сервер так, что он будет предоставлять доступ только к директории "web". Папка "vendor" и подобные будут находиться за пределами корня "web" директории, что положительно сказывается на безопасности.

**Пример структуры проекта:**

```bash
  project/
  ├─ web/
  │  ├─ core/
  │  ├─ libraries/
  │  ├─ modules/
  │  ├─ profiles/
  │  ├─ themes/
  │  └─ index.php
  ├─ vendor/
  └─ composer.json
```

## Создание проекта с использованием данного шаблона

```bash
composer -n create-project drupal/recommended-project my_new_site 
```

## Смотрите также

- [Руководство по установке Drupal](../../../drupal/9/installation/index.md)
- [drupal/legacy-project](../legacy-project/index.md) — альтернативный шаблон проекта, структура которого использовалась до релиза [Drupal 8.8.0](../../../drupal/8/releases/8.8.x/8.8.0/index.md).
- [Composer](../../index.md)
- [drupal/core-recommended](../core-recommended/index.md)
- [drupal/core-composer-scaffold](../core-composer-scaffold/index.md)

## Ссылки

- [drupal/recommended-project](https://github.com/drupal/recommended-project) (англ.)
