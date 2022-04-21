---
title: 'Drupal 9.4.0'
slug: wiki/drupal/releases/9.4.0
core: 9
metatags:
  title: 'Drupal 9.4.0: Список изменений'
  description: 'Список изменений Drupal 9.4.0.'
authors:
  - Niklan
category:
  area: 'Drupal 9.4.x'
  title: Drupal 9.4.0
  order: 0
---

<Aside type="warning">

Данная версия находится в разработке. Список изменений может меняться и дополняться, даты могут смещаться.

</Aside>

**Дата релиза**: июнь 2022\
**Активная поддержка до**: декабрь 2022\
**Поддержка безопасности до**: июнь 2023

## Хук `THEME_ENGINE_init()` объявлен устаревшим

- [#3158289](https://www.drupal.org/node/3158289) 

Хук `THEME_ENGINE_init()` объявлен устаревшим и будет удалён в [Drupal 10](../../../../10/index.md). Замена данному хуку не предоставляется.

## Для сущностей с поддержкой ревизий, модуль Views теперь предоставляет связи между основной и ревизионной таблицами

- [#2652652](https://www.drupal.org/node/2652652)

Ранее, модуль Views не предоставлял для ревизионных сущностей связи между основной таблицой и таблицой ревизий. Для модулей `block_content` и `node` были «костыли» для решения данной проблемы, но остальным сущностям приходилось решать это вопрос самостоятельно.

Начиная с текущего релиза, Views предоставляет соответствующие связи между основной и ревизионной таблицами для всех типов сущностей что поддерживают ревизии.

## В .htaccess файлы добавлена поддержка настроек PHP 8

- [#3186524](https://www.drupal.org/node/3186524)

В `.htaccess` файлы добавлен новый раздел для настроек параметров PHP 8. Это изменение касается как основного файла `.htaccess`, так и всех прочих что генерируются Drupal для различных ситуаций, например, для `sites/default/files`.

В эти файлы добавлена настройка для PHP 8:

```text
# Имеющийся раздел для PHP 7.
<IfModule mod_php7.c>
  php_value assert.active   0
</IfModule>
# Новые настройки для PHP 8.
<IfModule mod_php.c>
  php_value assert.active   0
</IfModule>
```

Если вы переопределяете данные файлы или изменяете параметры PHP, для PHP 8 вам необходимо скопировать данные настройки и поправить под себя. Если вы используете стандартные настройки, файлы обновятся автоматически.

## Для оптимизации определения сериализаторов и нормализаторов, `NormalizerBase` теперь реализует `CacheableSupportsMethodInterface`

- [#3252872](https://www.drupal.org/node/3252872)

`Drupal\serialization\Normalizer\NormalizerBase` теперь реализует `CacheableSupportsMethodInterface` и возвращает `FALSE` по умолчанию. Это означает, что классы, расширяющие абстрактные классы из ядра продолжат работать как и раньше. Все конкретные реализации теперь имеют метод `::hasCacheableSupportsMethod()`, который возвращает `TRUE`.

Данный интерфейс был представлен в [Symfony 4.1](https://symfony.com/blog/new-in-symfony-4-1-faster-serializer):

> Приложения могут определять множество нормализаторов и денормализаторов, Symfony вызывает `::supportsNormalization()` для каждого из них, каждый раз производя нормализацию или денормализацию объекта. В теории, результат вызова `::supportsNormalization()` зависит от моженства факторов. На практике большинство из них зависит лишь от типа и формата, а данная информация легко кешируется.

Утверждение выше также подходит и для Drupal. Мы можем кешировать результат основываясь на переданном типе и формате, что позволяет сократить количество вызовов. Подобное изменение позволяет добиться увеличения производительности от 5 до 60 процентов!

## Драйвера баз данных, предоставляемые ядром Drupal, перенесены в соответствующие одноимённые модули

- [#3129043](https://www.drupal.org/node/3129043) 

[Ранее](../../../8/8.9.x/8.9.0/index.md) была добавлена поддержка драйверов баз данных, как в пространстве имён модуля, так и в «src» директории ядра. Теперь эта возможность задействована для переноса драйверов баз данных предоставляемых ядром в соответствующие модули. Для каждого драйвера базы данных добавлен новый одноимённый модуль: `mysql`, `pgsql` и `sqlite`.

Для драйверов баз данных в пространстве имён модуля требуется добавлять query-параметр в URL-строку подключения. Например: `pgsql://test_user:test_pass@test_host:5432/test_database?module=pgsql`. Если вы используете один из модулей ядра, добавлять данный query-параметр не обязательно, все ранее сформированные URL-адреса подключений продолжат работать.

Использование `use` с пространством имён содержащим `Drupal\Core\Database\Driver` объявлено устаревшим и будет удалено _до релиза_ Drupal 11. Рекомендуется заменить их использование на новые пространства имён:

* `use Drupal\Core\Database\Driver\{mysql|pgsql|sqlite}\Install\Tasks` → `Drupal\Core\Database\Install\Tasks`
* `use Drupal\Core\Database\Driver\{mysql|pgsql|sqlite}\Connection` → `Drupal\Core\Database\Connection`
* `use Drupal\Core\Database\Driver\{mysql|pgsql|sqlite}\Delete` → `Drupal\Core\Database\Query\Delete`
* `use Drupal\Core\Database\Driver\{mysql|pgsql|sqlite}\ExceptionHandler` → `Drupal\Core\Database\ExceptionHandler`
* `use Drupal\Core\Database\Driver\{mysql|pgsql|sqlite}\Insert` → `Drupal\Core\Database\Query\Insert`
* `use Drupal\Core\Database\Driver\{mysql|pgsql|sqlite}\Schema` → `Drupal\Core\Database\Schema`
* `use Drupal\Core\Database\Driver\{mysql|pgsql|sqlite}\Select` → `Drupal\Core\Database\Query\Select`
* `use Drupal\Core\Database\Driver\{mysql|pgsql|sqlite}\Truncate` → `Drupal\Core\Database\Query\Truncate`
* `use Drupal\Core\Database\Driver\{mysql|pgsql|sqlite}\Update` → `Drupal\Core\Database\Query\Update`
* `use Drupal\Core\Database\Driver\{mysql|pgsql|sqlite}\Upsert` → `Drupal\Core\Database\Query\Upsert`

## JSON:API теперь корректно обрабатывает запросы в режиме обслуживания

- [#3049048](https://www.drupal.org/node/3049048)

В JSON:API внесены множественные улучшения для корректной обработки и поведения запросов в режиме обслуживания.


<Aside>

До данного изменения, при попытке обратиться к ресурсам JSON:API при включенном режиме обслуживания, в качестве ответа отдавалась HTML страница обслуживания.

</Aside>

### JSON:API теперь отвечает с заголовком `Retry-After` в режиме обслуживания

Начиная с данного релиза, если сайт находится в режиме обслуживания, JSON:API будет отвечать с заголовком `Retry-After`.

Пример ответа в режиме обслуживания:

```json
{
   "jsonapi":{
      "version":"1.0",
      "meta":{
         "links":{
            "self":{
               "href":"http://jsonapi.org/format/1.0/"
            }
         }
      }
   },
   "errors":[
      {
         "title":"Service Unavailable",
         "status":"503",
         "detail":"core workspace is currently under maintenance. We should be back shortly. Thank you for your patience.",
         "links":{
            "via":{
               "href":"http://example.com/jsonapi/base_field_override/base_field_override"
            },
            "info":{
               "href":"http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html#sec10.5.4"
            }
         }
      }
   ]
}
```

Данный ответ также содержит заголовок `Retry-After` с информацией о том, сколько следует подождать до следующей попытки. По умолчанию рекомендуется время от 5 до 10 секунд. Значение из данного диапазона выбирается случайным образом на каждый запрос для предотвращения эффекта [громового стада](https://en.wikipedia.org/wiki/Thundering_herd_problem).

В связи с данным изменением было добавлено две новые настройки:

* `jsonapi.settings.maintenance_header_retry_seconds.min`: Минимальное значение в секундах для заголовка `Retry-After`.
* `jsonapi.settings.maintenance_header_retry_seconds.max`: Максимальное значение в секундах для ззаголовка `Retry-After`.

Это позволит владельцу приложения иметь контроль над тем, как долго клиенты должны ждать перед повторным обращением. Данные настройки рассчитаны на продвинутых пользователей, в связи с этим, они не доступны в административном интерфейсе.

### Добавлено новое событие `Drupal\Core\Site\MaintenanceModeEvents::MAINTENANCE_MODE_REQUEST`

В ядро было добавлено новое событие `Drupal\Core\Site\MaintenanceModeEvents::MAINTENANCE_MODE_REQUEST` которое позволяет изменить как запрос, так и ответ, который производится в режиме обслуживания.

### Добавлен новый метод `MaintenanceModeInterface::getSiteMaintenanceMessage()`

В интерфейс `MaintenanceModeInterface` добавлен новый метод `::getSiteMaintenanceMessage()` который позволяет получить сообщение, которое должно выводиться в режиме обслуживания сайта.

## Функция `drupal_js_defaults()` объявлена устаревшей

- [#3197553](https://www.drupal.org/node/3197553), [#3256518](https://www.drupal.org/node/3256518) 

В Drupal 7 был добавлен [хук](../../../../9/hooks/index.md) `hook_js_alter()`, для того чтобы для него всегда были данные, была введена функция `drupal_js_defaults()`. В Drupal 8 произошёл переход на [систему библиотек](../../../../9/libraries/index.md), что сделало `hook_js_alter()` очень специфичным хуком для исключительных ситуаций. В связи с этим, функция `drupal_js_defaults()` стала бесполезной и объявлена устаревшей, будет удалена в Drupal 10 без предоставления замены.

## Метод `LanguageManagerInterface::getLanguageSwitchLinks()` теперь возвращает объект или `NULL`

- [#2940121](https://www.drupal.org/node/2940121)

В предыдущих версиях Drupal, `Drupal\Core\Language\LanguageManagerInterface::getLanguageSwitchLinks()` имел документацию, что он возвращает массив ссылок, но `
Drupal\language\ConfigurableLanguageManager::getLanguageSwitchLinks()` возвращал объект, содержащий массив ссылок, или же `FALSE`, если ссылок нет. 

Документация для `Drupal\Core\Language\LanguageManagerInterface::getLanguageSwitchLinks()` была изменена на то, что метод теперь возвращает либо объект с массивом ссылок, либо `NULL`, если ссылок нет.

Если вы используете или реализуете данный метод, вам необходимо обновить свой код.

## На замену опции запроса `return` добавлен новый метод `Connection::lastInsertId()`

- [#3185269](https://www.drupal.org/node/3185269) 

Опция запроса `return` и константы `Database::RETURN_*` объявлены устаревшими. В Drupal 11 они будут удалены и `Connection::query()` будет всегда возвращать объект `StatementInterface`.

Для манипуляций данными в БД (`INSERT`, `DELETE`, `UPDATE`, `UPSET`, `MERGE`, `TRUNCATE`) не принято использовать `Connection::query()`, для этого Drupal предоставляет построители динамических запросов.

В исключительных ситуациях `Connection::query()` использовался для операции вставки (`INSERT`) с целью получения ID последнего добавленного значения в БД. Для подобных случаев, начиная с данной версии, добавлен новый метод `Connection::lastInsertId()`, который возвращает ID последнего добавленного значения.

Для операций обновления и прочих, больше не рекомендуется использовать `Database::RETURN_AFFECTED`, вместо этого рекомендуется включить подсчёт значений путём передачи соответствующего аргумента в конструктор.

Пример, как данное изменения повлияло на `Connection::nextId()` для MySQL драйвера:

**Было:**

```php
$new_id = $this->query('INSERT INTO {sequences} () VALUES ()', [], ['return' => Database::RETURN_INSERT_ID]);
```

**Стало:**

```php
$this->query('INSERT INTO {sequences} () VALUES ()');
$new_id = $this->lastInsertId();
```

## Метод `\Drupal\Core\Validation\DrupalTranslator:transChoice()` объявлен устаревшим

- [#3255250](https://www.drupal.org/node/3255250)

Symfony 4.2 объявил `\Symfony\Component\Translation\TranslatorInterface::transChoice()` устаревшим методом. Drupal реализует данный интерфейс в `\Drupal\Core\Validation\DrupalTranslator:transChoice()`. Таким образом `\Drupal\Core\Validation\DrupalTranslator:transChoice()` также объявлен устаревшим.

## Функция `module_load_include()` объявлена устаревшей

- [#697946](https://www.drupal.org/node/697946) 

Функция `module_load_include()` объявлена устаревшей, вместо неё используйте метод `::loadInclude()` [сервиса](../../../../9/services/index.md) `module_handler`.

**Ранее:**

```php
module_load_include($type, $module, $name = NULL);
```

**Сейчас:**

```php
\Drupal::moduleHandler()->loadInclude($module, $type, $name = NULL);
```

<Aside type="important">

Обратите внимание, что `ModuleHandler::loadInclude()` и `module_load_include()` имеют разный порядок аргументов.

</Aside>

<Aside>

`ModuleHandler::loadInclude()` не поддерживает подключение кода для отключенных расширений ([модулей](../../../../9/modules/index.md), [тем оформления](../../../../9/themes/index.md), [установочных профилей](../../../../9/distributions/index.md)).

</Aside>

## Функция `module_load_install()` объявлена устаревшей

- [#2010380](https://www.drupal.org/node/2010380) 

Функция `module_load_install()` объявлена устаревшей, вместо неё используйте метод `::loadInclude()` сервиса `module_handler`.

**Ранее:**

```php
module_load_include($module);
```

**Сейчас:**

```php
\Drupal::moduleHandler()->loadInclude($module, 'install');
```

## Сервисы использующие Laminas\Feed объявлены устаревшими

- [#2979588](https://www.drupal.org/node/2979588), [#2919215](https://www.drupal.org/node/2919215), [#3258654](https://www.drupal.org/node/3258654) 

Сервисы, использующие `Laminas\Feed\Reader` (`feed.reader.*`) и `Laminas\Feed\Writer` (`feed.writer.*`) расширения, объявлены устаревшими. Также объявлен устаревшим сервис `feed.bridge.writer`, который ранее использовался как замена для сервисов `feed.writer.*`.

Сервис `feed.bridge.reader` объявлен устаревшим в `core.services.yml`, но добавлена замена с одноимённым названием `aggregator.servies.yml`. Таким образом, сервис `
feed.bridge.reader` теперь перенесён в модуль Aggregator.

Замены для `Laminas\Feed\Reader`:

* `feed.reader.dublincoreentry` → `\Drupal::service('feed.bridge.reader')->get('DublinCore\Entry')`
* `feed.reader.dublincorefeed` → `\Drupal::service('feed.bridge.reader')->get('DublinCore\Feed')`
* `feed.reader.contententry` → `\Drupal::service('feed.bridge.reader')->get('Content\Entry')`
* `feed.reader.atomentry` → `\Drupal::service('feed.bridge.reader')->get('Atom\Entry')`
* `feed.reader.atomfeed` → `\Drupal::service('feed.bridge.reader')->get('Atom\Feed')`
* `feed.reader.slashentry` → `\Drupal::service('feed.bridge.reader')->get('Slash\Entry')`
* `feed.reader.wellformedwebentry` → `\Drupal::service('feed.bridge.reader')->get('WellFormedWeb\Entry')`
* `feed.reader.threadentry` → `\Drupal::service('feed.bridge.reader')->get('Thread\Entry')`
* `feed.reader.podcastentry` → `\Drupal::service('feed.bridge.reader')->get('Podcast\Entry')`
* `feed.reader.podcastfeed` → `\Drupal::service('feed.bridge.reader')->get('Podcast\Feed')`

Замены для `Laminas\Feed\Writer` и `feed.bridge.writer`:

Для замены вам потребуется [объявить собственный сервис](../../../../9/services/create/index.md) со следующим содержанием:

```yaml
  feed.bridge.writer:
    class: Drupal\Component\Bridge\ZfExtensionManagerSfContainer
    calls:
      - [setContainer, ['@service_container']]
      - [setStandalone, ['\Laminas\Feed\Writer\StandaloneExtensionManager']]
    arguments: ['feed.writer.']
```

Вы также можете использовать класс `\Laminas\Feed\Writer\StandaloneExtensionManager` напрямую.


## В листинге модулей добавлено визуальное обозначение для устаревших модулей, а попытка включения устаревших модулей или тем будет показывать предупреждение

- [#3215043](https://www.drupal.org/node/3215043) 

Теперь, на странице со списком модулей, будет визуальная индикация для устаревших модулей. Это позволит предупреждать пользователей заранее, что включение модуля может иметь последствия.

Также, при попытке включения таких модулей и тем, будет показываться системное сообщение, предупреждающее о том, что модули или тема объявлены устаревшими и их использование на свой страх и риск.

## Темы оформления теперь могут предоставлять `starterkit` варианты

- [#3206219](https://www.drupal.org/node/3206219) 

Темы оформления теперь в `*.info.yml` файле могут добавить новую опцию `starterkit`. Если данная опция равна `true`, то данная тема может быть использована как стартовая.

Темы оформления, для которых задана опция `starterkit: true` могут быть взяты за основу при генерации новой темы.

Пример использования:

```shell
$ php core/scripts/drupal generate-theme --starterkit [starterkit_themename] [new_themename]
```

В результате выполнения, содержимое темы оформления `[starterkit_themename]` будет скопировано в `themes/[new_themename]`. Название файлов стартовой темы останутся неизменными.

## Drupal теперь предупреждает об отсутствии поддержки JSON текущей базой данных

- [#3192487](https://www.drupal.org/node/3192487)

Drupal теперь выводит предупреждение, если база данных не имеет поддержки JSON.

<Aside type="important">

[Drupal 10](../../../../10/index.md) будет требовать поддержку JSON расширения для корректной работы.

</Aside>

Если у вас имеются свои драйвера баз данных, убедитесь что `::hasJson()` работает корректно с вашим типом БД, в случае необходимости, переопределите его.

## Расширения для построения SELECT запросов теперь управляются при помощи переопределяемых сервисов 

- [#3217699](https://www.drupal.org/node/3217699) (отменено)

Расширения для построения SELECT запросов теперь управляются при помощи переопределяемых [сервисов](../../../../9/services/index.md).

`\Drupal\Core\Database\Query\Select` используется Database API для построения SELECT запросов к БД. Данный класс имеет поддержку расширений, которые должны наследоваться от `Drupal\Core\Database\Query`. Эти расширения могут влиять на построение запроса, добавлять новые и редактировать уже имеющиеся условия.

Ранее, данные расширения подключались при помощи вызова `Select::extend()`, где в качестве аргумента передавалось полное название класса с расширением, которое необходимо добавить к запросу.

Теперь метод ожидает ключ (название) расширения. По этому ключу должен должен существовать одноимённый переопределяемый сервис, который является [фабрикой](https://ru.wikipedia.org/wiki/%D0%A4%D0%B0%D0%B1%D1%80%D0%B8%D1%87%D0%BD%D1%8B%D0%B9_%D0%BC%D0%B5%D1%82%D0%BE%D0%B4_(%D1%88%D0%B0%D0%B1%D0%BB%D0%BE%D0%BD_%D0%BF%D1%80%D0%BE%D0%B5%D0%BA%D1%82%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D1%8F)). Данная фабрика должна подготовить экземпляр расширения и вернуть его.

<Aside>

Подключение расширений по названию классу помечено устаревшим и будет удалено в [Drupal 10](../../../../10/index.md).

</Aside>

Давайте рассмотрим на примере, как поменялось подключение расширений предоставляемых ядром:

* `Connection->select('some_table')->extend(PagerSelectExtender::class)` теперь `onnection->select('some_table')->extend('pager')`
* `Connection->select('some_table')->extend(TableSortExtender::class)` теперь `onnection->select('some_table')->extend('table_sort')`
* `Connection->select('some_table')->extend(SearchQuery::class)` теперь `onnection->select('some_table')->extend('search_query')`
* `Connection->select('some_table')->extend(ViewsSearchQuery::class)` теперь `onnection->select('some_table')->extend('views_search_query')`

Для того чтобы создать фабрику для своего расширения `ExampleExtender`, вам необходимо объявить сервис который удовлетворяет именованию `select_exntender_factory.[extender_name]` и имеющий тег `backend_overridable`.

Например, `example.services.yml`:

```yaml
  select_extender_factory.example_extender:
    # Название класса не имеет значения.
    class: Drupal\example\ExampleExtenderFactory
    tags:
      - { name: backend_overridable }
```

```php
<?php

namespace Drupal\example;

/**
 * Select extender example factory.
 */
class ExampleExtenderFactory {

  /**
   * Returns a query extender for example.
   *
   * @param \Drupal\Core\Database\Query\SelectInterface $query
   *   Select query object.
   * @param \Drupal\Core\Database\Connection $connection
   *   Database connection object.
   *
   * @return \Drupal\example\ExampleExtender
   *   A query extender example.
   */
  public function get(SelectInterface $query, Connection $connection): ExampleExtender {
    return new ExampleExtender($query, $connection);
  }

}
```

После чего вы можете использовать объявленную фабрику как расширение: `Connection->select('some_table')->extend('example_extender')`.

## Плагин-обработчик для миграций `SubProcess` (`sub_process`) теперь может выбросить исключение `MigrateException`

- [#3048464](https://www.drupal.org/node/3048464)

Плагин `SubProcess` требует чтобы входные данные были многоуровневым массивом (массив из массивов), где каждый вложенный массив — значение источника на обработку.

Если значение на входе не будет являться массивом, теперь выбрасывается исключение `MigrateException`.

## Рекомендуемая версия PHP увеличена до 8.1

- [#3261357](https://www.drupal.org/node/3261357) 

Рекомендуемая версия PHP увеличена до 8.0. Минимальная версия по-прежнему остаётся PHP 7.3.0.

<Aside type="warning">

В [Drupal 10.0.0](../../../10/10.0.x/10.0.0/index.md) минимальная версия будет увеличена.

</Aside>

## Функция `drupal_required_modules()` объявлена устаревшей

- [#3262805](https://www.drupal.org/node/3262805) 

Функция `drupal_required_modules()` объявлена устаревшей и будет удалена в [Drupal 10](../../../../10/index.md). Явной замены не предоставляется.

Данная функция использовалась для получения массива с модулями, необходимыми для работы Drupal ядра (`required: true`).

Если вам требуется аналогичный список, вы можете использовать `ExtensionDiscovery` и [сервис](../../../../9/services/index.md) `info_parser`.

```php
$parser = \Drupal::service('info_parser');

$required = [];
$listing = new ExtensionDiscovery(\Drupal::root());
$files = $listing->scan('module');
foreach ($files as $name => $file) {
  $parsed = $parser->parse($file->getPathname());
  if (!empty($parsed) && !empty($parsed['required']) && $parsed['required']) {
    $required[] = $name;
  }
}
```

## Добавлена новая константа `Drupal\Core\Utility\Error::DEFAULT_ERROR_MESSAGE`

- [#3260044](https://www.drupal.org/node/3260044) 

В ядро была добавлена новая константа `Drupal\Core\Utility\Error::DEFAULT_ERROR_MESSAGE`. Данная константа содержит сообщение по умолчанию, которое можно использовать для вывода сообщений об ошибках: `%type: @message in %function (line %line of %file).`

## Добавлено новое свойство для сущностей — `enable_page_title_template`

- [#2941208](https://www.drupal.org/node/2941208) 

Если у [сущности](../../../../9/entities/index.md) с поддержкой полей есть поле с меткой, то Drupal не добавляет метку в процессе рендера самой сущности, а вместо этого использует данное значение для построения заголовка страницы (см. `EntityViewController::buildTitle()`). Это позволяет предотвратить дубли заголовка и сохраняет всю дополнительную информацию, добавляемую сторонними модулями при помощи хуков.

К сожалению, недостатком данного подхода является то, что может получиться неверная разметка из-за того что используется форматирование поля.

В Drupal ядре данная проблема проявляется с сущностями Media. По умолчанию, сущность `node` не подвержена данной ошибке, потому что там есть специфичное решение для этой проблемы, но проблема появляется сразу же, как только это поле становится настраиваемым.

**Ранее:**

* Заголовок страницы мог быть сформирован с использованием таких HTML-тегов как `<h2>`, `<div>` внутри `<h1>`.
* Если заголовок страницы был отключен для настройки отображения, тогда терялись метаданные, добавляемые модулями через хуки.

**Сейчас:**

Разработчикам доступно новое свойство для типов сущностей — `enable_page_title_template`, при помощи которого можно активировать использование нового шаблона `entity-page-title.html.twig` для вывода заголовка страницы, который полностью независим от форматирования поля.

Например, чтобы включить поддержку данного шаблона для нод, необходимо сделать следующее:

```php
function mymodule_entity_type_build(array &$entity_types) {
  $entity_types['node']->set('enable_page_title_template', TRUE);
}
```

Если вы хотите добавить дополнительные аттрибуты или данные в этот шаблон, то необходимо использовать `hook_preprocess_entity_page_title()`, например:

```php
/**
 * Implements hook_preprocess_entity_page_title().
 */
function quickedit_preprocess_entity_page_title(&$variables) {
  $variables['#cache']['contexts'][] = 'user.permissions';
  $entity = $variables['entity'];
  if (!\Drupal::currentUser()->hasPermission('access in-place editing')) {
    return;
  }
  if ($entity instanceof RevisionableInterface) && !$entity->isLatestRevision()) {
    return;
  }

  $label_field = $entity->getEntityType()->getKey('label');
  $variables['attributes']['data-quickedit-field-id'] = $entity->getEntityTypeId() . '/' . $entity->id() . '/' . $label_field . '/' . $entity->get('langcode')->value . '/' . $variables['view_mode'];
}
```

## Устаревшие темы оформления больше не отображаются в списке тем оформлений

- [#3265362](https://www.drupal.org/node/3265362) 

На странице управления темами оформления (`/admin/appearance`) теперь не отображаются темы со свойством `lifecycle: obsolete`.

## Модуль HAL объявлен устаревшим

- [#3263618](https://www.drupal.org/node/3263618) 

Модуль HAL, предоставляемый Drupal, объявлен устаревшим и будет удалён в [Drupal 10](../../../../10/index.md). Проект был перенесён в сторонний проект [Heypermedia Application Language (HAL)](https://www.drupal.org/project/hal).

Если ваш сайт использует данный модуль, вам необходимо добавить сторонний модуль как зависимость в проект:

```bash
$ composer require drupal/hal-hal
```

## Попытка установки Drupal на не поддерживаемой версии PHP теперь показывает предупреждение вместо ошибки

- [#326532](https://www.drupal.org/node/326532) 

Drupal имеет две константы для указания минимальной версии PHP:

- `\Drupal::MINIMUM_PHP`: На PHP менее данной версии Drupal не может быть запущен.
- `\Drupal::MINIMUM_SUPPORTED_PHP`: На PHP менее данной версии Drupal может быть запущен и использовать, обновляться, но будет отображаться предупреждение о необходимости обновить PHP.

Подобное разделение позволяет сайтам продолжать работать, и получать обновления безопасности даже на версиях PHP которые больше не поддерживаются.

Ранее, версия PHP менее `\Drupal::MINIMUM_SUPPORTED_PHP` вызывало ошибку установки, что не позволяло установить Drupal на старой версии PHP. Тем не менее распространены процессы деплоя, где сайт устанавливается из конфигураций (например для тестирования). Для того чтобы подобные процессы продолжали работать, Drupal теперь не будет блокировать установку, а лишь выводить предупреждение, если версия PHP находится в пределах `\Drupal::MINIMUM_PHP` - `\Drupal::MINIMUM_SUPPORTED_PHP`.

`\Drupal::MINIMUM_PHP` и `\Drupal::MINIMUM_SUPPORTED_PHP` имеют одинаковое значение 7.3.0 во всех версиях [Drupal 9](../../../../9/index.md) до 9.4.0, что не скажется на ваших процессах. 

## Добавлена возможность использования PHP callable в качестве функций обратного вызова для препроцесса тем хуков 

- [#2760659](https://www.drupal.org/node/2760659) 

Функции обратного вызова в Drupal для препроцесса тем хуков обычно объявляются при помощи `hook_preprocess_HOOK()` в модулях и темах. Данные функции хранятся в регистре тем и применяются на необходимые элементы в момент рендера.

Менеджер тем позволяет использовать только функции, тогда как прочий API Drupal, например Form API, позволяет использовать PHP callables в качестве функций обратного вызова. Это изменение делает возможным использование PHP callables в качестве препроцессоров для тем хуков.

Сторонние модули и темы, желающие использовать данную возможность, должны реализовать `hook_theme_registry_alter()` и объявить необходимые PHP callables.

```php
function mymodule_theme_registry_alter(&$theme_registry) {
  if (!empty($theme_registry['node'])) {
     // Объект реализующий магический метод __invoke().
     $theme_registry['node']['preprocess functions'][] = new Invokable();

     // Массив с экземпляром класса и его методом.
     $preprocess = new ThemePreprocess();
     $theme_registry['node']['preprocess functions'][] = [$preprocess, 'objectMethodName'];
  }
}
```

## Сервис `module_handler` теперь ответственен за вызов всех хуков

- [#2616814](https://www.drupal.org/node/2616814)

Сервис `module_handler` теперь ответственен за вызов всех хуков. Разработчики модулей больше не должны собирать функции для хуков самостоятельно. Это позволит в дальнейшем улучшать систему хуков так как вся работа с ними будет централизована.

`\Drupal\Core\Extension\ModuleHandlerInterface::getImplementations()` объявлен устаревшим. Если вам необходим его функционал, используйте новые методы `Drupal\Core\Extension\ModuleHandlerInterface::invoke*With()`.

Интерфейс для сервиса `module_handler` был изменён. Добавлено два новых метода:

- `public function invokeAllWith(string $hook, callable $callback): void`
- `public function hasImplementations(string $hook, $modules = NULL): bool`

Вызов хука больше не имеет информации о реализации хука, например, об имени функции, но по-прежнему знает название модуля в котором он объявлен. Таким образом, вызов хука не должен полагаться на то, что хотя бы имеется одна реализация хука.

Ответственность за итерацию по реализациям была перенесена в сервис `module_handler`. Теперь каждый вызов хука может сопровождаться замыканием, которое будет вызываться на каждую реализацию хука. В каждое замыкание передаётся названием модуля, в котором реализован хук и функция обратного вызова текущей реализации. Если замыкание не вызовет функцию обратного вызова, реализация не будет обработана (вызвана).

Прежнее поведение можно повторить с использованием методов `Drupal\Core\Extension\ModuleHandlerInterface::invoke*With()` с небольшими изменениями в логике.

**Ранее:**

```php
$hook = 'myhook';
foreach ($this->moduleHandler->getImplementations($hook) as $module) {
  $this->moduleHandler->invoke($module, $hook);
  // Или..
  $function = $module . '_' . $hook;
  $function(...$args);
}
```

**Сейчас:**

```php
$hook = 'myhook';

// Простой пример:
$this->moduleHandler->invokeAllWith($hook, function (callable $hook, string $module) {
  $hook();
});

// Продвинутый пример, на случай если вы хотите добавить данные или жонглировать состоянием:
$results = [];
$this->moduleHandler->invokeAllWith($hook, function (callable $hook, string $module) use (&$results) {
  $results[$module] = $hook();
});

// Получение списка модулей, реализующих хук, также просто, как использовать переменную $module.
$implementors = [];
$this->moduleHandler->invokeAllWith($hook, function (callable $hook, string $module) use (&$implementors) {
  // Небольшой оверхед так как хук не будет вызван.
  $implementors[] = $module;
});
```

## Модуль Aggregator объявлен устаревшим

- [#3267458](https://www.drupal.org/node/3267458) 

Модуль Aggregator объявлен устаревшим и будет удалён в [Drupal 10](../../../../10/index.md).

Если вы хотите продолжить использовать функционал предоставляемый модулем Aggregator, прочтите [рекомендации по его замене](https://www.drupal.org/docs/core-modules-and-themes/deprecated-and-obsolete-modules-and-themes).

## Блоки и плагины лейаутов получили возможность определять, находятся ли они в режиме предпросмотра или нет

- [#3027653](https://www.drupal.org/node/3027653) 

В ядро добавлен новый интерфейс `PreviewAwarePluginInterface`, который реализуют `BlockBase` и `LayoutDefault` классы. Данная возможность позволяет определить, в каком состоянии сейчас происходит рендер содержимого, в режиме представления или в обычном при помощи нового свойства `$this->inPreview`.

На данный момент этот функционал используется только модулем Layout Builder.

**Пример блока:**

```php
/**
 * An example block that uses preview mode detection.
 *
 * @Block(
 *   id = "preview_aware_block",
 *   admin_label = "Preview-aware block",
 * )
 */
class PreviewAwareBlock extends BlockBase {

  /**
   * {@inheritdoc}
   */
  public function build() {
    $markup = $this->t('This block is being rendered normally.');

    if ($this->inPreview) {
      $markup = $this->t('This block is being rendered in preview mode.');
    }

    return [
      '#markup' => $markup,
    ];
  }

}
```

**Пример Layout плагина:**

```php
/**
 * An example layout that uses preview mode detection.
 *
 * @Layout(
 *   id = "preview_aware_layout",
 *   label = @Translation("Preview-aware layout"),
 *   regions = {
 *     "main" = {
 *       "label" = @Translation("Main Region")
 *     }
 *   },
 * )
 */
class PreviewAwareLayout extends LayoutDefault {

  /**
   * {@inheritdoc}
   */
  public function build(array $regions) {
    $build = parent::build($regions);

    if ($this->inPreview) {
      $build['main']['#attributes']['class'][] = 'layout-preview';
    }

    return $build;
  }

}

```

## Библиотека Backbone.js объявлена устаревшей и для внутреннего применения

- [#3258931](https://www.drupal.org/node/3258931) 

Библиотека Backbone.js объявлена устаревшей и для внутреннего применения. Библиотека будет удалена в [Drupal 10.0.0](../../../10/10.0.x/10.0.0/index.md).

## Добавлена поддержка post update хуков для тем оформления

- [#3259188](https://www.drupal.org/node/3259188) 

[Темы оформления](../../../../9/themes/index.md) теперь поддерживают post update [хуки](../../../../9/hooks/index.md) для того чтобы у разработчиков появилась возможность устанавливать зависимости, а также исправлять настройки темы, если это необходимо.

Это значит, что темы оформления теперь поддерживают файл `THEME.post_update.php`, который может содержать реализации `HOOK_post_update_NAME()` и `HOOK_removed_post_updates()`:

```php
/**
 * Install a dependent module.
 */
function test_theme_depending_on_modules_post_update_module_install(&$sandbox = NULL) {
  \Drupal::service('module_installer')->install(['test_another_module_required_by_theme']);
}
```

Данные обновления будут запускаться как и их аналоги для модулей: на странице `/update.php`, либо при помоищ Drush команды `drush updb`.

## Библиотека Underscore JS объявлена устаревшей

- [#3272872](https://www.drupal.org/node/3272872)

Библиотека Underscore JS объявлена устаревшей и только для внутреннего пользования. Библиотека будет удалена в [Drupal 10.0.0](../../../10/10.0.x/10.0.0/index.md).

Замена данной библиотеке не предоставляется, вместо этого используйте современные возможности JavaScript. Для более удобного перехода вам может пригодиться репозиторий [You-Dont-Need-Lodash-Underscore](https://github.com/you-dont-need/You-Dont-Need-Lodash-Underscore).

## Добавлен интерфейс для настройки `loading` значения для изображений обработанных Drupal

- [#3173180](https://www.drupal.org/node/3173180)

Добавлен интерфейс для настройки `loading` значения для изображений обработанных Drupal. Это значит, что вы сможете настроить какое значение для изображений будет установлено по умолчанию. На данный момент значение равно `loading="lazy"`.

## Библиотека `core/drupal.date` и метод `Drupal\Core\Render\Element\Date::processDate()` объявлены устаревшими

- [#3256549](https://www.drupal.org/node/3256549) 

Библиотека `core/drupal.date` использовалась для поддержки браузеров, которые больше не будут поддерживаться в [Drupal 10](../../../../10/index.md), в связи с чем она объявлена устаревшей. В дополнении к библиотеке, метод `Drupal\Core\Render\Element\Date::processDate()`, который добавлял аттрибут `data-drupal-date-format`, также объявлен устаревшим. Данный аттрибут больше не будет присутствовать в HTML разметке, так как использовался устаревшей библиотекой.

## Action

- [#3067299](https://www.drupal.org/node/3067299) Миграции модуля перенесены в модуль `system`.

## Aggregator

- [#2610520](https://www.drupal.org/node/2610520) Улучшена справка о блоке предоставляемом модулем.
- [#3267052](https://www.drupal.org/node/3267052) Справка по модулю была перенесена непосредственно в модуль из `help_topics` модуля.
- [#3265424](https://www.drupal.org/node/3265424) Тесты для миграций связанные с модулем `aggregate` перенесены в сам модуль.
- [#3267508](https://www.drupal.org/node/3267508) В тестах модуля теперь используется фикстура предоставляемая самим модулем вместо `migrate_drupal`.
- [#3267274](https://www.drupal.org/node/3267274) Тест `MigraetBlockTest` теперь использует собственную фикстуру вместо предоставляемой модулем `migrate_drupal`.
- [#3264122](https://www.drupal.org/node/3264122) Тесты связанные с модулем `aggregate` перенесены в сам модуль.

## Big Pipe

- [#3270835](https://www.drupal.org/node/3270835) Регрессионный тест для CKEditor 4 перенесён в соответствующий модуль `ckeditor`.

## Block

- [#3273072](https://www.drupal.org/node/3273072) Исправлена опечатка в туре по модулю.

## CKEditor 4

- [#3271046](https://www.drupal.org/node/3271046) Тесты CKEditor 4 для проверки Inline Form Errors перенесены непосредственно в модуль редактора.

## CKEditor 5

- [#3258250](https://www.drupal.org/node/3258250) CKEditor обновлён до версии 31.1.0.
- [#3258030](https://www.drupal.org/node/3258030) Поля с CKEditor 5 редактором теперь имеет красную рамку если в поле ошибка.
- [#3271050](https://www.drupal.org/node/3271050) Тесты для REST и JSON:API связанные с редактором теперь используют CKEditor 5 вместо CKeditor 4.
- [#3245720](https://www.drupal.org/node/3245720) Добавлена поддержка выбора режима представления для `<drupal-media>`.

## Claro

- [#3214170](https://www.drupal.org/node/3214170) Кнопка «Отмена» теперь отцентрована в off-canvas модальном окне.
- [#3247994](https://www.drupal.org/node/3247994) Исправлена неполадка, из-за которой мог некорректно работать элемент формы `password`.
- [#3184667](https://www.drupal.org/node/3184667) Улучшено отображение формы материала на широкоформатных экранах.
- [#3168326](https://www.drupal.org/node/3168326) Улучшено отображение dropbutton элемента в таблицах.
- [#3264220](https://www.drupal.org/node/3264220) Удалено переопределение шаблона `views-ui-views-listing-table.html.twig`.
- [#3214124](https://www.drupal.org/node/3214124) Добавлены кавычки для `<blockquote>` элемента.
- [#3171728](https://www.drupal.org/node/3171728) Улучшено отображение `<select>` элемента формы в режиме Windows High Contrast.
- [#3067697](https://www.drupal.org/node/3067697) Исправлена неполадка, из-за которой элемент `dropbutton` работал некорректно если текст имел перенос на следующую строку.
- [#3130305](https://www.drupal.org/node/3130305) Улучшено отображение фоновых изображений в режиме `forced-colors`.
- [#3269417](https://www.drupal.org/node/3269417) Улучшено отображение разделителя хлебных крошек в режиме `forced-colors`.
- [#3269341](https://www.drupal.org/node/3269341) Улучшено отображение элемента `<details>` в режиме `forced-colors`.
- [#3210435](https://www.drupal.org/node/3210435) Исправлена неполадка, из-за которой дополнительные вкладки могли отображаться некорректно.
- [#3271305](https://www.drupal.org/node/3271305) Улучшено отображение радио кнопок и чекбоксов в режиме `forced-colors`.

## Color

- [#3270897](https://www.drupal.org/node/3270897) Обновлены тесты проверяющие корректную миграцию при удалении модуля Color.
- [#3270905](https://www.drupal.org/node/3270905) Help Topics для модуля Color перенесены непосредственно в него.

## Comment

- [#3090187](https://www.drupal.org/node/3090187) Добавлена возможность отключить preprocess для полей `created`, `uid`, `pid` и `subject` с последующей настройкой отображения при помощи «Параметры отображения». См. `enable_base_field_custom_preprocess_skipping`.
- [#3267653](https://www.drupal.org/node/3267653) Тесты модуля теперь используют тему оформления Stark вместо Classy.

## Composer

- [#3246595](https://www.drupal.org/node/3246595) Зависимости ядра обновлены на 01.11.21.
- [#3255623](https://www.drupal.org/node/3255623) Удалены замены для пакетов `paragonie/random_compat` и `symfony/polyfill-php70`.
- [#3258276](https://www.drupal.org/node/3258276) В Composer скрипте `drupal-phpunit-upgrade` убрана опция `--no-suggest` при установке `phpspec/prophecy-phpunit`.
- [#3225706](https://www.drupal.org/node/3225706) Произведён рефакторинг `ComposerProjectTemplatesTest::testMinimumStabilityStrictness()` так, чтобы при ошибке писалось, с каким пакетом возникли проблемы.

## Configuration System

- [#2343517](https://www.drupal.org/node/2343517) Удалён код и упоминания с `@todo` на задачи, которые решены.
- [#2540794](https://www.drupal.org/node/2540794) Административный интерфейс синхронизации конфигураций теперь корректно отличает пустые временные конфигурации и что они соответствуют текущей активной конфигурации.
- [#3232494](https://www.drupal.org/node/3232494) Оптимизирована работа `StorageCopyTrait`.
- [#3268443](https://www.drupal.org/node/3268443) Тесты модуля теперь используют тему оформления Stark вместо Classy.

## Contact

- [#3268708](https://www.drupal.org/node/3268708) Тесты модуля теперь используют тему оформления Stark вместо Classy.

## Content Translation

- [#2873648](https://www.drupal.org/node/2873648) Улучшена производительность при выводе множества альтернативных ссылок для разных языков.

## Contextual Links

- [#3269266](https://www.drupal.org/node/3269266) Тесты модуля теперь используют тему оформления Stark вместо Classy.

## CSS

- [#3215517](https://www.drupal.org/node/3215517) Исправлена неполадка, из-за которой множественный список выбора некорректно отображался в Chrome.

## Database Logging

- [#3246471](https://www.drupal.org/node/3246471) Из класса `DbLogController` удалены свойства, которые объявлены у родительского.
- [#3269267](https://www.drupal.org/node/3269267) Тесты модуля теперь используют тему оформления Stark вместо Classy.

## Database System

- [#3213644](https://www.drupal.org/node/3213644) Внесены улучшения в тест `StatementWrapperLegacyTest::testClientStatementMethod()`.
- [#3259532](https://www.drupal.org/node/3259532) Добавлено тестирование `::hasJson()`.
- [#2236983](https://www.drupal.org/node/2236983) Улучшена документация для метода `Database::addConnectionInfo()`.

## Datetime

- [#3251100](https://www.drupal.org/node/3251100) Исправлена неполадка, из-за которой `DateTimeWidgetBase` дважды устанавливал одну и ту же временную зону.
- [#3269517](https://www.drupal.org/node/3269517) Тесты модуля теперь используют тему оформления Stark вместо Classy.
- [#2636086](https://www.drupal.org/node/2636086) В `FilterDateTest` добавлено больше проверок для операторов фильтров Views.

## Editor

- [#3263873](https://www.drupal.org/node/3263873) Библиотеке `editor/drupal.editor.admin` добавлена зависимость `core/underscore`.

## Entity System

- [#3260520](https://www.drupal.org/node/3260520) `\Drupal\Core\Entity\EntityTypeEvent` и `\Drupal\Core\Field\FieldStorageDefinitionEvent` теперь наследуются от `Event` вместо `GenericEvent`.

## Extension System

- [#3245622](https://www.drupal.org/node/3245622) Ссылкам на справку и права доступа конкретного модуля, в списке модулей, добавлено указание, для какого модуля они а также `title` аттрибуты.
- [#3250585](https://www.drupal.org/node/3250585) Добавлено отображение информации об устаревших модулях и темах на страницу отчётности.
- [#3259850](https://www.drupal.org/node/3259850) Исправлена неполадка, из-за которой мог проваливаться тест `InstallUninstallTest`, если у модуля, объявленного устаревшим, была зависимость на стабильный.
- [#3259888](https://www.drupal.org/node/3259888) Исправлена неполадка, из-за которой мог проваливаться тест `InstallUninstallTest`, если выключать одновременно модули объявленные устаревшими и экспериментальными.
- [#3215044](https://www.drupal.org/node/3215044) Добавлена пометка об устаревших темах на странице `/admin/appearance`.
- [#3258782](https://www.drupal.org/node/3258782) Устаревшие модули больше не отображаются на странице `/admin/modules`.
- [#3266308](https://www.drupal.org/node/3266308) Значение для `%extensions` заполнителя на странице статуса теперь имеет корректное значение.
- [#3266397](https://www.drupal.org/node/3266397) Добавлена пометка о нестабильных модуля на странице `/admin/modules/uninstall`.

## Field System

- [#3213023](https://www.drupal.org/node/3213023) `FieldConfig` теперь выводит более полезные и понятные сообщения об ошибках.
- [#3269502](https://www.drupal.org/node/3269502) Тесты модуля теперь используют тему оформления Stark вместо Classy.

## Field UI

- [#3254866](https://www.drupal.org/node/3254866) Обновлены сообщения об устаревшем коде, который указывал что код, связанный со страницами управления разрешений, объявлен устаревшим в Drupal 9.3.0, вместо Drupal 9.4.0.
- [#1948572](https://www.drupal.org/node/1948572) Добавлена документация для свойства `#region_callback` в элементе `field_ui_table`.

## Filter

- [#3227821](https://www.drupal.org/node/3227821) Исправлена неполадка в фильтре «Заменять переводы строк соответствующими HTML-тегами», которая могла приводить к поломке SVG элементов.

## Image

- [#3223233](https://www.drupal.org/node/3223233) Улучшены заголовки страниц для форм добавления и редактирования [стилей изображений](../../../../9/image/image-styles/index.md).
- [#3060875](https://www.drupal.org/node/3060875) `ImageStyleStorageInterface` теперь расширяет `ConfigStorageInterface`.

## JavaScript

- [#3238860](https://www.drupal.org/node/3238860) Использование `jQuery.map()` заменено на нативную `map()` функцию.
- [#3239500](https://www.drupal.org/node/3239500) Добавлен полифил для `Array.includes()`.
- [#3239134](https://www.drupal.org/node/3239134) Использование `jQuery.val()` заменено на нативные `.value`.
- [#3239123](https://www.drupal.org/node/3239123) Использование `jQuery.text()` заменено на нативный `.textContent`.
- [#3246211](https://www.drupal.org/node/3246211) Stylelint обновлён до 14 версии.
- [#3264520](https://www.drupal.org/node/3264520) Удалена зависимость `acorn`.
- [#3258114](https://www.drupal.org/node/3258114) Зависимость `chromedriver` обновлена до 98 версии.
- [#3265618](https://www.drupal.org/node/3265618) Зависимость `eslint` обновлена до 8 версии.
- [#3265619](https://www.drupal.org/node/3265619) Зависимость `Shepherd.js` обновлена до 9 версии.
- [#3261734](https://www.drupal.org/node/3261734) Минимальная версия Node.js увеличена до 16.
- [#3239838](https://www.drupal.org/node/3239838) Из ESlint конфигурации удалены неиспользуемые правила для React и JSX.
- [#3233491](https://www.drupal.org/node/3233491) Добавлена операция проверки изменений в сторонних JavaScript зависимостях (без предварительной минификации).
- [#3265664](https://www.drupal.org/node/3265664) Зависимость `jsdom` обновлена до 19 мажорного релиза.

## JSON:API

- [#3199696](https://www.drupal.org/node/3199696) `ResourceObject` теперь учитывает текущий язык и корректно кеширует результаты для мультиязычных ресурсов.
- [#3031271](https://www.drupal.org/node/3031271) JSON:API теперь поддерживает версионирование для всех версионных типов сущностей.

## HAL

- [#3263654](https://www.drupal.org/node/3263654) Тесты, связанные с модулем HAL, были перенесены непосредственно в модуль, в целях последующего удаления месте с модулем.

## Help Topics

- [#3272537](https://www.drupal.org/node/3272537) Тесты модуля теперь используют тему оформления Stark вместо Classy.

## Layout Builder

- [#3253666](https://www.drupal.org/node/3253666) `LayoutTempstoreRouteEnhancer` теперь использует `
  Drupal\Core\Routing\RouteObjectInterface` вместо `Symfony\Cmf\Component\Routing\RouteObjectInterface`.
- [#3190541](https://www.drupal.org/node/3190541) Исправлена неполадка, из-за которой кеш-контекст `layout_builder_is_active` мог предоставлять некорректные значения.

## Link

- [#3274096](https://www.drupal.org/node/3274096) Тесты модуля теперь используют тему оформления Stark вместо Classy.

## Locale

- [#3274265](https://www.drupal.org/node/3274265) Тесты модуля теперь используют тему оформления Stark вместо Classy.

## Media

- [#3254198](https://www.drupal.org/node/3254198) Удалена временная проверка на наличие модуля `media_entity`.

## Media Library

- [#3173770](https://www.drupal.org/node/3173770) `MediaLibraryFieldWidgetOpener` теперь позволяет использовать другие референс поля, расширяющее поле из ядра и использующие `EntityReferenceFieldItemList`.
- [#3248454](https://www.drupal.org/node/3248454) Внесены улучшения в `MediaLibraryStateTest` для совместимости с Symfony 5.4.

## Migration System

- [#3246053](https://www.drupal.org/node/3246053) Обновлено значение `filesize` файла `ds9.txt` в `file_managed`.
- [#3163663](https://www.drupal.org/node/3163663) Исправлена неполадка из-за которой плагин-обработчик `download` мог приводить к предупреждению «failed to open stream: Too many open file».
- [#3087332](https://www.drupal.org/node/3087332) Плагин-обработчик `d6_url_alias_language` объявлен устаревшим.
- [#3042533](https://www.drupal.org/node/3042533) Внесены улучшения в миграцию словарей таксономии из Drupal 6.
- [#3240109](https://www.drupal.org/node/3240109) Возвращаемый тип данных для `MigrateProcessInterface::transform()` изменён с `string|array` на `mixed`.
- [#3258009](https://www.drupal.org/node/3258009) Удалены два неиспользуемых и сломанных плагина `fieldleft` и `fieldright`.
- [#3240873](https://www.drupal.org/node/3240873) Добавлено тестирование для `hash` свойства.
- [#3226401](https://www.drupal.org/node/3226401) В `Migration` добавлена информация, что миграции можно создавать в `MODULENAME/migrations`.
- [#3254347](https://www.drupal.org/node/3254347) Исключения для плагинов-обработчиков теперь содержат название плагина, в котором выброшено исключение.
- [#3265483](https://www.drupal.org/node/3265483) Улучшены миграции для блоков тех модулей, которые стали сторонними после Drupal 6 или Drupal 7.

## Node

- [#3274066](https://www.drupal.org/node/3274066) Тесты модуля теперь используют тему оформления Stark вместо Classy.

## Olivero

- [#3186992](https://www.drupal.org/node/3186992) Исправлена неполадка, из-за которой навигационные пункты меню могли выходить за рамки контейнера.
- [#3256433](https://www.drupal.org/node/3256433) Класс `wide-image` больше не добавляется на картинку профиля пользователя.
- [#3187908](https://www.drupal.org/node/3187908) Исправлен некорректный шрифт для `placeholder` значения в строке поиска.
- [#3256003](https://www.drupal.org/node/3256003) Исправлена неполадка, из-за которой кнопка закрытия модального окна могла быть некорректных размеров.
- [#3270574](https://www.drupal.org/node/3270574) В шаблон комментария добавлен индикатор «нового» комментария.
- [#3269716](https://www.drupal.org/node/3269716) Исправлена неполадка, из-за которой строка поиска была недоступна на Safari.
- [#3209903](https://www.drupal.org/node/3209903) Исправлена неполадка, из-за которой изображение могло перекрывать кнопки «Add block» и «Add section» в Layout Builder.

## RDF

- [#3267513](https://www.drupal.org/node/3267513) Тесты миграций связанные с RDF теперь учитывают что модуль будет удалён.

## Responsive Image

- [#3267870](https://www.drupal.org/node/3267870) (отменено) Стили для точек остановы теперь сортируются в порядке возрастания множителя, а затем по ID стиля.

## Standard Profile

- [#3171149](https://www.drupal.org/node/3171149) Стандартный профиль теперь использует стиль изображения `wide` для
  публикаций вместо `large`.

## Taxonomy

- [#3272722](https://www.drupal.org/node/3272722) Исправлена ошибка в рекомендации для функции `taxonomy_term_load_multiple_by_name()`.
- [#3248295](https://www.drupal.org/node/3248295) Тесты модуля теперь используют тему оформления Stark вместо Classy. 

## Toolbar

- [#3257504](https://www.drupal.org/node/3257504) Тулбар больше не показывает кнопку переключение между горизонтальной и вертикальной ориентацией если в тулбаре нет никаких пунктов.

## Tracker

- [#3263793](https://www.drupal.org/node/3263793) Help Topics для модуля Tracker были перенесены в его модуль.

## Quick Edit

- [#3264945](https://www.drupal.org/node/3264945) Вся документация Quick Edit была перенесена непосредственно в модуль.

## Update

- [#3253639](https://www.drupal.org/node/3253639) Добавлен новый класс `UpdateUploaderTestBase` с универсальной реализацией `::setUp()` для уменьшения дублирования кода в тестах модуля.
- [#3238311](https://www.drupal.org/node/3238311) Маршрут `system.batch_page.html` добавлен в список маршрутов, на которых не показываются предупреждения об обновлениях безопасности.
- [#2807949](https://www.drupal.org/node/2807949) Исправлена неполадка, из-за которой не происходила очистка кеша модуля в момент установки или удаления сторонних модулей, что приводило к ненужным загрузкам переводов и прочим ошибкам. Эта проблема наблюдалась при включении или отключении модуля без участия интерфейса расширений Drupal (например через Drush).

## User

- [#3198010](https://www.drupal.org/node/3198010) Улучшено отображение ошибки о неудачных попытках авторизации, чтобы снизить нагрузку на систему. `UserLoginForm` теперь принимает сервис `bare_html_page_renderer` в качестве аргумента.
- [#1777270](https://www.drupal.org/node/1777270) Добавлены тесты для блока авторизации, которые покрывают ситуации, что можно авторизоваться с паролем длинной до 128 символов.
- [#3247694](https://www.drupal.org/node/3247694) Тесты модуля теперь используют тему оформления Stark вместо Classy.
- [#3258321](https://www.drupal.org/node/3258321) Кнопка удаления аккаунта в форме редактирования пользователя заменена на ссылку.
- [#3268105](https://www.drupal.org/node/3268105) Добавлен новый тест `UserRegistrationRestTest`.

## Views

- [#2569381](https://www.drupal.org/node/2569381) Из `Drupal\views\Plugin\views\area\Result` удалён лишний вызов `XSS::adminFilter()`.
- [#1810148](https://www.drupal.org/node/1810148) Исправлена неполадка, из-за которой сгруппированные раскрытые фильтры по терминам таксономии могли не работать.
- [#2845571](https://www.drupal.org/node/2845571) Исправлена неполадка, из-за которой плагин `ViewsJoin` не учитывал выбранный оператор.

## Views UI

- [#2807241](https://www.drupal.org/node/2807241) Исправлена неполадка, из-за которой список с добавлением новых страниц представлений не работал на отличных от английского языках.

## Symfony 6

- [#3232074](https://www.drupal.org/node/3232074) Для классов расширяющих `Normalizer` добавлен тайпхинт `array|string|int|float|bool|\ArrayObject|null` методу `::normalize()`.
- [#3232095](https://www.drupal.org/node/3232095) Произведён рефакторинг сервиса `update.root` для того чтобы он возвращал объект вместо строки.
- [#3232131](https://www.drupal.org/node/3232131) В `DebugClassLoader` добавлены тайпхинты.
- [#3250299](https://www.drupal.org/node/3250299) Внесены улучшения в констрейнты для совместимости с Symfony 6.
- [#3250442](https://www.drupal.org/node/3250442) Код с использованием Prophecy на методы с тайпхинтом на возвращаемое значение `static` заменены на моки для исправления проблем, пока поддержка не появится в Prophecy.

## Тестирование

- [#3258995](https://www.drupal.org/node/3258995) `run-tests.sh` теперь использует `\Drupal::service('app.root')` вместо `\Drupal::root()`.
- [#2867871](https://www.drupal.org/node/2867871) В `OptimizedPhpArrayDumperTest` улучшено использование Symfony Expression.
- [#3212346](https://www.drupal.org/node/3212346) Добавлен абстрактный тест `ConfigEntityResourceTestBase`. Тесты для конфигурационных сущностей и их ресурсов теперь расширяют данный тест. Это уменьшает количество установок Drupal в процессе тестирования.
- [#3257407](https://www.drupal.org/node/3257407) `BlockCreationTrait::placeBlock()` теперь помещает блоки в `content` регион вместо `sidebar_first`.
- [#3253715](https://www.drupal.org/node/3253715) Обновлены тесты, в которых использовался `isset()` с результатами `xPath`.
- [#3265546](https://www.drupal.org/node/3265546) Модули и темы оформления со свойством `lifecycle: deprecated` теперь исключаются из установки в `DefaultConfigTest`.
- [#3260710](https://www.drupal.org/node/3260710) `UpdateUploaderTestBase` теперь является абстрактным классом.
- [#3268932](https://www.drupal.org/node/3268932) В `WebAssert` добавлены методы для сравнения статусных сообщений.
- [#3272727](https://www.drupal.org/node/3272727) Внесены улучшения в команду `drupalModuleInstall()` для Nightwatch.
- [#3275093](https://www.drupal.org/node/3275093) Обновлены дампы БД для Drupal 9.3.0 тестов.

## Прочие изменения

- [#3038596](https://www.drupal.org/node/3038596) В `drupalci.yml` добавлено напоминание о том, что данный файл требуется в ручном изменении при создании ново ветки ядра.
- [#3245820](https://www.drupal.org/node/3245820) Из кода удалены упоминания удалённого `\Drupal\Core\Action\Plugin\Action\PublishAction`.
- [#3250263](https://www.drupal.org/node/3250263) Удалён неиспользуемый файл `core/scripts/test/test.script`.
- [#3251891](https://www.drupal.org/node/3251891) Исправлены неполадки, приводящие к проблемам на ветке Drupal 10.
- [#3251988](https://www.drupal.org/node/3251988) Обновлена документация о возвращаемых типах данных для `JSWebAssert::waitForText()`.
- [#3256451](https://www.drupal.org/node/3256451) Исправлен некорректный хэш в `composer.lock`.
- [#3210129](https://www.drupal.org/node/3210129) Внесены исправления опечаток для слов, начинающихся с «a» до «d» включительно.
- [#3175428](https://www.drupal.org/node/3175428) В документацию [настройки](../../../../9/settings-php/index.md) `trusted_host_patterns` добавлена ссылка на более детальную документацию.
- [#3031130](https://www.drupal.org/node/3031130) Улучшено поведение для `\Drupal\Core\KeyValueStore\MemoryStorage::rename()`. Если старое название и новое идентичны, данные не будут удалены, как это было ранее.
- [#3258014](https://www.drupal.org/node/3258014) Оформление информации об устаревшем коде улучшено для `Drupal\migrate_drupal\Plugin\migrate\field\NodeReference` и `
  Drupal\taxonomy\Plugin\views\argument_validator\Term`.
- [#3256539](https://www.drupal.org/node/3256539) `ContentEntityDeleteForm` больше не помечен как внутренний класс (`@internal`), это означает, что вы теперь можете использовать и наследоваться от этой формы.
- [#3259996](https://www.drupal.org/node/3259996) Использование `t()` для списков с ссылками в `system_requirements()` заменено на `Markup`.
- [#3181275](https://www.drupal.org/node/3181275) При использовании PHP 8+, Drupal больше не будет выдавать ошибку при использовании `phar://` URI. Так как на PHP 7 он имеет уязвимости, его использование будет по-прежнему запрещено. 
- [#3252406](https://www.drupal.org/node/3252406) Класс `PharExtensionInterceptor` помечен для внутреннего пользования `@internal`.
- [#3229714](https://www.drupal.org/node/3229714) Исправлена область видимости для метода `ContextAwarePluginTrait::getPluginDefinition()` с `protected` на `public`.
- [#3164210](https://www.drupal.org/node/3164210) Везде где `array_merge()` используется внутри цикла, внесены улучшения в код для увеличения производительности.
- [#3248879](https://www.drupal.org/node/3248879) Внесены улучшения в тест `UpdatePathTestTrait`.
- [#3259953](https://www.drupal.org/node/3259953) Ссылка на отчёт о состоянии системы теперь располагается вверху.
- [#3224178](https://www.drupal.org/node/3224178) Из `theme.api.php` удалены упоминания `theme()` функции.
- [#3264435](https://www.drupal.org/node/3264435) Внесены улучшения в тесты модулей `help_topics` и `rest`, в которых не фильтровались устаревшие модули.
- [#3260568](https://www.drupal.org/node/3260568) В процессах установки профилей [Demo Umami](../../../../9/distributions/demo-umami/index.md) и [Стандартный](../../../../9/distributions/standard/index.md) исправлено то, как добавляются роли для административного пользователя.
- [#3259928](https://www.drupal.org/node/3259928) Тесты, проверяющие функционал на «всех темах» теперь также проверяют [Olivero](../../../../olivero/index.md).
- [#3262320](https://www.drupal.org/node/3262320) Удалён устаревшее исправление в `ContextualLinksTest`.
- [#3261517](https://www.drupal.org/node/3261517) Из `InstallTest` удалено упоминание ныне несуществующей функции `drupal_get_schema()`.
- [#3267721](https://www.drupal.org/node/3267721) В `commit-code-check.sh` добавлена проверка, что CKEditor 5 плагины успешно собраны.
- [#3270323](https://www.drupal.org/node/3270323) Исправлена неполадка в тесте `ModuleConfigureRouteTest::testModuleConfigureRoutes()`.
- [#3264911](https://www.drupal.org/node/3264911) Из стилей тем оформления удалено упоминание подозрительного сайта.
- [#3261611](https://www.drupal.org/node/3261611) Ссылки на системные требования обновлены на универсальные.
- [#3272035](https://www.drupal.org/node/3272035) В CSPell словарь добавлены слова «linktext» и «canvastext».
- [#2917655](https://www.drupal.org/node/2917655) Прекращена поддержка PHP 7.3.
- [#3270886](https://www.drupal.org/node/3270886) Удалена устаревшая заметка в `drupalci.yml`.
- [#3013802](https://www.drupal.org/node/3013802) Улучшено сообщение об ошибке для `Url` объектов с несуществующим маршрутом.
- [#3270395](https://www.drupal.org/node/3270395) Библиотеки `editor/drupal.editor.admin` и `filter/drupal.filter.filter_html.admin` больше не зависят от UnderscoreJS.
- [#3270941](https://www.drupal.org/node/3270941) Модуль Color удалён из [стандартного установочного профиля](../../../../10/distributions/standard/index.md)