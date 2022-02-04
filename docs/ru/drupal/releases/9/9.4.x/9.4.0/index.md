---
title: 'Drupal 9.4.0'
slug: wiki/drupal/releases/9.4.0
core: 9
metatags:
  title: 'Drupal 9.4.0: Список изменений'
  description: 'Список изменений Drupal 9.4.0.'
authors:
  - Niklan
---

> [!WARNING]
> Данная версия находится в разработке. Список изменений может меняться и дополняться, даты могут смещаться.

**Дата релиза**: июнь 2022\
**Активная поддержка до**: декабрь 2022\
**Поддержка безопасности до**: июнь 2023

## Хук `THEME_ENGINE_init()` объявлен устаревшим

* [#3158289](https://www.drupal.org/node/3158289) 

Хук `THEME_ENGINE_init()` объявлен устаревшим и будет удалён в [Drupal 10](../../../../10/index.md). Замена данному хуку не предоставляется.

## Для сущностей с поддержкой ревизий, модуль Views теперь предоставляет связи между основной и ревизионной таблицами

* [#2652652](https://www.drupal.org/node/2652652)

Ранее, модуль Views не предоставлял для ревизионных сущностей связи между основной таблицой и таблицой ревизий. Для модулей `block_content` и `node` были «костыли» для решения данной проблемы, но остальным сущностям приходилось решать это вопрос самостоятельно.

Начиная с текущего релиза, Views предоставляет соответствующие связи между основной и ревизионной таблицами для всех типов сущностей что поддерживают ревизии.

## В .htaccess файлы добавлена поддержка настроек PHP 8

* [#3186524](https://www.drupal.org/node/3186524)

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

* [#3252872](https://www.drupal.org/node/3252872)

`Drupal\serialization\Normalizer\NormalizerBase` теперь реализует `CacheableSupportsMethodInterface` и возвращает `FALSE` по умолчанию. Это означает, что классы, расширяющие абстрактные классы из ядра продолжат работать как и раньше. Все конкретные реализации теперь имеют метод `::hasCacheableSupportsMethod()`, который возвращает `TRUE`.

Данный интерфейс был представлен в [Symfony 4.1](https://symfony.com/blog/new-in-symfony-4-1-faster-serializer):

> Приложения могут определять множество нормализаторов и денормализаторов, Symfony вызывает `::supportsNormalization()` для каждого из них, каждый раз производя нормализацию или денормализацию объекта. В теории, результат вызова `::supportsNormalization()` зависит от моженства факторов. На практике большинство из них зависит лишь от типа и формата, а данная информация легко кешируется.

Утверждение выше также подходит и для Drupal. Мы можем кешировать результат основываясь на переданном типе и формате, что позволяет сократить количество вызовов. Подобное изменение позволяет добиться увеличения производительности от 5 до 60 процентов!

## Драйвера баз данных, предоставляемые ядром Drupal, перенесены в соответствующие одноимённые модули

* [#3129043](https://www.drupal.org/node/3129043) 

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

* [#3049048](https://www.drupal.org/node/3049048)

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

* [#3197553](https://www.drupal.org/node/3197553), [#3256518](https://www.drupal.org/node/3256518) 

В Drupal 7 был добавлен [хук](../../../../9/hooks/index.md) `hook_js_alter()`, для того чтобы для него всегда были данные, была введена функция `drupal_js_defaults()`. В Drupal 8 произошёл переход на [систему библиотек](../../../../9/libraries/index.md), что сделало `hook_js_alter()` очень специфичным хуком для исключительных ситуаций. В связи с этим, функция `drupal_js_defaults()` стала бесполезной и объявлена устаревшей, будет удалена в Drupal 10 без предоставления замены.

## Метод `LanguageManagerInterface::getLanguageSwitchLinks()` теперь возвращает объект или `NULL`

* [#2940121](https://www.drupal.org/node/2940121)

В предыдущих версиях Drupal, `Drupal\Core\Language\LanguageManagerInterface::getLanguageSwitchLinks()` имел документацию, что он возвращает массив ссылок, но `
Drupal\language\ConfigurableLanguageManager::getLanguageSwitchLinks()` возвращал объект, содержащий массив ссылок, или же `FALSE`, если ссылок нет. 

Документация для `Drupal\Core\Language\LanguageManagerInterface::getLanguageSwitchLinks()` была изменена на то, что метод теперь возвращает либо объект с массивом ссылок, либо `NULL`, если ссылок нет.

Если вы используете или реализуете данный метод, вам необходимо обновить свой код.

## На замену опции запроса `return` добавлен новый метод `Connection::lastInsertId()`

* [#3185269](https://www.drupal.org/node/3185269) 

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

* [#3255250](https://www.drupal.org/node/3255250)

Symfony 4.2 объявил `\Symfony\Component\Translation\TranslatorInterface::transChoice()` устаревшим методом. Drupal реализует данный интерфейс в `\Drupal\Core\Validation\DrupalTranslator:transChoice()`. Таким образом `\Drupal\Core\Validation\DrupalTranslator:transChoice()` также объявлен устаревшим.

## Функция `module_load_include()` объявлена устаревшей

* [#697946](https://www.drupal.org/node/697946) 

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

* [#2010380](https://www.drupal.org/node/2010380) 

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

* [#2979588](https://www.drupal.org/node/2979588), [#2919215](https://www.drupal.org/node/2919215), [#3258654](https://www.drupal.org/node/3258654) 

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

* [#3215043](https://www.drupal.org/node/3215043) 

Теперь, на странице со списком модулей, будет визуальная индикация для устаревших модулей. Это позволит предупреждать пользователей заранее, что включение модуля может иметь последствия.

Также, при попытке включения таких модулей и тем, будет показываться системное сообщение, предупреждающее о том, что модули или тема объявлены устаревшими и их использование на свой страх и риск.

## Темы оформления теперь могут предоставлять `starterkit` варианты

* [#3206219](https://www.drupal.org/node/3206219) 

Темы оформления теперь в `*.info.yml` файле могут добавить новую опцию `starterkit`. Если данная опция равна `true`, то данная тема может быть использована как стартовая.

Темы оформления, для которых задана опция `starterkit: true` могут быть взяты за основу при генерации новой темы.

Пример использования:

```shell
$ php core/scripts/drupal generate-theme --starterkit [starterkit_themename] [new_themename]
```

В результате выполнения, содержимое темы оформления `[starterkit_themename]` будет скопировано в `themes/[new_themename]`. Название файлов стартовой темы останутся неизменными.

## Drupal теперь предупреждает об отсутствии поддержки JSON текущей базой данных

* [#3192487](https://www.drupal.org/node/3192487)

Drupal теперь выводит предупреждение, если база данных не имеет поддержки JSON.

> [!IMPORTANT]
> [Drupal 10](../../../../10/index.md) будет требовать поддержку JSON расширения для корректной работы.

Если у вас имеются свои драйвера баз данных, убедитесь что `::hasJson()` работает корректно с вашим типом БД, в случае необходимости, переопределите его.

## Расширения для построения SELECT запросов теперь управляются при помощи переопределяемых сервисов 

* [#3217699](https://www.drupal.org/node/3217699)

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

* [#3048464](https://www.drupal.org/node/3048464)

Плагин `SubProcess` требует чтобы входные данные были многоуровневым массивом (массив из массивов), где каждый вложенный массив — значение источника на обработку.

Если значение на входе не будет являться массивом, теперь выбрасывается исключение `MigrateException`.

## Рекомендуемая версия PHP увеличена до 8.1

* [#3261357](https://www.drupal.org/node/3261357) 

Рекомендуемая версия PHP увеличена до 8.0. Минимальная версия по-прежнему остаётся PHP 7.3.0.

> [!WARNING]
> В [Drupal 10.0.0](../../../10/10.0.x/10.0.0/index.md) минимальная версия будет увеличена.

## Aggregator

* [#2610520](https://www.drupal.org/node/2610520) Улучшена справка о блоке предоставляемом модулем.

## Cache System

* [#2873732](https://www.drupal.org/node/2873732) Внесены улучшения в `CookiesCacheContext`, который мог приводить к ошибке «Array
  to string conversion in CacheContextsManager::convertTokensToKeys()».

## CKEditor 5

* [#3258250](https://www.drupal.org/node/3258250) CKEditor обновлён до версии 31.1.0.
* [#3248448](https://www.drupal.org/node/3248448) Улучшено оформление индикатора загрузки диалога.
* [#3248423](https://www.drupal.org/node/3248423) Добавлен `ckeditor5.types.jsdoc` и скрипт, генерирующий актуальное содержание для него. Данный файл может быть использован в IDE для получения корректного автодополнения при написании плагинов для CKEditor.
* [#3258030](https://www.drupal.org/node/3258030) Поля с CKEditor 5 редактором теперь имеет красную рамку если в поле ошибка.

## Claro

* [#3214170](https://www.drupal.org/node/3214170) Кнопка «Отмена» теперь отцентрована в off-canvas модальном окне.
* [#3247994](https://www.drupal.org/node/3247994) Исправлена неполадка, из-за которой мог некорректно работать элемент формы `password`.
* [#3184667](https://www.drupal.org/node/3184667) Улучшено отображение формы материала на широкоформатных экранах.
* [#3168326](https://www.drupal.org/node/3168326) Улучшено отображение dropbutton элемента в таблицах.

## Composer

* [#3246595](https://www.drupal.org/node/3246595) Зависимости ядра обновлены на 01.11.21.
* [#3255623](https://www.drupal.org/node/3255623) Удалены замены для пакетов `paragonie/random_compat` и `symfony/polyfill-php70`.
* [#3258276](https://www.drupal.org/node/3258276) В Composer скрипте `drupal-phpunit-upgrade` убрана опция `--no-suggest` при установке `phpspec/prophecy-phpunit`.

## Configuration System

* [#2343517](https://www.drupal.org/node/2343517) Удалён код и упоминания с `@todo` на задачи, которые решены.

## Database Logging

* [#3246471](https://www.drupal.org/node/3246471) Из класса `DbLogController` удалены свойства, которые объявлены у родительского.

## Database System

* [#3213644](https://www.drupal.org/node/3213644) Внесены улучшения в тест `StatementWrapperLegacyTest::testClientStatementMethod()`.
* [#3259532](https://www.drupal.org/node/3259532) Добавлено тестирование `::hasJson()`.

## Datetime

* [#3251100](https://www.drupal.org/node/3251100) Исправлена неполадка, из-за которой `DateTimeWidgetBase` дважды устанавливал одну и ту же временную зону.

## Entity System

* [#3260520](https://www.drupal.org/node/3260520) `\Drupal\Core\Entity\EntityTypeEvent` и `\Drupal\Core\Field\FieldStorageDefinitionEvent` теперь наследуются от `Event` вместо `GenericEvent`.

## Extension System

* [#3245622](https://www.drupal.org/node/3245622) Ссылкам на справку и права доступа конкретного модуля, в списке модулей, добавлено указание, для какого модуля они а также `title` аттрибуты.
* [#3250585](https://www.drupal.org/node/3250585) Добавлено отображение информации об устаревших модулях и темах на страницу отчётности.
* [#3259850](https://www.drupal.org/node/3259850) Исправлена неполадка, из-за которой мог проваливаться тест `InstallUninstallTest`, если у модуля, объявленного устаревшим, была зависимость на стабильный.
* [#3259888](https://www.drupal.org/node/3259888) Исправлена неполадка, из-за которой мог проваливаться тест `InstallUninstallTest`, если выключать одновременно модули объявленные устаревшими и экспериментальными.

## Field System

* [#3213023](https://www.drupal.org/node/3213023) `FieldConfig` теперь выводит более полезные и понятные сообщения об
  ошибках.

## Filter

* [#3227821](https://www.drupal.org/node/3227821) Исправлена неполадка в фильтре «Заменять переводы строк соответствующими HTML-тегами», которая могла приводить к поломке SVG элементов.

## Image

* [#3223233](https://www.drupal.org/node/3223233) Улучшены заголовки страниц для форм добавления и редактирования [стилей изображений](../../../../9/image/image-styles/index.md).

## JavaScript

* [#3238860](https://www.drupal.org/node/3238860) Использование `jQuery.map()` заменено на нативную `map()` функцию.
* [#3239500](https://www.drupal.org/node/3239500) Добавлен полифил для `Array.includes()`.
* [#3239134](https://www.drupal.org/node/3239134) Использование `jQuery.val()` заменено на нативные `.value`.
* [#3239123](https://www.drupal.org/node/3239123) Использование `jQuery.text()` заменено на нативный `.textContent`.
* [#3246211](https://www.drupal.org/node/3246211) Stylelint обновлён до 14 версии.

## JSON:API

* [#3199696](https://www.drupal.org/node/3199696) `ResourceObject` теперь учитывает текущий язык и корректно кеширует результаты для мультиязычных ресурсов.

## Layout Builder

* [#3253666](https://www.drupal.org/node/3253666) `LayoutTempstoreRouteEnhancer` теперь использует `
  Drupal\Core\Routing\RouteObjectInterface` вместо `Symfony\Cmf\Component\Routing\RouteObjectInterface`.
* [#3190541](https://www.drupal.org/node/3190541) Исправлена неполадка, из-за которой кеш-контекст `layout_builder_is_active` мог предоставлять некорректные значения.

## Media Library

* [#3173770](https://www.drupal.org/node/3173770) `MediaLibraryFieldWidgetOpener` теперь позволяет использовать другие референс поля, расширяющее поле из ядра и использующие `EntityReferenceFieldItemList`.
* [#3248454](https://www.drupal.org/node/3248454) Внесены улучшения в `MediaLibraryStateTest` для совместимости с Symfony 5.4.
* [#3115054](https://www.drupal.org/node/3115054) Исправлена неполадка, из-за которой добавление или удаление элементов сбрасывало сортировку в виджете.

## Migration System

* [#3246053](https://www.drupal.org/node/3246053) Обновлено значение `filesize` файла `ds9.txt` в `file_managed`.
* [#3163663](https://www.drupal.org/node/3163663) Исправлена неполадка из-за которой плагин-обработчик `download` мог приводить к предупреждению «failed to open stream: Too many open file».
* [#3087332](https://www.drupal.org/node/3087332) Плагин-обработчик `d6_url_alias_language` объявлен устаревшим.
* [#3042533](https://www.drupal.org/node/3042533) Внесены улучшения в миграцию словарей таксономии из Drupal 6.
* [#3240109](https://www.drupal.org/node/3240109) Возвращаемый тип данных для `MigrateProcessInterface::transform()` изменён с `string|array` на `mixed`.
* [#3258009](https://www.drupal.org/node/3258009) Удалены два неиспользуемых и сломанных плагина `fieldleft` и `fieldright`.
* [#3240873](https://www.drupal.org/node/3240873) Добавлено тестирование для `hash` свойства.
* [#3226401](https://www.drupal.org/node/3226401) В `Migration` добавлена информация, что миграции можно создавать в `MODULENAME/migrations`.
* [#3254347](https://www.drupal.org/node/3254347) Исключения для плагинов-обработчиков теперь содержат название плагина, в котором выброшено исключение.

## Olivero

* [#3186992](https://www.drupal.org/node/3186992) Исправлена неполадка, из-за которой навигационные пункты меню могли выходить за рамки контейнера.
* [#3256433](https://www.drupal.org/node/3256433) Класс `wide-image` больше не добавляется на картинку профиля пользователя.

## Render System

* [#3172166](https://www.drupal.org/node/3172166) (отменено) Улучшена проверка в `Element::properties()`, которая приводила к «Notice: Trying to access array offset on value of type int» при попытке вызова метода с массивом в качестве аргумента, где
  ключи являлись числами.

## Standard Profile

* [#3171149](https://www.drupal.org/node/3171149) Стандартный профиль теперь использует стиль изображения `wide` для
  публикаций вместо `large`.

## Toolbar

* [#3257504](https://www.drupal.org/node/3257504) Тулбар больше не показывает кнопку переключение между горизонтальной и вертикальной ориентацией если в тулбаре нет никаких пунктов.

## Update

* [#3253639](https://www.drupal.org/node/3253639) Добавлен новый класс `UpdateUploaderTestBase` с универсальной реализацией `::setUp()` для уменьшения дублирования кода в тестах модуля.
* [#3238311](https://www.drupal.org/node/3238311) Маршрут `system.batch_page.html` добавлен в список маршрутов, на которых не показываются предупреждения об обновлениях безопасности.

## User

* [#3198010](https://www.drupal.org/node/3198010) Улучшено отображение ошибки о неудачных попытках авторизации, чтобы снизить нагрузку на систему. `UserLoginForm` теперь принимает сервис `bare_html_page_renderer` в качестве аргумента.

## Views

* [#2569381](https://www.drupal.org/node/2569381) Из `Drupal\views\Plugin\views\area\Result` удалён лишний вызов `XSS::adminFilter()`.

## Symfony 6

* [#3232074](https://www.drupal.org/node/3232074) Для классов расширяющих `Normalizer` добавлен тайпхинт `array|string|int|float|bool|\ArrayObject|null` методу `::normalize()`.
* [#3232095](https://www.drupal.org/node/3232095) Произведён рефакторинг сервиса `update.root` для того чтобы он возвращал объект вместо строки.
* [#3232131](https://www.drupal.org/node/3232131) В `DebugClassLoader` добавлены тайпхинты.
* [#3250299](https://www.drupal.org/node/3250299) Внесены улучшения в констрейнты для совместимости с Symfony 6.
* [#3250442](https://www.drupal.org/node/3250442) Код с использованием Prophecy на методы с тайпхинтом на возвращаемое значение `static` заменены на моки для исправления проблем, пока поддержка не появится в Prophecy.

## Тестирование

* [#3258995](https://www.drupal.org/node/3258995) `run-tests.sh` теперь использует `\Drupal::service('app.root')` вместо `\Drupal::root()`.
* [#2867871](https://www.drupal.org/node/2867871) В `OptimizedPhpArrayDumperTest` улучшено использование Symfony Expression.
* [#3212346](https://www.drupal.org/node/3212346) Добавлен абстрактный тест `ConfigEntityResourceTestBase`. Тесты для конфигурационных сущностей и их ресурсов теперь расширяют данный тест. Это уменьшает количество установок Drupal в процессе тестирования.
* [#3257407](https://www.drupal.org/node/3257407) `BlockCreationTrait::placeBlock()` теперь помещает блоки в `content` регион вместо `sidebar_first`.

## Прочие изменения

* [#3038596](https://www.drupal.org/node/3038596) В `drupalci.yml` добавлено напоминание о том, что данный файл требуется в ручном изменении при создании ново ветки ядра.
* [#3245820](https://www.drupal.org/node/3245820) Из кода удалены упоминания удалённого `\Drupal\Core\Action\Plugin\Action\PublishAction`.
* [#3250263](https://www.drupal.org/node/3250263) Удалён неиспользуемый файл `core/scripts/test/test.script`.
* [#3251891](https://www.drupal.org/node/3251891) Исправлены неполадки, приводящие к проблемам на ветке Drupal 10.
* [#3251988](https://www.drupal.org/node/3251988) Обновлена документация о возвращаемых типах данных для `JSWebAssert::waitForText()`.
* [#3256451](https://www.drupal.org/node/3256451) Исправлен некорректный хэш в `composer.lock`.
* [#3210129](https://www.drupal.org/node/3210129) Внесены исправления опечаток для слов, начинающихся с «a» до «d» включительно.
* [#3175428](https://www.drupal.org/node/3175428) В документацию [настройки](../../../../9/settings-php/index.md) `trusted_host_patterns` добавлена ссылка на более детальную документацию.
* [#3031130](https://www.drupal.org/node/3031130) Улучшено поведение для `\Drupal\Core\KeyValueStore\MemoryStorage::rename()`. Если старое название и новое идентичны, данные не будут удалены, как это было ранее.
* [#3258014](https://www.drupal.org/node/3258014) Оформление информации об устаревшем коде улучшено для `Drupal\migrate_drupal\Plugin\migrate\field\NodeReference` и `
  Drupal\taxonomy\Plugin\views\argument_validator\Term`.
* [#3256539](https://www.drupal.org/node/3256539) `ContentEntityDeleteForm` больше не помечен как внутренний класс (`@internal`), это означает, что вы теперь можете использовать и наследоваться от этой формы.
* [#3259996](https://www.drupal.org/node/3259996) Использование `t()` для списков с ссылками в `system_requirements()` заменено на `Markup`.
* [#3181275](https://www.drupal.org/node/3181275) При использовании PHP 8+, Drupal больше не будет выдавать ошибку при использовании `phar://` URI. Так как на PHP 7 он имеет уязвимости, его использование будет по-прежнему запрещено. 
* [#3252406](https://www.drupal.org/node/3252406) Класс `PharExtensionInterceptor` помечен для внутреннего пользования `@internal`.
* [#3229714](https://www.drupal.org/node/3229714) Исправлена область видимости для метода `ContextAwarePluginTrait::getPluginDefinition()` с `protected` на `public`.