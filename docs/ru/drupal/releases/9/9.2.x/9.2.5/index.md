---
title: 'Drupal 9.2.5'
slug: drupal/releases/9.2.5
core: 9
metatags:
  title: 'Drupal 9.2.5: Список изменений'
  description: 'Список изменений Drupal 9.2.5.'
---

> [!WARNING]
> Данная версия находится в разработке и не предназначена для использования.

## CKEditor

* [#3227945](https://www.drupal.org/node/3227945) Из процесса сборки CKEditor удален `bender-runner.config.json`.

## Database System

* [#3228237](https://www.drupal.org/node/3228237) `Drupal\Core\Command\DbDumpCommand::getTables()` теперь сортирует таблицы по алфавиту.

## Media System

* [#3063343](https://www.drupal.org/node/3063343) `MediaLibraryState` теперь реализует `CacheableDependencyInterface`.
* [#3222616](https://www.drupal.org/node/3222616) Улучшено регулярное выражение для YouTube, которое теперь позволяет добавлять плейлисты в качестве удаленных видео.
* [#3202145](https://www.drupal.org/node/3202145) Запрос oEmbed ресурсов теперь имеет таймаут равный 5 секундам, а не 30, чтобы в случае проблем не блокировать запросы пользователей.
* [#3054888](https://www.drupal.org/node/3054888) oEmbed URL-адреса больше не переводятся принудительно в нижний регистр, так как некоторые провайдеры могут использовать пути с большим регистром, что приводит к ошибкам добавления таких адресов при сравнении.

## MySQL DB Driver

* [#3218978](https://www.drupal.org/node/3218978) Исправлена неполадка когда в [settings.php](../../../../9/settings-php/index.md) для соединения БД задана команда удаления `ANSI_QUOTES` из `['init_commands']['sql_mode']`, но она игнорировалась.

## Olivero

* [#3224466](https://www.drupal.org/node/3224466) `<button>` элементы теперь наследуют шрифт.
* [#3225188](https://www.drupal.org/node/3225188) Улучшены размеры заголовков чтобы они были различимых размеров.

## Syslog

* [#3224414](https://www.drupal.org/node/3224414) Исправлена неполадка из-за которой модуль Syslog использовал свои настройки до того как они были записаны.

## Typed Data System

* [#2047119](https://www.drupal.org/node/2047119) Акутализирована документация аннотации `DataType`.

## Тестирование

* [#3191935](https://www.drupal.org/node/3191935) Использование устаревшего `AssertLegacyTrait::assertNoText()` заменено современными методами.
* [#3170396](https://www.drupal.org/node/3170396) Вызовы `t()` заменены на `::pageTextContains()` в вызовах `::assertRaw()` и `::assertNoRaw()`.
* [#3227060](https://www.drupal.org/node/3227060) Вызов устаревшего `AssertLegacyTrait::assertNoRaw()` заменены на современные аналоги.
* [#3226008](https://www.drupal.org/node/3226008) Удалены вызовы `t()` в вызовах `::assertEquals()`.
* [#3220255](https://www.drupal.org/node/3220255) Сравнения с использованием `xpath` на ссылки заменены на WebAssert.

## Прочие изменения

* [#2989893](https://www.drupal.org/node/2989893) Из примера `substr` плагина-обработчика удалён лишний ключ `key`.
* [#3190070](https://www.drupal.org/node/3190070) Исправлен некорректный отступ в `default.services.yml`.