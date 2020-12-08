---
id: release-9.1.1
title: 'Drupal 9.1.1'
path: /9/releases/9.1.1
core: 9
metatags:
  title: 'Drupal 9.1.1: Список изменений'
  description: 'Список изменений Drupal 9.1.1.'
---

**Дата релиза**: 6 января 2020

> [!WARNING]
> Данный релиз находится в разработке, актуальная версия [9.1.0](release-9.1.0.md).

> [!NOTE]
> Данный релиз также содержит изменения внесённые в [Drupal 8.9.12](../../8/releases/release-8.9.12.md).

## Content Moderation

- [#3048962](https://www.drupal.org/project/drupal/issues/3048962) Для полей состояния добавлена реализация `::generateSampleItems()` которая ничего не делает.

## Layout Builder

- [#3103812](https://www.drupal.org/project/drupal/issues/3103812) Формы `ConfigureSectionForm` теперь корректно отображаются ошибки валидации.

## Migration system

- [#3151980](https://www.drupal.org/project/drupal/issues/3151980) Миграция `d7_system_mail` теперь пропускается для элементов без `mail_system`.
- [#3184545](https://www.drupal.org/project/drupal/issues/3184545) Исправлена документация о возвращаемом типе `Migration::getMigrationPluginManager()`.

## Olivero

- [#3183112](https://www.drupal.org/project/drupal/issues/3183112) Исправлено значение высоты с «px» на «em».

## Theme system

- [#3120567](https://www.drupal.org/project/drupal/issues/3120567) Исправлена опечатка в описании переменной `secondary` в файлах `menu-local-tasks.html.twig`.

## User

- [#86287](https://www.drupal.org/project/drupal/issues/86287) Письмо с одноразовой ссылкой на вход теперь учитывает язык выбранный пользователем и отсылает письмо на данном языке.
- [#3109109](https://www.drupal.org/project/drupal/issues/3109109) Токен для восстановления пароля `pass-reset-token` теперь проверяется только из GET параметра.

## Views

- [#2969107](https://www.drupal.org/project/drupal/issues/2969107) Аргументы для даты больше не возвращают HTTP 500 при некорректном формате.
- [#3069925](https://www.drupal.org/project/drupal/issues/3069925) Views теперь проверяет значение `target_bundles` для поля и если не задано, не выводит ошибку.

## Тестирование

- [#3186443](https://www.drupal.org/project/drupal/issues/3186443) Исправлена ошибка «Call to undefined method ::getAnnotations()» при использовании PHPUnit 9.5.
- [#3184493](https://www.drupal.org/project/drupal/issues/3184493) Исправлены вызовы `t()` при конкатенации строк.
- [#3184632](https://www.drupal.org/project/drupal/issues/3184632) Сравнения с xpath для отправленных данных с форм заменены на WebAssert.

## Прочие изменения

- [#3185917](https://www.drupal.org/project/drupal/issues/3185917) Улучшена производительность `TaggedHandlerPass`.
- [#3172757](https://www.drupal.org/project/drupal/issues/3172757) В `SessionManager::destroy()` добавлена проверка на то что вызов из CLI.
- [#3151800](https://www.drupal.org/project/drupal/issues/3151800) Улучшена документация для `DataDefinitionInterface::setInternal()`.