---
id: release-8.7.7
title: 'Drupal 8.7.7'
path: /8/releases/8.7.7
core: 8
metatags:
  title: 'Drupal 8.7.7: Список изменений'
  description: 'В данном релизе исправлены различные ошибки и внесены улучшения.'
---

**Дата релиза**: 4 сентября 2019 г.

## JSON:API

- [#3075831](https://www.drupal.org/node/3075831) В модуле JSON:API улучшена проверка на бандл сущности. Теперь бандлы, чьё название состоит только из цифр, преобразуются в строковое значение для сравнения и проходят проверки.
- [#3043168](https://www.drupal.org/node/3043168) Улучшена проверка языка для `PATCH` запросов. Теперь можно патчить контент-сущности, которые не являются переводимыми.

## Views

- [#3006815](https://www.drupal.org/node/3006815) Исправлена неполадка, когда `ViewsEntitySchemaSubscriber` приводил к ошибке, если хендлер сломан.

## Migration API

- [#3007102](https://www.drupal.org/node/3007102) Исправлена неполадка, при которой миграция значения поля "date only" не сбрасывало значение для времени, из-за чего приводила к "битым" данным.
- [#3067889](https://www.drupal.org/node/3067889) Исправлена неполадка, из-за которой некорректно мигрировали метки `boolean` поля.
- [#3076169](https://www.drupal.org/node/3076169) Исправлен неймспейс для `OptionWidgetsField`, который по ошибке имел префикс `d7`, будучи плагином для `d6`.

## Workspaces

- [#2984938](https://www.drupal.org/node/2984938) При переключение рабочих пространств, теперь запоминается страница, откуда было произведено переключение, и при возвращении, урл восстанавливается.

## Прочие изменения

- [#3078001](https://www.drupal.org/node/3078001), [#3078001](https://www.drupal.org/node/3078001) `core` параметр в `*info.yml` файлах теперь поддерживает указание семантической версионности.
- [#3076609](https://www.drupal.org/node/3076609), [#3076609](https://www.drupal.org/node/3076609) Улучшены тесты `CKEditorIntegrationTest` для sqlite.
- [#3061610](https://www.drupal.org/node/3061610), [#3061610](https://www.drupal.org/node/3061610) Исправлен Typed Data's EntityDeriver, который не мог корректно обработать поведение, когда бандл сущности имел то же машинное имя, как и сама сущность.
- [#3075661](https://www.drupal.org/node/3075661) Исправлен некорректный тайпхинтинг в phpDoc для методов `getHtml5DateFormat()` и `getHtml5TimeFormat()` объекта `Drupal\Core\Datetime\Element\Datetime`.
- [#3073342](https://www.drupal.org/node/3073342) Обновлены инструкции PHPUnit, для того чтобы JavaScript тесты работали с ChromeDriver 75+.
- [#2962765](https://www.drupal.org/node/2962765) Добавлены примеры как задавать переменные MINK в `phpunit.xml.dist`.
- [#2849413](https://www.drupal.org/node/2849413) Исправлена неполадка, приводящая к фатальной ошибке, если ImageStyle пытается построить URL с использованием StreamWrapper, который ещё не зарегистрирован.

## Известные проблемы

- [#3078671](https://www.drupal.org/project/drupal/issues/3078671) Некоторые пользователи сообщают о проблемах с пакетом `webflo/drupal-core-require-dev`.

## Ссылки

- [Drupal 8.7.7](https://www.drupal.org/project/drupal/releases/8.7.7) (англ.), drupal.org, 4 сентября 2019
