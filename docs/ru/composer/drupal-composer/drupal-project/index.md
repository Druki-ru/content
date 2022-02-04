---
title: drupal-composer/drupal-project
slug: wiki/drupal-composer-drupal-project
search-keywords:
  - Установка при помощи композера
  - composer
metatags:
  description: 'Composer шаблон с Drupal ядром, Drush и Drupal Console.'
authors:
  - Niklan
---

**Composer Drupal Project** — шаблон проекта предоставляет стартовый набор для управления зависимостями сайта при помощи [Composer](../../index.md).

Данный способ установки Drupal является альтернативой установки Drupal из архива, скаченного на drupal.org.

<Aside>

Начиная с [Drupal 8.8.0](../../../drupal/releases/8/8.8.x/8.8.0/index.md) предоставляются официальные шаблоны.

Рекомендуем ознакомиться с [Drupal Recommended Project](../../drupal/recommended-project/index.md) в качестве замены или альтернативы.

</Aside>


## Что предоставляет данный шаблон

Шаблон предоставляет свой composer.json файл, который берет на себя задачи:

- Ядро Drupal будет установлено в `web` директорию.
- Будет использоваться сгенерированный [Composer'ом](../../index.md) `vendor/autoload.php` (автолоадер зависимостей), вместо поставляемого Drupal `/web/vendor/autoload.php`.
- Модули (пакеты типа `drupal-module`) будут установлены в `web/modules/contrib`.
- Темы оформления (пакеты типа `drupal-theme`) будут установлены в `web/themes/contrib`.
- [Установочные профили](../../../drupal/8/distributions/index.md) (пакеты типа `drupal-profile`) будут установлены в `web/profiles/contrib`.
- Будут созданы со всеми необходимыми правами файлы по умолчанию [settings.php](../../../drupal/8/settings-php/index.md) и `services.yml`.
- Будет создана `web/sites/default/files`.
- Последняя версия [Drush](../../../drush/index.md) будет установлена с сайтом по пути `vendor/bin/drush`.
- Последняя версия DrupalConsole будет установлена с сайтом по пути `vendor/bin/drupal`.
- Будут созданы переменные окружения основанные на вашем `.env` файле. Для примера смотрите [.env.example](https://github.com/drupal-composer/drupal-project/blob/8.x/.env.example).

## Часто задаваемые вопросы

### Должен ли я коммитить в репозиторий проекта контриб модули которые я скачал?

Рекомендация [Composer](../../index.md) — **нет**. Они представляют [аргументацию против данного подхода, а также решения, если все же решите так делать](https://getcomposer.org/doc/faqs/should-i-commit-the-dependencies-in-my-vendor-directory.md).

### Должен ли я коммитить в репозиторий проекта скафолд файлы?

Плагин [drupal-scaffold](https://github.com/drupal-composer/drupal-scaffold) (поставляется с данным проектом) может загружать скафолд-файлы (например index.php, update.php, robots.txt и т.д. - те что лежать не в папке core в легаси варианте с архивом) в web папку вашего проекта. Если вы их не модифицируете, рекомендуется исключить данные файлы из вашего репозитория проекта. Если вам необходимо чтобы данный плагин запускался автоматически после каждой установки или обновления проекта, вы можете зарегистрировать `@composer drupal:scaffold` команду на post-install и post-update события в вашем composer.json

```json
"scripts": {
    "post-install-cmd": [
        "@composer drupal:scaffold",
        "..."
    ],
    "post-update-cmd": [
        "@composer drupal:scaffold",
        "..."
    ]
},
```

### Как я могу применять патчи на загружаемые модули?

Если вам необходимо применять патчи, вы можете это сделать при помощи плагина [composer-patches](https://github.com/cweagans/composer-patches) (поставляется с шаблоном).

Для того чтобы добавить патч для Drupal модуля "foobar", добавьте секцию "patches" в раздел "extra" вашего composer.json файла:

```json
"extra": {
    "patches": {
        "drupal/foobar": {
            "Patch description": "URL or local path to patch"
        }
    }
}
```

### Как мне указать версию PHP?

Данный проект поддерживает PHP 5.6 как минимально требуемую версию для запуска, тем не менее вы можете скачать зависимости которые потребуют PHP 7+.

Для того чтобы предотвратить это, укажите конкретную версию PHP в config разделе composer.json:

```json
"config": {
    "sort-packages": true,
    "platform": {
        "php": "5.6.40"
    }
},
```

## Смотрите также

- [Руководство по установке Drupal](../../../drupal/8/installation/index.md)
- [drupal/recommended-project](../../drupal/recommended-project/index.md) — рекомендуемый шаблон для всех сайтов.
- [drupal/legacy-project](../../drupal/legacy-project/index.md) — альтернативный шаблон проекта, структура которого использовалась до релиза [Drupal 8.8.0](../../../drupal/releases/8/8.8.x/8.8.0/index.md).
- [Composer](../../index.md)

## Ссылки

- [GitHub репозиторий проекта](https://github.com/drupal-composer/drupal-project) (англ.)
- [Drupal 8: Два варианта установки ядра](https://niklan.net/blog/185), Niklan, 2018
- [Миграция с drupal-composer/drupal-project на drupal/recommended-project](https://niklan.net/blog/209), Niklan, 2020