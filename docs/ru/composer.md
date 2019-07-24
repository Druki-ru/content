---
id: composer
title: Composer
metatags:
  description: 'Узнайте что такое Composer и как им пользоваться в Drupal.'
---

**Composer** — пакетный менеджер для языка программирования PHP, написанный на PHP. Основная задача Composer — управление зависимостями проекта, также, как это делает npm\yarn для node.js, pip для python и др.

Зависимости от Composer в виде компонентов Symfony, появились начная с Drupal 8.0, в дальнейшем, с релиза Drupal 8.1, у Drupal появились собственные репозитории в композере и появилась возможность управлять зависимостями друпала полностью при помощи Composer.

> [!TIP]
> Не используйте на production сайтах команду `require`, изучите её аналог `install` и в чем их отличие.

## Примеры использования

### Установка зависимостей

Установка Drupal проекта последней стабильной версии:

```bash
composer require drupal/paragraphs
```

Установка Drupal проекта конкретной версии. При данном подходе, модуль обновляться не будет.

```bash
composer require drupal/paragraphs:1.8
```

Пример установки Drupal проекта из dev-версии. При данном подходе, зависимость будет обновляться только в пределах dev версии.

```bash
composer require drupal/drupal:1.0-dev
```

Пример установки Drupal проекта из dev-версии, но с блокировкой на определенный коммит. Данный пакет будет установлен из гит репозитория проекта на определенный коммит. Вы можете указать hash ID коммита целиком, либо первые 7 символов.

```bash
composer require drupal/drupal:1.0-dev#be6a91c
```

### Обновление зависимостей

Пример команды обновления всех зависимостей на проекте, включая ядро, модули, и зависимости зависимостей.

```bash
composer update --with-dependencies -o
```

Пример обновления конкретной зависимости.

```bash
composer update drupal/paragraphs
```

Приме обновления конкретной зависимости, а также зависимостей указанных для него.

```bash
composer update drupal/paragraphs --with-dependencies -o
```

### Удаление зависимостей

Пример удаления Drupal проекта.

```bash
composer remove drupal/paragraphs
```

> [!WARNING]
> Удаление зависимости через Composer, удалит файлы физически с проекта. Прежде чем выполнять команду, убедитесь, что вы удалили модуль с проекта при помощи административного интерфейса или команды `drush pmu MODULENAME`.

## Патчинг зависимостей

Иногда в модулях для Drupal, или в самом ядре, обнаруживают ошибки. Для исправления ошибок готовят патчи, которые в дальнейшем попадут в dev релиз, а затем в стабильный.

Для применения патчей, необходимо чтобы на проекте была установлена зависимость `cweagans/composer-patches`. После чего, вы можете указывать патчи в разделе "extra.patches".

```bash
{
    ...
    "extra": {
        ...
        "patches": {
          "drupal/core": {
            "Add startup configuration for PHP server": "https://www.drupal.org/files/issues/add_a_startup-1543858-30.patch",
            "Patch from local file": "./patches/fix.patch"
          }
        }
    },
}
```

## См. также

- [Drush — командная утилита для управления Drupal](drush.md)

## Ссылки

- [Composer](https://getcomposer.org/) (англ.)
- [Composer Documentation](https://getcomposer.org/doc/) (англ.)
- [Packagist](https://packagist.org/) (англ.)
- [Drupal 8: Работа с Composer](https://niklan.net/blog/130), Niklan, 2016
