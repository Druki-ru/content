---
id: composer
title: Composer
---

**Composer** — пакетный менеджер для языка программирования PHP, написанный на PHP. Основная задача Composer — управление зависимостями проекта, также, как это делает npm\yarn для node.js, pip для python и др.

Зависимости от Composer в виде компонентов Symfony, появились начная с Drupal 8.0, в дальнейшем, с релиза Drupal 8.1, у Drupal появились собственные репозитории в композере и появилась возможность управлять зависимостями друпала полностью при помощи Composer.

> [!TIP]
> Не используйте на production сайтах команду `require`, изучите её аналог `install` и в чем их отличие.

## Примеры использования

Установка Drupal модуля последней стабильной версии:

```bash
composer require drupal/paragraphs
```

Пример установки Drupal модуля конкретной версии. При данном подходе, модуль обновляться не будет.

```bash
composer require drupal/paragraphs:1.8
```

Пример установки Drupal модля из dev-версии.

```bash
composer require drupal/drupal:1.0-dev
```

Пример удаления Drupal модуля с проекта.

```bash
composer remove drupal/paragraphs
```

> [!WARNING]
> Удаление зависимости через Composer, удалит файлы физически с проекта. Прежде чем выполнять команду, убедитесь, что вы удалили модуль с проекта при помощи административного интерфейса или команды `drush pmu MODULENAME`.

Пример команды обновления всех зависимостей на проекте, включая ядро, модули, и зависимости зависимостей.

```bash
composer update --with-dependencies -o
```

## Ссылки

- [Официальный сайт Composer](https://getcomposer.org/) (англ)
- [Документая по Composer](https://getcomposer.org/doc/) (англ)
- [Packagist — Основной репозиторий Composer](https://packagist.org/) (англ)
- [Drupal 8: Работа с Composer](https://niklan.net/blog/130), Niklan, 2016
