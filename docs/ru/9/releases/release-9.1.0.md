---
id: release-9.1.0
title: 'Drupal 9.1.0'
path: /9/releases/9.1.0
core: 9
metatags:
  title: 'Drupal 9.1.0: Список изменений'
  description: 'Список изменений Drupal 9.1.0.'
---

**Дата релиза**: TBA

> [!WARNING]
> Drupal 9.1.0 находится в разработке.

## Cache::merge* методы теперь принимают неограниченное кол-во аргументов

- [#3125032](https://www.drupal.org/node/3125032)

Следующие методы теперь принимают неограниченное количество аргументов:

- `\Drupal\Core\Cache\Cache::mergeTags()`
- `\Drupal\Core\Cache\Cache::mergeMaxAges()`
- `\Drupal\Core\Cache\Cache::mergeContexts()`

### Пример

Имеем следующие кэш-теги:

```php
$cache_tags_foo = ['foo'];
$cache_tags_bar = ['foo', 'bar'];
$cache_tags_baz = ['baz'];
```

Как было ранее:

```php
$merge_tags = \Drupal\Core\Cache\Cache::mergeTags(\Drupal\Core\Cache\Cache::mergeTags($cache_tags_foo, $cache_tags_bar), $cache_tags_baz);
```

Новый вариант №1:

```php
$merge_tags = \Drupal\Core\Cache\Cache::mergeTags($cache_tags_foo, $cache_tags_bar, $cache_tags_baz);
```

Новый вариант №2:

```php
$args = [$cache_tags_foo, $cache_tags_bar, $cache_tags_baz];
$merge_tags = \Drupal\Core\Cache\Cache::mergeTags(...$args);
```

## Taxonomy

- [#3122511](https://www.drupal.org/node/3122511) На странице редактирования добавлен пункт удаления во вкладки.

## Устаревший API

- [#2278971](https://www.drupal.org/node/2278971) `Connection::supportsTransactions` помечен устаревшим. Таким образом настройка подключения к БД `transactions` также становится устаревшей.

## Прочие изменения

- [#3123472](https://www.drupal.org/node/3123472) Последовательный вызов методов `StorageComparer` больше не используется в условиях.
- [#2488350](https://www.drupal.org/node/2488350) При установке Drupal теперь используется кэш-бэкенд в памяти. Это позволяет ускорить установку.
- [#3127255](https://www.drupal.org/node/3127255) Из проверки системных требований удалены проверки `mbstring.http_input` и `mbstring.http_output`. Данные параметры, начиная с PHP 5.6 являются устаревшими и ничего не возвращают.