---
title: 'Drupal 9.4.1'
slug: wiki/drupal/releases/9.4.1
core: 9
metatags:
  title: 'Drupal 9.4.1: Список изменений'
  description: 'Список изменений Drupal 9.4.1.'
authors:
  - Niklan
category:
  area: 'Drupal 9.4.x'
  title: Drupal 9.4.1
  order: 1
---

<Aside type="warning">

Данная версия находится в разработке и не предназначена для использования.

</Aside>

## Composer

- [#3283498](https://www.drupal.org/node/3283498) Drupal Scaffold больше не приводит к ошибке если скафолд файл пустой.
- [#3145738](https://www.drupal.org/node/3145738) Исправлена ошибка в команде для обновления метапакетов ядра.

## File

- [#3283235](https://www.drupal.org/node/3283235) Исправлена опечатка в `FileFieldWidgetTest`.

## Form API

- [#2747273](https://www.drupal.org/node/2747273) Улучшен пример программной отправки формы в `FormBuilderInterface`.
- [#1645328](https://www.drupal.org/node/1645328) Добавлен тест, что `#title` элемент формы может быть также и не строкой (например 0).

## Forum

- [#3291265](https://www.drupal.org/node/3291265) `ForumNodeAccessTest` больше не зависит от модуля `tracker`.

## Help Topics

- [#3164699](https://www.drupal.org/node/3164699) Проведены проверки на 15 ошибочных слов и внесены исправления там где это требуется.

## PHP 8.2

- [#3280033](https://www.drupal.org/node/3280033) Использование устаревшего синтаксиса `${var}` заменено на `{$var}`.

## Quick Edit

- [#3268244](https://www.drupal.org/node/3268244) Исправлен тест `QuickEditIntegrationTest::testArticleNode()`.

## Settings Tray

- [#3257600](https://www.drupal.org/node/3257600) Удалено лишнее переопределение метода `::getTestThemes()` в `SettingsTrayBlockFormTest`.

## Прочие изменения

- [#3283794](https://www.drupal.org/node/3283794) Внесены исправления для «should return {type} but return statement is missing».