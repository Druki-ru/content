---
title: 'Drupal 9.4.2'
slug: wiki/drupal/releases/9.4.2
core: 9
metatags:
  title: 'Drupal 9.4.2: Список изменений'
  description: 'Список изменений Drupal 9.4.2.'
authors:
  - Niklan
category:
  area: 'Drupal 9.4.x'
  title: Drupal 9.4.2
  order: 2

---

**Дата релиза:** 7 июля 2022

## CKEditor 5

- [#3291295](https://www.drupal.org/node/3291295) В схему `ckeditor5.plugin.
  media_media` добавлена отсутствующая метка.

## Composer

- [#3293051](https://www.drupal.org/node/3293051) В 
  `Composer::preAutoloadDump()` удалено лишнее условие.

## Config System

- [#3264145](https://www.drupal.org/node/3264145) В `ConfigOverrider` 
  добавлен отсутствующий `use`.
- [#3225381](https://www.drupal.org/node/3225381) Исправлена неполадка в 
  `ConfigSingleExportForm`, приводящая к PHP уведомлению.

## Image System

- [#3239935](https://www.drupal.org/node/3239935) (отменено) Произведён рефакторинг `ToolkitGdTest`.

## Layout Builder

- [#3264633](https://www.drupal.org/node/3264633), [#3292413](https://www.drupal.org/node/3292413) Функционал для интеграции
  Layout Builder с модулем Quick Edit вынесен непосредственно в Quick Edit.
- [#3031492](https://www.drupal.org/node/3031492) Из конструктора 
  `InlineBlockEntityOperations` удалён `@todo`.

## Migrate

- [#3270556](https://www.drupal.org/node/3270556) `FieldInstanceSettings` 
  для Drupal 6 теперь корректно обрабатывает отсутствующее значение 
  `max_filesize_per_file`.

## Olivero

- [#3291317](https://www.drupal.org/node/3291317) Исправлена метка 
  «Предыдущая страница» в 
  мини-пейджере для кнопки «Следующая страница».
- [#3284010](https://www.drupal.org/node/3284010) Ссылка в `get-started.html.
  twig` для добавления содержимого заменена с фиксированной на маршрут.

## Views

- [#2987089](https://www.drupal.org/node/2987089) `ViewExecutable` больше не 
  использует свойство `_serviceId`.
- [#3291303](https://www.drupal.org/node/3291303) Из `BasicTest` удалена 
  устаревшая проверка.

## Прочие изменения

- [#3291877](https://www.drupal.org/node/3291877) Исправлен `InstallerTestBase`.
- [#3285230](https://www.drupal.org/node/3285230)
  В `DownloadFunctionalTest:: testExceptionThrow()` внесены улучшения для
  совместимости с `guzzlehttp/psr7` 2.3.0.
- [#3293114](https://www.drupal.org/node/3293114) Исправлен некорректный 
  комментарий в `BrowserHtmlDebugTrait`.
- [#3070747](https://www.drupal.org/node/3070747) Теперь при сбросе кеша 
  очищаются пути до установочных профилей и движков темизации.
- [#3291830](https://www.drupal.org/node/3291830) Пути до драйверов баз 
  данных теперь соответствуют PSR4.
- [#2896993](https://www.drupal.org/node/2896993) Декораторы сервисов, 
  которые были ранее сериализованы теперь корректно обрабатываются при 
  десириализации.
- [#3292850](https://www.drupal.org/node/3292850) В `MAINTAINERS.txt` 
  добавлены мейнтейнеры инициативы Project Browser.

## Ссылки

- [Drupal 9.4.2](https://www.drupal.org/project/drupal/releases/9.4.2) (англ.
  ), drupal.org, 7 июля 2022