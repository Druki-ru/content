---
title: 'Drupal 9.5.0'
slug: wiki/drupal/releases/9.5.0
core: 9
metatags:
  title: 'Drupal 9.5.0: Список изменений'
  description: 'Список изменений Drupal 9.5.0.'
authors:
  - Niklan
  - chesn0k
category:
  area: 'Drupal 9.5.x'
  title: Drupal 9.5.0
  order: 0
---

**Дата релиза**: не позднее декабря 2022

<Aside type="warning">

Данная версия находится в разработке. Список изменений может меняться и дополняться, даты могут смещаться.

</Aside>

## Метод `Connection::lastInsertId()` теперь является частью публичного Database API

- [#3260007](https://www.drupal.org/node/3260007)

Метод `Drupal\Core\Database\Connection::lastInsertId()` больше не является `@internel` для того чтобы стать публичным API. Данный метод возвращает ID последней вставленной строки или последовательного значения.

## Drupal теперь предупреждает если MySQL база данных не использует уровень изоляции `READ COMMITED`

- [#2733675](https://www.drupal.org/node/2733675)

Стандартный уровень изоляции транзакций для MySQL и MariaDB равен `REPEATABLE READ`. Эта настройка в Drupal может приводить к мёртвым блокам в таблицах, которые могут замедлить работу БД или вовсе повредить её.

Рекомендуемый уровень изоляции транзакций для Drupal `READ COMMITED`. Другие два варианта `READ UNCOMMITED` и `SERIALIZABLE` не поддерживаются ядром, используйте их на свой страх и риск.

Drupal будут генерировать предупреждение на странице `status/report` если значение будет задано неверно.

Для того чтобы активировать `READ COMMITED` для Drupal (если он не включен), необходимо обновить настройки подключения к БД в [settings.php](../../../../9/settings-php/index.md):

```php
  'init_commands' => [
    'isolation_level' => 'SET SESSION TRANSACTION ISOLATION LEVEL READ COMMITTED',
  ],
```

Полный пример:

```php
$databases['default']['default'] = [
  'database' => 'databasename',
  'username' => 'sqlusername',
  'password' => 'sqlpassword',
  'host' => 'localhost',
  'driver' => 'mysql',
  'prefix' => '',
  'port' => '3306',
  'init_commands' => [
    'isolation_level' => 'SET SESSION TRANSACTION ISOLATION LEVEL READ COMMITTED',
  ],
];
```

## CKEditor 4

- [#3271057](https://www.drupal.org/node/3271057) Интеграция Media Library с 
  CKEditor 4 перенесена в модуль CKEditor 4.

## CKEditor 5

- [#3292626](https://www.drupal.org/node/3292626) Удалён файл стилей 
  `core/modules/ckeditor5/css/quickedit.css`.
- [#3270831](https://www.drupal.org/node/3270831) Обновление с CKEditor 4 до 
  CKEditor 5 теперь возможно даже если CKEditor 4 удалён.

## Composer

- [#3280399](https://www.drupal.org/node/3280399) Пакет `drupal/core-bridge:9.5.x` объявлен заброшенным.
- [#3280589](https://www.drupal.org/node/3280589) Скрипт для чистки vendor 
  директории объявлен устаревшим.

## JavaScript

- [#3086931](https://www.drupal.org/node/3086931) Удалён файл `postcss.
  config.js`.

## Theme System

- [#3285131](https://www.drupal.org/node/3285131) Инициализация `Drupal\Core\Theme\Registry::__construct()` без передачи параметра `$runtime_cache` объявлена устаревшей.

## Tracker

- [#3267314](https://www.drupal.org/node/3267314) Добавлены тесты для миграций покрывающие удаления модуля Tracker.

## Quick Edit

- [#3267258](https://www.drupal.org/node/3267258) Функционал интеграции модуля Quick Edit с модулем Editor, перенесён непосредственно в Quick Edit.
- [#3291047](https://www.drupal.org/node/3291047) Функционал интеграции модуля CKEditor 4/5 с модулем Editor, перенесён непосредственно в Quick Edit.
- [#3291018](https://www.drupal.org/node/3291018) `CKEditor5QuickEditLibraryTest` перенесён в Quick Edit.

## Прочие изменения

- [#3279192](https://www.drupal.org/node/3279192) Внесены улучшения в `DrupalKernel::handle()` для того чтобы Drupal мог работать со Swoole.
- [#3227431](https://www.drupal.org/node/3227431) Иконка для Tabledrag API теперь адаптируется по режим `forced-colors`.
- [#3276108](https://www.drupal.org/node/3276108) Добавлена возможность указывать заголовок для блока RSS.
- [#3257485](https://www.drupal.org/node/3257485) Удалён лишний код в `TestDiscoveryTest`.
- [#3153852](https://www.drupal.org/node/3153852) `ContextAwarePluginBase` 
  объявлен устаревшим.
- [#3291283](https://www.drupal.org/node/3291283) Восстановлен файл 
  `backbone-min.js.map`.
- [#3293215](https://www.drupal.org/node/3293215) Удалены остатки от Simpletest.