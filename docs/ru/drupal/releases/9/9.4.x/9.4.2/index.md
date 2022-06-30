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
  title: Drupal 9.4.2 (в разработке)
  order: 2

---

<Aside type="warning">

Данная версия находится в разработке и не предназначена для использования.

</Aside>

## Composer

- [#3293051](https://www.drupal.org/node/3293051) В 
  `Composer::preAutoloadDump()` удалено лишнее условие.

## Layout Builder

- [#3264633](https://www.drupal.org/node/3264633), [#3292413](https://www.drupal.org/node/3292413) Функционал для интеграции
  Layout Builder с модулем Quick Edit вынесен непосредственно в Quick Edit.

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