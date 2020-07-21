---
id: release-9.1.0
title: 'Drupal 9.1.0'
path: /9/releases/9.1.0
core: 9
metatags:
  title: 'Drupal 9.1.0: Список изменений'
  description: 'Список изменений Drupal 9.1.0.'
---

**Дата релиза**: 2 декабря 2020\
**Прекращение исправления ошибок**: 2 июня 2020\
**Прекращение поддержки безопасности**: ноябрь 2021

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

## Добавлен новый сервис user.flood_control и соответствующие события

- [#2983395](https://www.drupal.org/project/drupal/issues/2983395)

Модуль User теперь будет отвечать со статусом HTTP 403 если попытка авторизации заблокирована flood control.

В дополнение, добавлен новый сервис `user.flood_control`, который построен поверх сервиса `flood`. Данный сервис вызывает новые события:

- `UserEvents::FLOOD_BLOCKED_IP`: Событие вызывается когда авторизация заблокирована на уровне IP.
- `UserEvents::FLOOD_BLOCKED_USER`: Событие вызывается когда авторизация заблокирована по причине того что пользователь, под которым пытаются авторизоваться, заблокирован.

По умолчанию, данные события используются для добавления соответствующих записей в журнал системы.

## Connection::prepareQuery и Connection::prepare помечены устаревшими

- [#2345451](https://www.drupal.org/project/drupal/issues/2345451)

`Connection::prepareQuery` и `Connection::prepare` помечены устаревшими. Новые замены:

- `Connection::prepareQuery` заменён на `Connection::prepareStatement`.
- `Connection::prepare` заменён на [PDO::prepare](https://www.php.net/manual/en/pdo.prepare.php).

Данное изменение задевает только разработчиков модулей, предоставляющих новые драйвера БД.

**Раньше:**

```php
// $query is the query as a SQL string.
\Drupal\Core\Database\Connection->prepareQuery($query);

\Drupal\Core\Database\Connection->prepare($query);
```

**Теперь:**

```php
// $options are the query options.
\Drupal\Core\Database\Connection->prepareStatement($query, $options);

// For the possible $driver_options, see: https://www.php.net/manual/en/pdo.prepare.php
\PDO::prepare($query, $driver_options);
```

## Глобальные константы bootstrap.inc, относящиеся к PHP, помечены устаревшими

- [#2908079](https://www.drupal.org/project/drupal/issues/2908079)

Для минимизации подключения файлов с глобальными константами, четыре глобальные константы относящиеся к PHP перенесены в `Drupal`.

- Вместо `DRUPAL_MINIMUM_PHP` используйте `\Drupal::MINIMUM_PHP`.
- Вместо `DRUPAL_MINIMUM_SUPPORTED_PHP` используйте `\Drupal::MINIMUM_SUPPORTED_PHP`.
- Вместо `DRUPAL_RECOMMENDED_PHP` используйте `\Drupal::RECOMMENDED_PHP`.
- Вместо `DRUPAL_MINIMUM_PHP_MEMORY_LIMIT` используйте `\Drupal::MINIMUM_PHP_MEMORY_LIMIT`.

## Зависимость ядра symfony-cmf/routing помечена устаревшей

- [#2917331](https://www.drupal.org/project/drupal/issues/2917331)

Зависимость ядра `symfony-cmf/routing` помечена устаревшей. Следующие классы и интерфейсы заменены собственной реализацией в ядре:

- `\Symfony\Cmf\Component\Routing\RouteObjectInterface` заменён на `\Drupal\Core\Routing\RouteObjectInterface`.
- `\Symfony\Cmf\Component\Routing\RouteProviderInterface` заменён на `\Drupal\Core\Routing\RouteProviderInterface`.
- `\Symfony\Cmf\Component\Routing\LazyRouteCollection` заменён на `\Drupal\Core\Routing\LazyRouteCollection`.

Обратите внимание на то, что константа `RouteObjectInterface::ROUTE_NAME` теперь предоставляется `\Drupal\Core\Routing\RouteObjectInterface`.

Методы `getRoutesPaged()` и `getRoutesCount()` предоставляемые `\Drupal\Core\Routing\RouteProvider` помечены устаревшими и будут удалены в Drupal 10.

## Claro

- [#3060697](https://www.drupal.org/project/drupal/issues/3060697) Claro теперь использует `#dropbutton_type` для вариантов `dropbutton` элемента, вместо классов.
- [#3154425](https://www.drupal.org/project/drupal/issues/3154425) Удалён комментарий «@todo Remove this after 8.6.x is out of support.» и код для него.

## Composer

- [#3156558](https://www.drupal.org/project/drupal/issues/3156558) Обновлены зависимости.

## Content Moderation

- [#3044292](https://www.drupal.org/project/drupal/issues/3044292) Добавлен новый метод `::isModeratedEntity` для хендлеров moderation сущностей.

## Database System

- [#3143618](https://www.drupal.org/project/drupal/issues/3143618) Обычные пробелы (`U+0020`) теперь заменяются на неделимые пробелы (`U+00A0`), для минимизации ложных срабатываний.
- [#3151990](https://www.drupal.org/project/drupal/issues/3151990) Запросы к БД переписаны на EntityQuery в `NodeRevisionPermissionsTest`.
- [#3151981](https://www.drupal.org/project/drupal/issues/3151981) Запросы к БД переписаны на EntityQuery в `NodeRevisionsAllTest`.
- [#3152001](https://www.drupal.org/project/drupal/issues/3152001) Запросы к БД переписаны на EntityQuery в `NodeAccessBaseTableTest`.
- [#3151959](https://www.drupal.org/project/drupal/issues/3151959) Запросы к БД переписаны на EntityQuery в `PathTaxonomyTermTest`.
- [#3151990](https://www.drupal.org/project/drupal/issues/3151990) Запросы к БД переписаны на EntityQuery в `NodeRevisionPermissionsTest`.
- [#3151968](https://www.drupal.org/project/drupal/issues/3151968) Запросы к БД переписаны на EntityQuery в `NodeTranslationUITest`.

## Entity System

- [#3033986](https://www.drupal.org/node/3033986) Удалено перезаписывание `$limit` в некоторых классах расширяющих `EntityListBuilder`. Оно было без значения.
- [#2927077](https://www.drupal.org/project/drupal/issues/2927077) `Entity::toUrl` теперь передает параметр ревизии на все маршруты, чьё название начинается с `revision`. Таким образом, это автоматизирует передачу параметров для кастомных маршрутов типа `revision_revert` и `revision_delete`.

## Extension System

- [#3150726](https://www.drupal.org/project/drupal/issues/3150726) Функция `update_check_incompatibility()` помечена устаревшей.

## Image

- [#3153009](https://www.drupal.org/project/drupal/issues/3153009) Добавлен новый стиль изображения устанавливаемый с модулем — «Wide (1090)». Он будет использоваться как Hero стиль в будущей теме Olivero.

## JavaScript

- [#3145930](https://www.drupal.org/project/drupal/issues/3145930) Размер «липкого» заголовка теперь пересчитывается после сворачивания и разворачивания тулбара.
- [#3096516](https://www.drupal.org/project/drupal/issues/3096516) В `domready` внесены улучшения, которые решают проблему с [race condition](https://ru.wikipedia.org/wiki/%D0%A1%D0%BE%D1%81%D1%82%D0%BE%D1%8F%D0%BD%D0%B8%D0%B5_%D0%B3%D0%BE%D0%BD%D0%BA%D0%B8).
- [#3152473](https://www.drupal.org/project/drupal/issues/3152473) Улучшена работа обратного вызова domready.

## Media

- [#3142818](https://www.drupal.org/project/drupal/issues/3142818) Из ссылок удалён аттрибут `target=_blank`.

## Migrate

- [#3024682](https://www.drupal.org/node/3024682) На странице со списком миграций теперь показываются человекопонятные названия, вместо машинных.

## Help Topics

- [#3047723](https://www.drupal.org/project/drupal/issues/3047723) Документация модулей views, views_ui конвертирована в Help Topics.
- [#3067614](https://www.drupal.org/project/drupal/issues/3067614) Документация модулей filter, ckeditor, editor конвертирована в Help Topics.

## REST

- [#3152848](https://www.drupal.org/project/drupal/issues/3152848) Код связанный с `bc_entity_resource_permissions` настройкой удалён, так как она больше не используется.

## Search

- [#3086794](https://www.drupal.org/project/drupal/issues/3086794) Плагины результатов поиска теперь могут указывать, какую тему использовать для отрисовки страниц.
- [#3086795](https://www.drupal.org/project/drupal/issues/3086795) «Search help» на странице поиска заменён на «About searching» для избежания двусмысленности.
- [#3155221](https://www.drupal.org/project/drupal/issues/3155221) Удален устаревший «@todo».

## Serialization

- [#3135304](https://www.drupal.org/project/drupal/issues/3135304) Удалён слой обратной совместимости с Symfony 3 из `JsonEncoder`.

## Seven

- [#3054196](https://www.drupal.org/node/3054196) Исправлена проблема с белым фоном у кнопки в таблице.

## Taxonomy

- [#3122511](https://www.drupal.org/node/3122511) На странице редактирования добавлен пункт удаления во вкладки.
- [#3151953](https://www.drupal.org/project/drupal/issues/3151953) В тесте `TermTranslationUITest` использование прямого запроса заменено на Entity Query.

## User

- [#3082006](https://www.drupal.org/node/3082006) Поле пароля больше нельзя использовать в Views для вывода. Ранее он не показывал ничего, сейчас отключена возможность выбора данного значения.
- [#3150070](https://www.drupal.org/project/drupal/issues/3150070) Видимость свойств в новых тестах изменена с `public` на `protected`.

## Views

- [#3139353](https://www.drupal.org/project/drupal/issues/3139353) Добавлен новый публичный метод `Drupal\views\Plugin\views\query\Sql::getConnection()`.
- [#3150490](https://www.drupal.org/project/drupal/issues/3150490) Улучшено именование переменных в `Drupal\views\ViewExecutableFactory::get`.

## Тестирование

- [#3131820](https://www.drupal.org/node/3131820) Использование `is_string()` заменено на нативные методы `::assertIsString()`, `::assertIsNotString()`.
- [#3130341](https://www.drupal.org/node/3130341) Удалён `UpdateKernel::fixSerializedExtensionObjects()`.
- [#3114617](https://www.drupal.org/node/3114617) Методы `Drupal\FunctionalTests\AssertLegacyTrait` и `Drupal\KernelTests\AssertLegacyTrait` помечены устаревшими.
- [#3138652](https://www.drupal.org/node/3138652) Удалён тест `StableDecoupledTest`.
- [#3139412](https://www.drupal.org/project/drupal/issues/3139412) Использование `::assertTitle()` заменено на `$this->assertSession()->titleEquals()`.
- [#3077785](https://www.drupal.org/project/drupal/issues/3077785) `DrupalMinkClient` удалён и весь код отрефакторен для использования Mink `Client`.
- [#3139437](https://www.drupal.org/project/drupal/issues/3139437) Использование устаревшего `AssertLegacyTrait::assertCacheTag` заменено на `$this->assertSession()->responseHeaderContains()`.
- [#3144732](https://www.drupal.org/project/drupal/issues/3144732) Удалены вызовы `t()` в связке с `$this->assertSession()->optionExists()`.
- [#3135538](https://www.drupal.org/project/drupal/issues/3135538) Заменены оставшиеся `assert*` вызовы использующие `count()`. 

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
- [#3112790](https://www.drupal.org/project/drupal/issues/3112790) Исправлена неполадка, из-за которой «установка» модулей User и System происходила дважды.
- [#3143605](https://www.drupal.org/project/drupal/issues/3143605) Удалена функция `update_replace_permissions()`.
- [#2972224](https://www.drupal.org/project/drupal/issues/2972224) В ядро добавлен `.cspell.json` для автоматической проверки правописания в ядре Drupal.
- [#2256367](https://www.drupal.org/project/drupal/issues/2256367) Использование «web site» в документации и UI заменено на «website».
- [#3143724](https://www.drupal.org/project/drupal/issues/3143724) «dont» заменён на «do_not» в якоре ссылки на документацию.
- [#3153790](https://www.drupal.org/project/drupal/issues/3153790) Исправлена опечатка в сервисе `user.flood_subscriber`.
- [#3085751](https://www.drupal.org/project/drupal/issues/3085751) `setter` свойство у [сервисов](../services/services.md) теперь учитывается как зависимость и не должно приводить к ошибкам в обновлениях.
- [#3055189](https://www.drupal.org/project/drupal/issues/3055189) Маппинг ключей в несколько строк помечен устаревшим в Symfony 4.3, соответствующие изменения внесены в ядро.
- [#2937844](https://www.drupal.org/project/drupal/issues/2937844) Внесены исправления для соответствия стандарту `Squiz.PHP.NonExecutableCode`.
- [#3143713](https://www.drupal.org/project/drupal/issues/3143713) Функция `drupal_get_schema_versions()` теперь всегда возвращает целые числа.
- [#3154594](https://www.drupal.org/project/drupal/issues/3154594) `composer.json` и `composer.lock` будут пропускаться CSpell.
- [#3154665](https://www.drupal.org/project/drupal/issues/3154665) Из словаря CSpell удалены названия модулей и плагинов.