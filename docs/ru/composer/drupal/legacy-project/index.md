---
title: drupal/legacy-project
slug: drupal-legacy-project
search-keywords:
  - Установка при помощи композера
  - composer
metatags:
  description: 'Альтернативный способ установки Drupal при помощи Composer.'
---

**drupal/legacy-project** — данный шаблон создает новый сайт со структурой как она была в [Drupal 8.7.0](./release-8.7.0.md) и ранее. 

Файл "index.php" , "core" директория и т.д. расположены в корне проекте рядом с "composer.json" и "vendor" директорией. Используйте данный шаблон только если у вас нет возможности использовать рекомендуемый шаблон.

> [!NOTE]
> Рекомендуется использовать [drupal/recommended-project](../recommended-project/index.md).

**Пример структуры проекта:**

```bash
  project/
  ├─ core/
  ├─ libraries/
  ├─ modules/
  ├─ profiles/
  ├─ themes/
  ├─ vendor/
  ├─ index.php
  └─ composer.json
```

## Создание проекта с использованием данного шаблона

```bash
composer -n create-project drupal/legacy-project:^8.8@dev my_new_site 
```

## Смотрите также

- [Руководство по установке Drupal](../../../drupal/9/installation/index.md)
- [drupal/recommended-project](../recommended-project/index.md) — рекомендуемый шаблон для всех сайтов.
- [Composer](../../index.md)
- [drupal/core-recommended](../core-recommended/index.md)
- [drupal/core-composer-scaffold](../core-composer-scaffold/index.md)

## Ссылки

- [drupal/legacy-project](https://github.com/drupal/legacy-project) (англ.)
