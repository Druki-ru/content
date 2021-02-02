---
id: drupal-core-vendor-hardening
title: drupal/core-vendor-hardening
search-keywords:
  - composer
metatags:
  description: 'Плагин Composer для чистки пакетов.'
---

**drupal/core-vendor-hardening** — [Composer](composer.md) плагин предназначенный для чистки загруженных пакетов.

## Для чего данный плагин?

Данный плагин позволяет удалять директории из загруженных пакетов. Как правило, это необходимо, для того чтобы удалить потенциально опасные и исполняемые файлы из загруженных пакетов, которые не требуются для работы.

Также данный плагин создаёт `.htaccess` и `web.config` файлы с корректными настройками, чтобы вендоры не были доступны из сети.

## Настройка плагина

По умолчанию, данный плагин знает о всех пакетах необходимых для Drupal ядра и файлов, что могут быть безопасно удалены. Поэтому, достаточно запросить данный пакет чтобы он начал чистить данные зависимости. Данный список директорий можно найти в файле `Config.php` данного пакета.

### Удаление директорий

Если вы запрашиваете сторонние пакеты самостоятельно, вы можете указать плагину какие директории необходимо удалять:

```json
    "extra": {
      "drupal-core-vendor-hardening": {
        "vendor/package": ["test", "documentation"]
      }
    }
```

## Смотрите также

- [drupal/core-composer-scaffold](drupal-core-composer-scaffold.md)

## Ссылки

- [drupal/core-vendor-hardening](https://github.com/drupal/core-vendor-hardening), Github