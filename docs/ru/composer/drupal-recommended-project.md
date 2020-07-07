---
id: drupal-recommended-project
title: drupal/recommended-project
search-keywords:
  - Установка при помощи композера
  - composer
metatags:
  description: 'Composer шаблон с ядром Drupal.'
---

**drupal/recommended-project** — рекомендуемый шаблон для создания новых сайтов на Drupal, при котором корень проекта находится на уровень выше.

Это означает, что файл "index.php", папка "core" и т.д. расположены в директории "web", вместо того чтобы располагаться рядом с "composer.json" и "vendor" директорией в корне проекта. Данная структура рекомендуется, потому что она позволяет настроиться веб-сервер, что он будет предоставлять доступ только к "web" директории. Папка "vendor" и подобные будут находиться за пределами корня "web" директории, что положительно для безопасности.

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

- [Руководство по установке Drupal](../8/installation.md)
- [drupal/legacy-project](drupal-legacy-project.md) — альтернативный шаблон проекта, структура которого использовалась до релиза [Drupal 8.8.0](../8/releases/release-8.8.0.md).
- [Composer](composer.md)
- [drupal/core-recommended](drupal-core-recommended.md)
- [drupal/core-composer-scaffold](drupal-core-composer-scaffold.md)

## Ссылки

- [drupal/recommended-project](https://github.com/drupal/recommended-project) (англ.)
