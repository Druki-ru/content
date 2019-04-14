---
id: composer-drupal-project
title: Composer Drupal Project
search-keywords:
  - Установка при помощи композера
---

**Composer Drupal Project** — шаблона проекта предоставляет стартовый набор для управления зависимостями сайта при помощи {Composer}(composer).

Данный способ установки Drupal является альтернативой установки Drupal из архива, скаченного на drupal.org.

Composer Drupal Project является официально рекомендованным способом установки как Drupal, так и всех необходимых зависимостей.

## Что предоставляет данный шаблон

Шаблон предоставляет свой composer.json файл, который берет на себя задачи:

- Ядро Drupal будет установлено в `web` директорию.
- Будет использоваться сгенерированный {Composer'ом}(composer) `vendor/autoload.php` (автолоадер зависимостей), вместо поставляемого Drupal `/web/vendor/autoload.php`.
- Модули (пакеты типа `drupal-module`) будут установлены в `web/modules/contrib`.
- Темы оформления (пакеты типа `drupal-theme`) будут установлены в `web/themes/contrib`.
- {Установочные профили}(distributions) (пакеты типа `drupal-profile`) будут установлены в `web/profiles/contrib`.
- Будут созданы со всеми необходимыми правами файлы по умолчанию `settings.php` и `services.yml`.
- Будет создана `web/sites/default/files`.
- Последняя версия {Drush}(drush) будет установлена с сайтом по пути `vendor/bin/drush`.
- Последняя версия DrupalConsole будет установлена с сайтом по пути `vendor/bin/drupal`.
- Будут созданы переменные окружения основанные на вашем `.env` файле. Для примера смотрите [.env.example](https://github.com/drupal-composer/drupal-project/blob/8.x/.env.example).

## Часто задаваемые вопросы

### Должен ли я коммитить в репозиторий проекта контриб модули которые я скачал?

Рекомендация {Composer}(composer) — **нет**. Они представляют [аргументацию против данного подхода, а также решения, если все же решите так делать](https://getcomposer.org/doc/faqs/should-i-commit-the-dependencies-in-my-vendor-directory.md).

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

Данный проект поддерживает PHP 5.6 как минимально требуемую версию для запуска, тем не менее вы можете о скачать зависимости которые потребуют PHP 7+.

Для того чтобы предотвратить это, укажите конкретную версию PHP в config разделе composer.json:

```json
"config": {
    "sort-packages": true,
    "platform": {
        "php": "5.6.40"
    }
},
```

## Ссылки

- [GitHub репозиторий проекта](https://github.com/drupal-composer/drupal-project) (англ)
- [Drupal 8: Два варианта установки ядра](https://niklan.net/blog/185), Niklan, 2018