---
title: 'Drupal 9.4.0'
slug: wiki/drupal/releases/9.4.0
core: 9
metatags:
  title: 'Drupal 9.4.0: Список изменений'
  description: 'Список изменений Drupal 9.4.0.'
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

## Обновлены старые схемы для `uid` поля сущностей, где может использоваться устаревший метод получения ID пользователя

* [#3153455](https://www.drupal.org/node/3153455) Добавлено обновление, которое исправляет

Сайты, установленные на версии ранее чем Drupal 8.6.0, а теперь использующие Drupal 9, могут столкнуться с проблемой, что у некоторых сущностей из ядра поле `uid` использует устаревшее значение для `default_value_callback`. Ранее, использовался метод `{EntityTypeClass}::getCurrentUserId()`, который был объявлен устаревшим в Drupal 8.6.0 и удалён в Drupal 9.

На таких проектах, подобная ситуация может приводить, как к ошибке: «The website encountered an unexpected error. Please try again later. Error: Call to a member function getAccountName() on null», так и предупреждению «call_user_func() expects parameter 1 to be a valid callback, class 'Drupal\node\Entity\Node' does not have a method 'getCurrentUserId' in Drupal\Core\Field\FieldConfigBase->getDefaultValue()».

В данном релизе добавлены обновления для всех сущностей ядра, которые проверяют, что используется современный метод, и если обнаруживается старый, он обновляется на новый.

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

> [!NOTE]
> До данного изменения, при попытке обратиться к ресурсам JSON:API при включенном режиме обслуживания, в качестве ответа отдавалась HTML страница обслуживания.

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

## Cache System

* [#2873732](https://www.drupal.org/node/2873732) Внесены улучшения в `CookiesCacheContext`, который мог приводить к ошибке «Array
  to string conversion in CacheContextsManager::convertTokensToKeys()».

## Claro

* [#3214170](https://www.drupal.org/node/3214170) Кнопка «Отмена» теперь отцентрована в off-canvas модальном окне.
* [#3247994](https://www.drupal.org/node/3247994) Исправлена неполадка, из-за которой мог некорректно работать элемент формы `password`.

## Composer

* [#3246595](https://www.drupal.org/node/3246595) Зависимости ядра обновлены на 01.11.21.
* [#3255623](https://www.drupal.org/node/3255623) Удалены замены для пакетов `paragonie/random_compat` и `symfony/polyfill-php70`.

## Database System

* [#3213644](https://www.drupal.org/node/3213644) Внесены улучшения в тест `StatementWrapperLegacyTest::testClientStatementMethod()`.

## Datetime

* [#3251100](https://www.drupal.org/node/3251100) Исправлена неполадка, из-за которой `DateTimeWidgetBase` дважды устанавливал одну и ту же временную зону.

## Extension System

* [#3245622](https://www.drupal.org/node/3245622) Ссылкам на справку и права доступа конкретного модуля, в списке модулей, добавлено указание, для какого модуля они а также `title` аттрибуты.

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

## JSON:API

* [#3199696](https://www.drupal.org/node/3199696) `ResourceObject` теперь учитывает текущий язык и корректно кеширует результаты для мультиязычных ресурсов.

## Layout Builder

* [#3253666](https://www.drupal.org/node/3253666) `LayoutTempstoreRouteEnhancer` теперь использует `
  Drupal\Core\Routing\RouteObjectInterface` вместо `Symfony\Cmf\Component\Routing\RouteObjectInterface`.

## Media Library

* [#3173770](https://www.drupal.org/node/3173770) `MediaLibraryFieldWidgetOpener` теперь позволяет использовать другие референс поля, расширяющее поле из ядра и использующие `EntityReferenceFieldItemList`.
* [#3248454](https://www.drupal.org/node/3248454) Внесены улучшения в `MediaLibraryStateTest` для совместимости с Symfony 5.4.

## Migration System

* [#3246053](https://www.drupal.org/node/3246053) Обновлено значение `filesize` файла `ds9.txt` в `file_managed`.
* [#3163663](https://www.drupal.org/node/3163663) Исправлена неполадка из-за которой плагин-обработчик `download` мог приводить к предупреждению «failed to open stream: Too many open file».
* [#3087332](https://www.drupal.org/node/3087332) Плагин-обработчик `d6_url_alias_language` объявлен устаревшим.

## Olivero

* [#3186992](https://www.drupal.org/node/3186992) Исправлена неполадка, из-за которой навигационные пункты меню могли выходить за рамки контейнера.

## Standard Profile

* [#3171149](https://www.drupal.org/node/3171149) Стандартный профиль теперь использует стиль изображения `wide` для
  публикаций вместо `large`.

## Update

* [#3253639](https://www.drupal.org/node/3253639) Добавлен новый класс `UpdateUploaderTestBase` с универсальной реализацией `::setUp()` для уменьшения дублирования кода в тестах модуля.

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

## Прочие изменения

* [#3038596](https://www.drupal.org/node/3038596) В `drupalci.yml` добавлено напоминание о том, что данный файл требуется в ручном изменении при создании ново ветки ядра.
* [#3245820](https://www.drupal.org/node/3245820) Из кода удалены упоминания удалённого `\Drupal\Core\Action\Plugin\Action\PublishAction`.
* [#3250263](https://www.drupal.org/node/3250263) Удалён неиспользуемый файл `core/scripts/test/test.script`.
* [#3251891](https://www.drupal.org/node/3251891) Исправлены неполадки, приводящие к проблемам на ветке Drupal 10.
* [#3251988](https://www.drupal.org/node/3251988) Обновлена документация о возвращаемых типах данных для `JSWebAssert::waitForText()`.