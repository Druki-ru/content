---
title: 'Drupal 9.2.5'
slug: drupal/releases/9.2.5
core: 9
metatags:
  title: 'Drupal 9.2.5: Список изменений'
  description: 'Список изменений Drupal 9.2.5.'
---

**Дата релиза:** 2 сентября 2021

## CKEditor

* [#3227945](https://www.drupal.org/node/3227945) Из процесса сборки CKEditor удален `bender-runner.config.json`.

## Composer

* [#3221748](https://www.drupal.org/node/3221748) В примере для [Composer плагина Scaffold](../../../../../composer/drupal/core-composer-scaffold/index.md) убран `drupal/core`, так как он явно разрешен и его объявление там не окажет никакого эффекта.

## Database System

* [#3228237](https://www.drupal.org/node/3228237) `Drupal\Core\Command\DbDumpCommand::getTables()` теперь сортирует таблицы по алфавиту.

## Forms System

* [#3229012](https://www.drupal.org/node/3229012) Исправлена ошибка в комментарии к `FormSubmitter`.

## Media System

* [#3063343](https://www.drupal.org/node/3063343) `MediaLibraryState` теперь реализует `CacheableDependencyInterface`.
* [#3222616](https://www.drupal.org/node/3222616) Улучшено регулярное выражение для YouTube, которое теперь позволяет добавлять плейлисты в качестве удаленных видео.
* [#3202145](https://www.drupal.org/node/3202145) Запрос oEmbed ресурсов теперь имеет таймаут равный 5 секундам, а не 30, чтобы в случае проблем не блокировать запросы пользователей.
* [#3054888](https://www.drupal.org/node/3054888) oEmbed URL-адреса больше не переводятся принудительно в нижний регистр, так как некоторые провайдеры могут использовать пути с большим регистром, что приводит к ошибкам добавления таких адресов при сравнении.

## Migration System

* [#3022910](https://www.drupal.org/node/3022910) (откачено) Название файла (`filename`) теперь получается из `uri`.

## MySQL DB Driver

* [#3218978](https://www.drupal.org/node/3218978) Исправлена неполадка когда в [settings.php](../../../../9/settings-php/index.md) для соединения БД задана команда удаления `ANSI_QUOTES` из `['init_commands']['sql_mode']`, но она игнорировалась.

## Olivero

* [#3224466](https://www.drupal.org/node/3224466) `<button>` элементы теперь наследуют шрифт.
* [#3225188](https://www.drupal.org/node/3225188) Улучшены размеры заголовков чтобы они были различимых размеров.
* [#3228145](https://www.drupal.org/node/3228145) `aria-label` для кнопки поиска заменена на «Search Form».
* [#3227427](https://www.drupal.org/node/3227427) Улучшено отображение состояния фокуса элемента `<summary>` на Windows в режиме высокой контрастности. 
* [#3223268](https://www.drupal.org/node/3223268) Исправлена неполадка на IE11 из-за которой в выпадающем меню появляется прокрутка.
* [#3221871](https://www.drupal.org/node/3221871) Исправлена неполадка при которой ссылка из мобильного меню ведущая на текущую страницу с якорем не закрывала меню.
* [#3226704](https://www.drupal.org/node/3226704) Улучшено отображение пункта главного меню при наведении.
* [#3212120](https://www.drupal.org/node/3212120) Размер шрифта цитаты уменьшено если оно выводится в сайдбере.

## Quick Edit

* [#3228634](https://www.drupal.org/node/3228634) Все интеграционные тесты QuickEdit, включая `QuickEditImageEditorTestTrait`, были перемещены в пространство имён `quickedit`.

## Render System

* [#3228656](https://www.drupal.org/node/3228656) Удалён устаревший `@todo` в `HtmlResponseAttachmentsProcessor`.

## Syslog

* [#3224414](https://www.drupal.org/node/3224414) Исправлена неполадка из-за которой модуль Syslog использовал свои настройки до того как они были записаны.

## Theme System

* [#3228963](https://www.drupal.org/node/3228963) Исправлен путь в сообщении исключения `ThemeExtensionList`.

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
* [#3221715](https://www.drupal.org/node/3221715) [Brian Perry «brianperry»](https://www.drupal.org/u/brianperry) добавлен в качестве координатора инициативы decoupled меню.
* [#3018091](https://www.drupal.org/node/3018091) Дополнена документация для `TaggedHandlersPass::process()`.
* [#3203416](https://www.drupal.org/node/3203416) Добавлено объяснение, что параметр $form_id может обнаружить рекурсию в `FormValidator::doValidateForm()`.

## Ссылки

- [Drupal 9.2.5](https://www.drupal.org/project/drupal/releases/9.2.5) (англ.), drupal.org, 2 сентября 2021
