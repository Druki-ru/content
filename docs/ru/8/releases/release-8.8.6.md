---
id: release-8.8.6
title: 'Drupal 8.8.6'
path: /8/releases/8.8.6
core: 8
metatags:
  title: 'Drupal 8.8.6: Список изменений'
  description: 'Патч релиз Drupal 8.8.6, список изменений.'
---

**Дата релиза**: В разработке

> [!WARNING]
> Данный релиз находится в разработке.

## CKEditor

- [#3070745](https://www.drupal.org/node/3070745) `localStorage` теперь хранит только последние версии стилей, для избежания проблем лимита хранения данного хранилища.

## Views

- [#2989745](https://www.drupal.org/node/2989745) Обновление конфигураций обновления `views_update_8500()` перенесены на процесс сохранения конфигурации для сохранения BC.

## User

- [#3084813](https://www.drupal.org/node/3084813) Тайпхинт для `user_load()` был изменён с `object|bool` на `\Drupal\user\UserInterface|false`.

## Update

- [#2992631](https://www.drupal.org/node/2992631) Информация об обновлении больше не будет рекомендовать новую минорную версию при наличии обновления безопасности для текущей минорной.

## Тестирование

- [#3126695](https://www.drupal.org/node/3126695) Возвращены методы `::assertEqualsCanonicalizing`, `::assertNotEqualsCanonicalizing`, `::assertEqualsCanonicalizing`, `::assertNotEqualsCanonicalizing` и `::testAssertEqualsCanonicalizing`, которые вызывают новые. Это изменение касается только Drupal 8. Тесты для Drupal 9 по-прежнему должны использовать новые методы.
- [#3126797](https://www.drupal.org/node/3126797) Возвращены методы `::assertStringContainsString`, `::assertStringContainsStringIgnoringCase`, `::assertStringNotContainsString`, `::assertStringNotContainsStringIgnoringCase`, `::assertStringContainsString`, `::assertStringContainsStringIgnoringCase`, `::assertStringNotContainsString`, `::assertStringNotContainsStringIgnoringCase`  и `::testAssertStringContainsString`, которые вызывают новые. Это изменение касается только Drupal 8. Тесты для Drupal 9 по-прежнему должны использовать новые методы.

## Прочие изменения

- [#3030989](https://www.drupal.org/node/3030989) Исправлена ошибка вызываемая при попытке массово удалить удалённые ноды.
- [#3074047](https://www.drupal.org/node/3074047) Исправлена и улучшена документация метода `MigrateDestinationInterface::import`. Теперь возвращаемый тип не `mixed`, а `array|bool`.
- [#3098475](https://www.drupal.org/node/3098475) Проверка на реализацию хука `hook_update_last_removed()` в процессе обновления баз данных стала более строгой и сообщение об ошибке более доступным.
- [#3121362](https://www.drupal.org/node/3121362) Различные исправления опечаток.
- [#3120901](https://www.drupal.org/node/3120901) Сообщения об устаревшем коде в 8.8.4 обновлены до 8.8.5, так как [Drupal 8.8.4](release-8.8.4.md) — обновление безопасности.