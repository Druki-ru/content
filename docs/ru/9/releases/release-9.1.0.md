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

## Entity API

- [#3033986](https://www.drupal.org/node/3033986) Удалено перезаписывание `$limit` в некоторых классах расширяющих `EntityListBuilder`. Оно было без значения.

## Migrate

- [#3024682](https://www.drupal.org/node/3024682) На странице со списком миграций теперь показываются человекопонятные названия, вместо машинных.

## Seven

- [#3054196](https://www.drupal.org/node/3054196) Исправлена проблема с белым фоном у кнопки в таблице.

## Taxonomy

- [#3122511](https://www.drupal.org/node/3122511) На странице редактирования добавлен пункт удаления во вкладки.

## User

- [#3082006](https://www.drupal.org/node/3082006) Поле пароля больше нельзя использовать в Views для вывода. Ранее он не показывал ничего, сейчас отключёна возможность выбора данного значения.

## Тестирование

- [#3131820](https://www.drupal.org/node/3131820) Использование `is_string()` заменено на нативные методы `::assertIsString()`, `::assertIsNotString()`.
- [#3130341](https://www.drupal.org/node/3130341) Удалён `UpdateKernel::fixSerializedExtensionObjects()`.
- [#3114617](https://www.drupal.org/node/3114617) Методы `Drupal\FunctionalTests\AssertLegacyTrait` и `Drupal\KernelTests\AssertLegacyTrait` помечены устаревшими.
- [#3138652](https://www.drupal.org/node/3138652) Удалён тест `StableDecoupledTest`.

## Устаревший API

- [#2278971](https://www.drupal.org/node/2278971) `Connection::supportsTransactions` помечен устаревшим. Таким образом [настройка](../settings-php.md) подключения к БД `transactions` также становится устаревшей.

## Прочие изменения

- [#3123472](https://www.drupal.org/node/3123472) Последовательный вызов методов `StorageComparer` больше не используется в условиях.
- [#2488350](https://www.drupal.org/node/2488350) При установке Drupal теперь используется кэш-бэкенд в памяти. Это позволяет ускорить установку.
- [#3127255](https://www.drupal.org/node/3127255) Из проверки системных требований удалены проверки `mbstring.http_input` и `mbstring.http_output`. Данные параметры, начиная с PHP 5.6 являются устаревшими и ничего не возвращают.
- [#2778917](https://www.drupal.org/node/2778917) Вместо тернарного оператора при вызове `\Drupal::state()->get()` теперь используется второй параметр.
- [#3021788](https://www.drupal.org/node/3021788) Функции `template_preprocess_menu_local_task()` и `template_preprocess_menu_local_action()` перенесены в `core/includes/theme.inc`.
- [#3112328](https://www.drupal.org/node/3112328) Классы расширяющие `FormatterBase` больше не реализуют `ContainerFactoryPluginInterface`, так как это объявлено в `FormatterBase`.
- [#3033734](https://www.drupal.org/node/3033734) На странице списка модулей исправлен горозонтальный скрол при больших описаниях.