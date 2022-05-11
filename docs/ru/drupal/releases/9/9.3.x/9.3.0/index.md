---
title: 'Drupal 9.3.0'
slug: wiki/drupal/releases/9.3.0
core: 9
metatags:
  title: 'Drupal 9.3.0: Список изменений' 
  description: 'Список изменений Drupal 9.3.0.'
authors:
  - Niklan
  - chesn0k
category:
  area: 'Drupal 9.3.x'
  title: Drupal 9.3.0
  order: 0
---

**Дата релиза**: 8 декабря 2021

## Параметры `{node}` и `{node_revision}` для маршрутов ревизий теперь конвертируются в сущности

- [#2730631](https://www.drupal.org/node/2730631)

Маршруты ревизий нод, такие, как `/node/123/revisions/456/view`, теперь возвращают объекты сущности Node при обращении к
параметрам `\Drupal::routeMatch()->getParameter('node')` и `\Drupal::routeMatch()->getParameter('node_revision')`.

Данное изменение касается следующих маршрутов:

* `entity.node.revision`: `/node/{node}/revisions/{node_revision}/view`
* `node.revision_revert_confirm`: `/node/{node}/revisions/{node_revision}/revert`
* `node.revision_revert_translation_confirm`: `/node/{node}/revisions/{node_revision}/revert/{langcode}`
* `node.revision_delete_confirm`: `/node/{node}/revisions/{node_revision}/delete`

**Ранее:**

```php
$nid = \Drupal::routeMatch()->getParameter('node');
$node = \Drupal\node\Entity\Node::load($nid);
$vid = \Drupal::routeMatch()->getParameter('node_revision');
$node_revision = node_revision_load($vid);
```

До этого изменения, `$nid` и `$vid` возвращали целые числа, поэтому требовалось загружать сущность самостоятельно.

**Теперь:**

```php
$node = \Drupal::routeMatch()->getParameter('node');
$nid = $node->id();
$node_revision = \Drupal::routeMatch()->getParameter('node_revision');
$vid = $node_revision->getRevisionId();
```

Параметры возвращают объекты сущности.

## ThemeManagerInterface::getActiveTheme() теперь принимает параметр $route_match

- [#3160307](https://www.drupal.org/node/3160307)

Сигнатура метода `ThemeManagerInterface::getActiveTheme()` была обновлена следующим образом:

```php
public function getActiveTheme(RouteMatchInterface $route_match = NULL) {
```

Отсутствие данного параметра было недосмотром при введении данного интерфейса в ядро. Все реализации данного интерфейса
ядром ожидают данный аргумент.

## Добавлен новый метод Connection::lastInsertId(), опция запроса 'return' и константы `Database::RETURN_*` были объявлены устаревшими

- [#3177660](https://www.drupal.org/node/3177660)

Опция запроса `return` и константы `Database::RETURN_*` объявлены устаревшими. В [Drupal 10](../../../../10/index.md) они
будут удалены, а `Connection::query()` всегда будет возвращать объект `StatementInterface`.

В целом, `Connection::query()` не должен использоваться для операций манипулирования данных (`INSERT`, `DELETE`
, `UPSERT`, `MERGE`, `TRUNCATE`). Для данных целей Drupal предоставляет API для построения динамических запросов,
который позволяет абстрагироваться от базы данных.

Для крайних случаев, когда `INSERT` требуется использовать при помощи `Connection:query()`, добавлен новый
метод `Connection::lastInsertId()` который возвращает ID последнего значения.

Для операций `UPDATE` и других, начиная с текущего изменения не следует использовать `Database::RETURN_AFFECTED`. Вместо
этого используйте подсчёт строк, передав соответствующий аргумент в конструктор.

Ниже представлен пример использования `Connection::nextId()` для MySQL базы данных.

**Ранее:**

```php
$new_id = $this->query('INSERT INTO {sequences} () VALUES ()', [], ['return' => Database::RETURN_INSERT_ID]);
```

**Сейчас:**

```php
$this->query('INSERT INTO {sequences} () VALUES ()');
$new_id = $this->lastInsertId();
```

## Drupal больше не поддерживает Opera Mini

- [#3202818](https://www.drupal.org/node/3202818)

Начиная с Drupal 9.3.0, Drupal ядро больше не гарантирует поддержку на Opera Mini.

<Aside>

Не перепутайте Opera Mini с Opera Mobile — это два разных браузера. Второй будет поддерживаться корректно, так как он основан на Chromium.

</Aside>

## Cо страниц нод и терминов таксономии были удалены специфичные для Drupal теги

- [#2922570](https://www.drupal.org/node/2922570)

В предыдущих версиях Drupal, на страницах материалов сущности `node` и `taxonomy_term` добавлялись `<link>` теги в шапку
страницы:

```html

<link rel="delete-form" href="https://example.com/node/1/delete"/>
<link rel="delete-multiple-form" href="https://example.com/admin/content/node/delete?node=1"/>
<link rel="edit-form" href="https://example.com/node/1/edit"/>
<link rel="version-history" href="https://example.com/node/1/revisions"/>
<link rel="revision" href="https://example.com/node/1"/>
```

Вывод данных ссылок — ресурсоёмкая и затратная операция, которая негативно сказывается на кешировании. Данные ссылки
специфичны для Drupal и не имеют известных применений в реальных проектах, поэтому они были удалены.

`canonical` и `shortlink` по-прежнему будут добавляться без изменений, дополнительно, данные `<link>` теги будут теперь
добавляться на страницы всех типов сущностей, а не только `node` и `taxonomy_term`.

Сайты, которым требуется старое поведение, могут вернуть ссылки добавив следующий код в `hook_entity_view()`
или `hook_page_attachments()`:

```php
foreach ($node->uriRelationships() as $rel) {
  $url = $node->toUrl($rel)->setAbsolute(TRUE);
  // Add link relationships if the user is authenticated or if the anonymous
  // user has access. Access checking must be done for anonymous users to
  // avoid traffic to inaccessible pages from web crawlers. For
  // authenticated users, showing the links in HTML head does not impact
  // user experience or security, since the routes are access checked when
  // visited and only visible via view source. This prevents doing
  // potentially expensive and hard to cache access checks on every request.
  // This means that the page will vary by user.permissions. We also rely on
  // the access checking fallback to ensure the correct cacheability
  // metadata if we have to check access.
  if ($this->currentUser->isAuthenticated() || $url->access($this->currentUser)) {
    // Set the node path as the canonical URL to prevent duplicate content.
    $build['#attached']['html_head_link'][] = [
      [
        'rel' => $rel,
        'href' => $url->toString(),
      ],
      TRUE,
    ];

  if ($rel == 'canonical') {
    // Set the non-aliased canonical path as a default shortlink.
    $build['#attached']['html_head_link'][] = [
      [
        'rel' => 'shortlink',
        'href' => $url->setOption('alias', TRUE)->toString(),
      ],
      TRUE,
    ];
  }
}

// Since this generates absolute URLs, it can only be cached "per site".
$build['#cache']['contexts'][] = 'url.site';

// Given this varies by $this->currentUser->isAuthenticated(), add a cache
// context based on the anonymous role.
$build['#cache']['contexts'][] = 'user.roles:anonymous';
```

Вы можете посмотреть на удалённые реализации в `taxonomy_page_attachments_alter()`
и ` content_translation_page_attachments()`.

Авторы модулей, которые добавляли для своих сущностей `canonical` и `shortlink` теги, могут удалить их.

## В info файлах модулей и тем можно указывать их стабильность с помощью ключей  'lifecycle' и 'lifecycle_link'.

- [#3124762](https://www.drupal.org/node/3124762), [#3225812](https://www.drupal.org/node/3225812)

Информационные файлы модулей и тем теперь поддерживают два новых параметра `lifecycle` и `lifecycle_link`.

Поддерживаемые значения для `lifecycle`:

* **experimental**: Что-то новое, не завершённое. Будет показано предупреждение при попытке включить подобное
  расширение, но оно будет работать.
* **stable**: (по умолчанию) Стабильное расширение, никаких дополнительных предупреждений показано не будет.
* **deprecated**: Что-то на пути к выводу из применения. Пользователь по-прежнему сможет устанавливать подобное
  расширение, но будут показываться предупреждения.
* **obsolete**: Поддержка прекращена. Пользователь должен удалить данное расширение. Ранее установленные расширения
  продолжат работать, но будет показано предупреждение. Установить данное расширение будет невозможно.

Для значений **deprecated** и **obsolete** состояний требуется указать значение для `lifecycle_link`. Данное значение
должно быть URL на документ, где содержится информация для пользователя, которая может помочь сориентироваться что
делать дальше.

Пример использования:

```yaml
name: Some core module
type: module
description: '...'
package: Core
version: VERSION
core: 9.x.x
hidden: true
lifecycle: [ experimental|stable|deprecated|obsolete ]
lifecycle_link: 'https://www.drupal.org/about/core/policies/core-change-policies/deprecated-and-obsolete-modules-and-themes#s-entity-reference'
```

В связи с изменением, добавлено новое исключение `\Drupal\Core\Extension\Exception\ObsoleteExtensionException`, которое
выбрасывается при попытке установить **obsolete** расширение.

Добавлен класс с константами `\Drupal\Core\Extension\ExtensionStatus` в которых перечислены допустимые статусы.

## Функции `schema.inc` заменены новым классом `UpdateHookRegistry`

- [#2124069](https://www.drupal.org/node/2124069)

Константа `SCHEMA_UNINSTALLED` объявлена устаревшей и была
заменена `Drupal\Core\Update\UpdateHookRegistry::SCHEMA_UNINSTALLED`.

Следующие функции были объявлены устаревшими и заменены соответствующими методами сервиса `update.update_hook_registry`:

* `drupal_get_schema_versions()` заменена `\Drupal::service('update.update_hook_registry')->getAvailableUpdates()`
* `drupal_get_installed_schema_version()`
  заменена `\Drupal::service('update.update_hook_registry')->getInstalledVersion()`
* `drupal_set_installed_schema_version()`
  заменена `\Drupal::service('update.update_hook_registry')->setInstalledVersion()`

## Стандартный профиль теперь предоставляет новую роль «Content Editor»

- [#3059984](https://www.drupal.org/node/3059984)

При установке [стандартного профиля](../../../../9/distributions/standard/index.md) теперь добавляется новая роль —
«Content Editor».

## rel="shortcut icon" теперь rel="icon"

- [#3195222](https://www.drupal.org/node/3195222)

Исторически, иконка сайта (favicon) внедрялась в HTML следующим образом:

```html

<link rel="shortcut icon" href="favicon.ico">
```

Данная разметка больше не соответствует HTML спецификации поэтому начиная с Drupal 9.3.0 разметка будет следующей:

```html

<link rel="icon" href="favicon.ico">
```

## Создание меню для страниц представлений больше не требует включения модуля menu_ui

- [#3021804](https://www.drupal.org/node/3021804)

Следующие плагины представлений больше не требуют активного модуля `menu_ui`:

* `\Drupal\node\Plugin\views\wizard\Node`
* `\Drupal\views\Plugin\views\display\Page`
* `\Drupal\views\Plugin\views\wizard\WizardPluginBase`

Конструктор данных классов теперь требует опциональный аргумент с объектом,
реализующий `\Drupal\Core\Menu\MenuParentFormSelectorInterface` (сервис `menu.parent_form_selector`).

## Добавлена возможность указывать идентификатор для раскрытой сортировки Views

- [#2897638](https://www.drupal.org/node/2897638)

До текущего релиза, только идентификаторы раскрытых фильтров Views могли быть настроены. С текущего релиза также можно
указать идентификаторы для раскрытых сортировок. Это позволяет указать более понятное значение в качестве идентификатора
для `sort_by`.

В связи с данным изменением, метод `\Drupal\views\Plugin\views\display\DisplayPluginBase::isIdentifierUnique()` теперь
требует третий параметр `string $handler_type`.

## `LayoutBuilderContextTrait::getAvailableContexts()` объявлен устаревшим. Добавлен `LayoutBuilderContextTrait::getPopulatedContexts()`

- [#3099968](https://www.drupal.org/node/3099968)

`\Drupal\layout_builder\Context\LayoutBuilderContextTrait::getAvailableContexts()` объявлен устаревшим и будет удалён
в [Drupal 10](../../../../../drupal/10/index.md). Вместо этого метода используйте новый
метод `\Drupal\layout_builder\Context\LayoutBuilderContextTrait::getPopulatedContexts()`.

Это сделано потому что метод `::getAvailableContexts()` был предназначен, для того чтобы предоставить список доступных
контекстов, но не предоставлял соответствующие значения для данных контекстов, так как некоторые значения вычисляются в
рантайме. Это изменение позволяет Layout Builder использовать контексты, которые регистрируются в рантайме.

## Функции обратного вызова указанные в `#date_time_callback` и `#date_date_callbacks` должны реализовывать `TrustedCallbackInterface`

- [#3217966](https://www.drupal.org/node/3217966)

В [Drupal 8.8.0](../../../8/8.8.x/8.8.0/index.md) функции обратного вызова `#access_callback`, `#lazy_builder`
, `#pre_render` и `#post_render` стали требовать реализации `TrustedCallbackInterface`. Начиная с текущей версии, это
требование распространяется на функции обратного вызова для `#date_time_callback` и `#date_date_callbacks`.

## В ядро добавлен `eslint-plugin-jquery`

- [#3191023](https://www.drupal.org/node/3191023)

Для того чтобы ограничить использование jQuery в Drupal-ядре и улучшить совместимость с будущими версия, был добавлен
плагин [eslint-plugin-jquery](https://github.com/dgraham/eslint-plugin-jquery) в конфигурацию `eslint core-jspassing`.

В настоящее время включено только небольшое количество правил в `eslint-plugin-jquery`. В основном это правила
проверяющие на использование возможностей jQuery которые не используются ядром. По мере того как ядро исключает
использование той или иной функции jQuery, в связи с тем что современный ES6 JavaScript имеет аналогичные нативные
возможности, дополнительные правила могут быть включены. Это предотвратит повторное использование этих функций jQuery.

## Сервисам стало доступно автомонтирование

- [#3021803](https://www.drupal.org/node/3021803)

Drupal использует контейнер сервисов Symfony, однако некоторые функции, такие как автомонтирование и автоконфигурация,
не были задействованы в Drupal 8.

Сервисы могут быть автомонтированы, и ядро Drupal теперь включает тесты для этой функциональности. Это означает, что при
определении [сервиса](../../../../9/services/index.md) в `*.services.yml`, во многих случаях аргументы могут быть
автоматически выведены из подсказок типов аргументов конструктора. Чтобы активировать функцию автомонтирования, при
определении сервиса необходимо указать `autowire: true`. (Включение autowiring по умолчанию для всех сервисов в файле
пока не поддерживается).

Сервис будет создан точно так же, как и раньше, но нет необходимости явно указывать, какие сервисы требуются;
интерфейсы, объявленные в конструкторе сервиса, будут использоваться для определения того, какие сервисы должны быть
внедрены.

Хотя конструктор контейнеров/парсер файлов сервисов Yaml в Drupal теперь поддерживает автомонтирование, в ядре этот
паттерн пока не реализован. Поскольку текущее соглашение Drupal об именовании сервисов использует строки, а не имена
классов для идентификаторов сервисов, разработчики сайтов не сразу смогут использовать автомонтирование для сервисов из
ядра. Для добавления уровня обратной совместимости псевдонимов сервисов существующих интерфейсов ядра
существует [последующий issue](https://www.drupal.org/project/drupal/issues/3049525).

Для случаев, когда несколько сервисов реализуют один и тот же интерфейс, например кеш-бэкенды, мы не можем добавить
синоним, так как мы не знаем какой из сервисов необходимо подключать. Для таких случаев добавлена возможность указать
необходимый сервис, например:

```php
token:
  class: Drupal\Core\Utility\Token
  autowire: true
  arguments:
    $cache: '@cache.default'
```

В примере выше, аргумент `$cache` имеет тип `\Drupal\Core\Cache\CacheBackendInterface`, но у нас множество реализаций
данного интерфейса и они имеют разные сервисы. В примере выше мы явно указали что данный параметр ожидает
сервис `@cache.default` в качестве аргумента, а все остальные параметры будут автомонтированы, как и прежде.

Для более детальной информации об автомонтировании
сервисов [изучите документацию Symfony](https://symfony.com/doc/current/service_container/autowiring.html).

## Модуль Update больше не зависит от модуля File

- [#3014051](https://www.drupal.org/node/3014051)

Модуль Update позволяет устанавливать модули и темы при помощи URL-адресов и загрузки файлов. Ранее, модуль File
устанавливался как зависимость, хотя модуль Update может работать без него (загрузка на основе URL-адреса).

Теперь модуль File не является зависимостью для модуля Update, но всё также требуется если вы хотите включить установку
модулей и тем при помощи загрузки архива.

## Функции `menu_list_system_menus()` и `menu_ui_get_menus()` объявлены устаревшими

- [#1882552](https://www.drupal.org/node/1882552)

Функции `menu_list_system_menus()` и `menu_ui_get_menus()` объявлены устаревшими. Вместо них используйте систему
сущностей.

### menu_list_system_menus()

**Ранее:**

```php
$menu_list = menu_list_system_menus();
```

**Сейчас:**

```php
Menu::loadMultiple();
```

### menu_ui_get_menus()

**Ранее:**

```php
$menu_list = menu_ui_get_menus();
```

**Сейчас:**

```php
$menu_list = array_map(static function ($menu) {
  return $menu->label();
}, \Drupal\system\Entity\Menu::loadMultiple());
\asort($menu_list);
```

## Часть функций `taxonomy.module` объявлены устаревшими

- [#3039039](https://www.drupal.org/node/3039039)

Некоторые функции `taxonomy.module` устарели. Также, использование `drupal_static_reset()` c
значением `taxonomy_vocabulary_get_names` в качестве параметра устарело.

Следующие функции устарели:

* `taxonomy_vocabulary_get_names()`
* `taxonomy_term_uri()`
* `taxonomy_term_load_multiple_by_name()`
* `taxonomy_terms_static_reset()`
* `taxonomy_vocabulary_static_reset()`
* `taxonomy_implode_tags()`
* `taxonomy_term_title()`

### Замены

#### taxonomy_vocabulary_get_names()

**Ранее:**

```php
$vids = taxonomy_vocabulary_get_names();
```

**Сейчас:**

```php
$vids = \Drupal::entityQuery('taxonomy_vocabulary')->execute();
```

#### taxonomy_term_load_multiple_by_name()

**Ранее:**

```php
$terms = taxonomy_term_load_multiple_by_name(
  'Foo',
  'topics'
);
```

**Сейчас:**

```php
$storage = \Drupal::entityTypeManager()->getStorage('taxonomy_vocabulary');
$terms = $storage->loadByProperties([ 
  'name' => 'Foo',
  'vid' => 'topics',
]);
```

#### taxonomy_term_uri()

**Ранее:**

```php
$url = taxonomy_term_uri($term);
```

**Сейчас:**

```php
$url = $term->toUrl();
```

#### taxonomy_terms_static_reset()

**Ранее:**

```php
taxonomy_terms_static_reset()
```

**Сейчас:**

```php
$storage = \Drupal::entityTypeManager()->getStorage('taxonomy_term');
$storage->resetCache();
```

#### taxonomy_vocabulary_static_reset()

**Ранее:**

```php
taxonomy_vocabulary_static_reset($vids);
```

**Сейчас:**

```php
$storage = \Drupal::entityTypeManager()->getStorage('taxonomy_vocabulary');
$storage->resetCache($vids);
```

#### taxonomy_implode_tags()

**Ранее:**

```php
taxonomy_implode_tags()
```

**Сейчас:**

```php
\Drupal\Core\Entity\Element\EntityAutocomplete::getEntityLabels();
```

#### taxonomy_term_title()

**Ранее:**

```php
$name = taxonomy_term_title($term);
```

**Сейчас:**

```php
$name = $term->label();
```

#### drupal_static_reset() + taxonomy_vocabulary_get_names

**Ранее:**

```php
drupal_static_reset('taxonomy_vocabulary_get_names');
```

**Сейчас:**

```php
$storage = \Drupal::entityTypeManager()->getStorage('taxonomy_vocabulary');
$storage->resetCache();
```

## Права доступа теперь могут объявлять зависимости

- [#2571235](https://www.drupal.org/node/2571235)

Разрешения могут генерироваться динамически с помощью функций обратного вызова. Например, модуль Node генерирует
разрешения на основе доступных типов содержимого. Если условия, вызывающие генерацию таких разрешений, изменяются, и они
больше не существуют, то разрешение будет удалено из любой роли, которой оно было назначено.

Для поддержки удаления таких разрешений в массив информации о разрешении был добавлен новый ключ `dependencies`. Текущая
структура этого массива такова:

```php
 * # The key is the permission machine name, and is required.
 * administer filters:
 *   # (required) Human readable name of the permission used in the UI.
 *   title: 'Administer text formats and filters'
 *   # (optional) Additional description fo the permission used in the UI.
 *   description: 'Define how text is handled by combining filters into text formats.'
 *   # (optional) Boolean, when set to true a warning about site security will
 *   # be displayed on the Permissions page. Defaults to false.
 *   restrict access: false
 *   # (optional) Dependency array following the same structure as the return
 *   # config entities dependencies.
 *   dependencies:
 *     config:
 *       - node.type.article
```

### Добавление разрешений при помощи `BundlePermissionHandlerTrait`

Для того чтобы помочь вам добавлять подобные разрешения добавлен новый
трейт `Drupal\Core\Entity\BundlePermissionHandlerTrait` с методом `::generatePermissions()`.

Например, `Drupal\node\NodePermissions` уже имеет метод `::buildPermissions()`, который возвращает массив массивов.
Ключами внешнего массива являются машинные имена разрешений, а внутренние массивы имеют ключи `title` (обязательно)
и `description` (необязательно).

**До Drupal 9.3.0:**

```php
  public function nodeTypePermissions() {
    $perms = [];
    // Generate node permissions for all node types.
    foreach (NodeType::loadMultiple() as $type) {
      $perms += $this->buildPermissions($type);
    }

    return $perms;
  }
```

**В Drupal 9.3.0 и далее:**

```php
use Drupal\Core\Entity\BundlePermissionHandlerTrait;

class NodePermissions {
  use BundlePermissionHandlerTrait;
  // ...
  public function nodeTypePermissions() {
    return $this->generatePermissions(NodeType::loadMultiple(), [$this, 'buildPermissions']);
  }
  // ...
}
```

## Функции `file_create_url()` и `f`ile_url_transform_relative()` объявлены устаревшими, добавлен новый сервис `file_url_generator`

- [#2669074](https://www.drupal.org/node/2669074)

Функции `file_crete_url()` и `file_url_transform_relative()` объявлены устаревшими.

В качестве замены добавлен новый сервис `file_url_generator`.

Ниже представлены примеры как старые вызовы заменить сервисом:

* `file_create_url($uri)` → `\Drupal::service('file_url_generator')->generateAbsoluteString($uri)`
* `file_url_transform_relative($file_url)` → `\Drupal::service('file_url_generator')->transformRelative($file_url)`
* `file_url_transform_relative(file_create_url($uri)` → `\Drupal::service('file_url_generator')->generateString($uri)`
* `Drupal\Core\Url::fromUri(file_create_url($uri))` → `\Drupal::service('file_url_generator')->generate($uri)`

Также были представлены следующие новые методы:

* Метод `::generateString()` генерирует URL относительно корня web-приложения (webroot, `index.php`).
* Метод `::generateAbsoluteString()` генерирует абсолютный URL.
* Метод `::generate()` URL-объект с относительным путём.
* Метод `::transformRelative()` который преобразует абсолютный URL локального файла в относительный.

Существующие сервисы `asset.css.optimizer`, `asset.js.collection_renderer` и `asset.css.collection_renderer` теперь
принимают сервис `file_url_generator` в качестве аргумента.

## Библиотека core/jquery.once объявлена устаревшей

- [#3207782](https://www.drupal.org/node/3207782), [#3183149](https://www.drupal.org/node/3183149)

**Изменения API:**

* Добавлена новая библиотека `core/once`
  использующая [Drupal Once JavaScript API](../../../../../javascript/drupal/once/index.md).
* Библиотека `core/jquery.once` объявлена устаревшей.
* Добавлена новая глобальная переменная `once` в конфигурацию eslint.
* Новая once библиотека предоставляет 4 функции:
    * `once` = `$.once`
    * `once.filter()` = `$.findOnce()`
    * `once.remove()` = `$.removeOnce()`
    * `once.find()` новая функция не имеющая аналога в jQuery.

### Обратная совместимость

Если текущий код использует `core/jquery.once` из ядра, добавлен слой обратной совместимости который автоматически при
использовании устаревшей библиотеки. Вызовы `jQuery.once` будут учитывать предыдущие вызовы `once()`.

Для более детальной документации изучите [Drupal Once JavaScript API](../../../../../javascript/drupal/once/index.md).

```javascript
// Core calls once()
once('once-id', '[data-drupal-selector="element"]');

// In a contrib or custom code, jQuery.once will work as expected: 
$('[data-drupal-selector="element"]').once('once-id') // will return an empty set.
```

### Необходимые изменения в коде

**Ранее:**

```yaml
# mymodule.libraries.yml
myfeature:
  js:
    js/myfeature.js: { }
  dependencies:
    - core/drupal
    - core/jquery
    - core/jquery.once
```

```javascript
# js / myfeature.js
(function ($, Drupal) {
    Drupal.behaviors.myfeature = {
        attach(context) {
            const $elements = $(context).find('[data-myfeature]').once('myfeature');
            // `$elements` is always a jQuery object.
            $elements.each(processingCallback);
        }
    };

    function processingCallback(index, value) {
    }
}(jQuery, Drupal));
```

**После (удаляем jQuery зависимость):**

```yaml
# mymodule.libraries.yml
myfeature:
  js:
    js/myfeature.js: { }
  dependencies:
    - core/drupal
    - core/once
```

```javascript
# js / myfeature.js
(function (Drupal, once) {
    Drupal.behaviors.myfeature = {
        attach(context) {
            const elements = once('myfeature', '[data-myfeature]', context);
            // `elements` is always an Array.
            elements.forEach(processingCallback);
        }
    };

    // The parameters are reversed in the callback between jQuery `.each` method 
    // and the native `.forEach` array method.
    function processingCallback(value, index) {
    }
}(Drupal, once));
```

## `drupal_get_path()` и `drupal_get_filename()` объявлены устаревшими

- [#2347783](https://www.drupal.org/node/2347783)

Функции `drupal_get_path()` и `drupal_get_filename()` [объявлены устаревшими](../../../../../deprecation/index.md).
Вместо данных функций используйте, где это возможно, специальные
методы `\Drupal\Core\Extension\ExtensionList::getPath()` и `\Drupal\Core\Extension\ExtensionList::getPathname()`. Вы
можете использовать `extension.path.resolver:getPath()` и `extension.path.resolver:getPathname()` там где
поддерживается [Dependency Injection](../../../../9/services/dependency-injection/index.md).

Данные методы выбрасывают следующие исключения:

* `\Drupal\Core\Extension\Exception\UnknownExtensionException` если расширение не найдено.
* `\Drupal\Core\Extension\Exception\UnknownExtensionTypeException` если тип расширения не найден.

Ниже представлен список того, как необходимо обновить свой код:

* `drupal_get_path()`
    * `drupal_get_path('module', 'node')` → `\Drupal::service('extension.list.module')->getPath('node')`
    * `drupal_get_path('module', 'node')` → `\Drupal::service('extension.path.resolver')->getPath('module', 'node')`
    * `drupal_get_path('theme', 'seven')` → `\Drupal::service('extension.list.theme')->getPath('seven')`
    * `drupal_get_path('profile', 'standard')` → `\Drupal::service('extension.list.profile')->getPath('standard')`
* `drupal_get_filename()`
    * `drupal_get_filename('module', 'node')` → `\Drupal::service('extension.list.module')->getPathname('node')`
    * `drupal_get_filename('module', 'node')`
      → `\Drupal::service('extension.path.resolver')->getPathname('module', 'node')`
    * `drupal_get_filename('theme', 'seven')` → `\Drupal::service('extension.list.theme')->getPathname('seven')`
    * `drupal_get_filename('profile', 'standard')`
      → `\Drupal::service('extension.list.profile')->getPathname('standard')`

## `GuzzleMiddlewarePass` заменён на `TaggedHandlersPass`

- [#3215397](https://www.drupal.org/node/3215397)

`GuzzleMiddlewarePass` отвечал за сбор сервисов с метками `http_client_middleware` и передавал их сервису
с `Guzzle` (`http_client`).

`TaggedHandlersPass` выполняет ту же работу в общем виде - он работает для любого сервиса с меткой - поэтому теперь этот
обработчик используется вместо него, а `GuzzleMiddlewarePass` был удален.

Если вы использовали или расширяли `GuzzleMiddlewarePass`, вам следует перейти на `TaggedHandlersPass`.

## Функция `file_build_uri()` объявлена устаревшей

- [#3223016](https://www.drupal.org/node/3223016)

Функция `file_build_uri()` была объявлена устаревшей. Прямой замены данной функции не предоставлено.

Данная функция делает две вещи: объединяет схему по умолчанию (как правило `public://`) с предоставленным путём, а затем
нормализует его.

**Ранее:**

```php
$uri = file_build_uri($path);
```

**Теперь:**

```php
$uri = \Drupal::config('system.file')->get('default_scheme') . '://' . $path;
/** @var \Drupal\Core\StreamWrapper\StreamWrapperManagerInterface $stream_wrapper_manager */
$stream_wrapper_manager = \Drupal::service('stream_wrapper_manager');
$uri = $stream_wrapper_manager->normalizeUri($uri);
```

Однако нормализация пути не нужна, если это жестко заданная папка модуля, например `file_build_uri('my_module')` можно
заменить лишь одной строкой `\Drupal::config('system.file')->get('default_scheme') . '://my_module'`.

Для настраиваемых путей рекомендуется включать обертку потока явно, либо как отдельный параметр конфигурации, подобно
полям файлов, либо объединенный в текстовом поле.

## Добавлена поддержка oEmbed-ресурсов без явной высоты

- [#2966043](https://www.drupal.org/node/2966043)

Ранее система oEmbed требовала, чтобы все ресурсы без ссылок (т.е. фотографии, видео, текст) имели явную, ненулевую
ширину и высоту, определенные в соответствии со спецификацией oEmbed.

Однако некоторые поставщики oEmbed, такие как Twitter и Instagram, не следуют этому правилу, потому что ресурсы, которые
они обслуживают, содержат текст и могут быть адаптивными, поэтому они не могут иметь известную, заранее определенную
высоту.

Для того чтобы Drupal мог работать с такими провайдерами, начиная с версии 9.3.0 высота ресурса, не являющегося ссылкой,
теперь является необязательной, и если она не указана, то будет возвращаться к максимальной высоте, установленной в
форматере.

## oEmbed сервисы теперь запрашивают бэкенд кеша

- [#3222632](https://www.drupal.org/node/3222632)

Медиа система имеет три службы, которые вместе обеспечивают поддержку oEmbed:

* `\Drupal\media\OEmbed\ResourceFetcher`
* `\Drupal\media\OEmbed\ProviderRepository`
* `\Drupal\media\OEmbed\UrlResolver`

Ранее каждый из этих сервисов принимал необязательный параметр `$cache_backend`, который мог быть `NULL` или
экземпляром `\Drupal\Core\Cache\CacheBackendInterface`. Они использовали `\Drupal\Core\Cache\UseCacheBackendTrait`,
чтобы вызывающий код мог включать/выключать кеширование.

Начиная с текущего изменения, бэкенд кеша всегда должен быть предоставлен, и сервисы больше не
используют `UseCacheBackendTrait`. Чтобы отключить кеширование, передайте экземпляр `\Drupal\Core\Cache\NullBackend`.
Подклассы должны напрямую вызывать `$this->cacheBackend->get()` и `$this->cacheBackend->set()` соответственно.

## Типы ресурсов JSON:API теперь могут быть программно переименованы

- [#3105318](https://www.drupal.org/node/3105318)

Некоторые сайты могут захотеть раскрыть определенные типы ресурсов JSON:API под определенными именами.

В предыдущих версиях было невозможно переименовывать типы ресурсов, так как не было публичного PHP API, однако некоторые
сторонние и пользовательские модули могли задекорировать класс `Drupal\jsonapi\ResourceType\ResourceTypeRepository`.
Такой подход означал перезапись методов и приводил к сложному в поддержке решению. Например, `jsonapi_extras`
значительно переопределяет класс, чтобы переименовать типы ресурсов.

JSON:API `ResourceTypeBuildEvent` теперь имеет новый метод: `::setResourceTypeName(string $resource_type_name)`
. [Подписчики этого события](../../../../9/events/index.md) теперь могут переименовать ресурс, используя публично
поддерживаемый PHP API.

Смотрите `\Drupal\jsonapi_test_resource_type_building\EventSubscriber\ResourceTypeBuildEventSubscriber` для примера
того, как добавить синоним имени типа ресурса.

Была введена новая константа: `Drupal\jsonapi\ResourceType::TYPE_NAME_URI_PATH_SEPARATOR`. Эта константа представляет
собой строку, которая используется в качестве разделителя путей в именах типов ресурсов.
Функция `Drupal\jsonapi\ResourceType::getPath()` заменяет "--" на "/" в uri пути. Пример: `node--article`
→ `node/article`.

## Добавлена передача состояния Media Library в `hook_ENTITY_TYPE_create_access()`

- [#3224530](https://www.drupal.org/node/3224530)

Модуль Media Library будет передавать в `hook_ENTITY_TYPE_create_access()` объект-значение, содержащий его текущее
состояние (экземпляр `Drupal\media_library\MediaLibraryState`) в параметре `$context` при определении доступа к секции "
add media" медиатеки. Это позволит сторонним и пользовательским модулям реализовать сложную логику доступа, например,
для формы:

```php
use Drupal\Core\Session\AccountInterface;

function mymodule_media_create_access(AccountInterface $account, array $context, $entity_bundle) {
  if (isset($context['media_library_state'])) {
    // $context['media_library_state'] is an instance of Drupal\media_library\MediaLibraryState. Use it as needed to
    // make an access decision.
  }
}
```

Для ясности, полученное решение о доступе будет применяться только к разделу медиатеки "Добавить медиа" (т.е. к той
части, которая позволяет пользователю создавать новые медиа элементы), как в виджете поля, так и в CKEditor. Оно НЕ
влияет на доступ к разделу медиатеки, позволяющему выбирать существующие медиафайлы, или на доступ к медиатеке в целом.

## Сторонние JavaScript библиотеки теперь управляются через `package.json`

- [#3219088](https://www.drupal.org/node/3219088)

Для разработки ядра мы теперь используем `package.json` и дополнительный шаг сборки для добавления сторонних скриптов в
ядро Drupal. Это сделано для того, чтобы облегчить обновление и упростить процесс обновления. В настоящее время все
библиотеки управляются таким образом, кроме CKEditor и modernizr, для которых требуются пользовательские сборки, которые
на данный момент не могут быть легко автоматизированы.

**Ранее:**

1. Проверить страницу каждой библиотеки, чтобы узнать, доступна ли новая версия.
2. Скачать последнюю версию библиотеки с github, npm или с сайта библиотеки.
3. Обновить ядро и создать патч.

**Теперь:**

1. Запустить `yarn outdated` чтобы проверить наличие обновлений.
2. Запустить `yarn upgrade <library>` для каждой библиотеки что необходимо обновить.
3. Запустить `yarn vendor-update` которая обновит библиотеку в `core/assets/vendor` директории, а также информацию
   в `core.libraries.yml`.
4. Отправить изменения в ядро.

## Drupal ядро больше не использует doctrine/reflection

- [#3180351](https://www.drupal.org/node/3180351)

Пакет `doctrine/reflection` больше не используется Drupal ядром потому что пакет заброшен и больше не поддерживается.
Код из данного пакета на который опирается Drupal был скопирован в `Drupal\Component\Annotation\Doctrine`. Чтобы
использовать этот код в сторонних и собственных модулях, вам требуется обновить пространства имён:

* `Doctrine\Common\Reflection\StaticReflectionParser` → `Drupal\Component\Annotation\Doctrine\StaticReflectionParser`
* `Doctrine\Common\Reflection\ClassFinderInterface` → `Drupal\Component\ClassFinder\ClassFinderInterface`

Пакет `doctrine/reflection` останется в зависимостях до [Drupal 10](../../../../10/index.md) где и будет удалён. Однако,
если вы хотите получить полную совместимость со всеми версиями PHP, код придется отрефакторить раньше, так как пакет не
совместим с PHP 8.1.

## Добавлен слой совместимости для InputBag Symfony

- [#3162016](https://www.drupal.org/node/3162016)

Symfony 5.2 представил `Symfony\Component\HttpFoundation\InputBag` для
замены `Symfony\Component\HttpFoundation\ParameterBag` в некоторых случаях, и
метод `Symfony\Component\HttpFoundation\InputBag::get()` был объявлен устаревшим для нестроковых возвращаемых значений.
Поэтому для получения нестрокового возвращаемого значения вместо него следует
использовать `Drupal\Core\Http\InputBag::all()`. Класс `Drupal\Core\Http\InputBag` будет заменен
классом `Symfony\Component\HttpFoundation\InputBag`, когда Symfony 4 перестанет поддерживаться Drupal.

Основное отличие в том что `::all()` теперь может принимать параметр.

**Ранее:**

```php
$ajax_page_state = $request->request->get('ajax_page_state', []);
```

Второй параметр опциональный и используется как значение по умолчанию если значение по ключу первого параметра
отсутствует.

**Сейчас:**

Вариант #1:

```php
$ajax_page_state = $request->request->all('ajax_page_state');
```

* Если значение по ключу не существует, метод вернёт пустой массив.
* Если значение по ключу существует и не является массивом, тогда будет выброшено исключение `UnexpectedValueException`.

Вариант #2:

```php
$ajax_page_state = $request->request->all()['ajax_page_state'] ?? NULL;
```

* Вызывая метод таким способом, он никогда не приведёт к вызову исключения `UnexpectedValueException`.
* Вызывая метод таким способом, у разработчика появляется возможность задать значение по умолчанию.

## Базовые поля сущности теперь используют стандартные шаблоны полей если у них включена настройка отображения

- [#3043840](https://www.drupal.org/node/3043840)

Это изменение затрагивает только те сайты, которые включили настраиваемое отображение базовых полей типов
материалов (`title`, `uid` и `created`) с помощью хука:

```php
/**
 * Implements hook_entity_base_field_info_alter().
 */
function examplemodule_entity_base_field_info_alter(&$base_field_definitions, EntityTypeInterface $entity_type) {
  if ($entity_type->id() == 'node') {
    $base_field_definitions['created']->setDisplayConfigurable('view', TRUE);
  }
} 
```

### Ранее

Для этих базовых полей использовался специальный шаблон поля, который отображал значения в строке и не отображал метку
или многие атрибуты и классы обычного поля.

Это поведение можно было отменить следующим образом:

```php
/**
 * Implements hook_theme_registry_alter().
 */
function mymodule_theme_registry_alter(&$theme_registry) {
  unset($theme_registry['field__node__title']);
  unset($theme_registry['field__node__uid']);
  unset($theme_registry['field__node__created']);
}
```

### Сейчас

Если включено настраиваемое отображение, будет использоваться стандартный шаблон поля. Для поддержания обратной
совместимости это происходит только в том случае, если было установлено дополнительное свойство типа сущности (
введенное [в этом изменении](https://www.drupal.org/node/2925634)):

```php
function mymodule_entity_type_build(array &$entity_types) {
  $entity_types['node']->set('enable_base_field_custom_preprocess_skipping', TRUE);
}
```

### Изменения в шаблонах

В стандартных темах (Stable, Classy, Bartik) шаблоны полей типов материалов `field--node--title.html.twig`
, `field--node--created.html.twig`, `field--node--uid.html.twig` были изменены для проверки новой
переменной `is_inline`:

```php
{% if not is_inline %}
  {% include "field.html.twig" %}
{% else %}
...
```

Разработчики тем, переопределяющие любой из этих шаблонов, должны внести соответствующие изменения. В прочих случаях,
когда тема наследует эти шаблоны от одной из тем ядра, изменения не требуются.

## Константа `FILE_STATUS_PERMANENT` объявлена устаревшей

- [#3021833](https://www.drupal.org/node/3021833)

Константа `FILE_STATUS_PERMANENT` объявлена устаревшей.

* Для проверки статуса файла используйте `\Drupal\file\FileInterface::isPermanent()`
  или `\Drupal\file\FileInterface::isTemporary()`.
* Чтобы сделать файл постоянным используйте `\Drupal\file\FileInterface::setPermanent()`.
* Константа `\Drupal\file\FileInterface::STATUS_PERMANENT` является внутренней заменой и не предназначена для
  использования сторонними разработчиками.

**Ранее:**

```php
$fids = Drupal::entityQuery('file')
  ->condition('status', FILE_STATUS_PERMANENT, '<>')
...
```

```php
if ($file->status->value !== FILE_STATUS_PERMANENT) {
  $file->status->value = FILE_STATUS_PERMANENT;
  $file->save();
}
```

**Теперь:**

```php
use Drupal\file\FileInterface;
...
$fids = Drupal::entityQuery('file')
  ->condition('status', FileInterface::STATUS_PERMANENT, '<>')
```

```php
if ($file->isTemporary()) {
  $file->setPermanent();
  $file->save();
}
```

## «Стандартный» профиль больше не устанавливает Quick Edit

- [#3227039](https://www.drupal.org/node/3227039)

Модуль Quick Edit планируется удалить из ядра в Drupal 10, основываясь на данных, которые указывают на то, что модуль
нечасто используется целевой аудиторией, а также на многочисленных технических, юзабилити и ограничениях доступности
текущего модуля.

В связи с этим Quick Edit был удален из [«Стандартного» профиля](../../../../9/distributions/standard/index.md) в Drupal
9.3.0. Это изменение не затрагивает существующие сайты, только новые сайты, впервые устанавливающие профиль. Сайты
Drupal 9.3.x могут продолжать использовать этот модуль, а для сайтов Drupal 10 он будет доступен как сторонний (contrib)
модуль, хотя в долгосрочной перспективе рекомендуется рассмотреть альтернативные варианты.

Стандартный профиль будет продолжать предоставлять Contextual Links (модуль, который обеспечивает выпадающее меню с
иконкой карандаша, позволяющее открыть форму редактирования фрагмента контента). Владельцы сайта могут также рассмотреть
альтернативные решения для редактирования на месте, например, модуль [Geysir](https://www.drupal.org/project/geysir).

## `ProviderRepository` теперь запрашивает сервисы `keyvalue` и `logger.factory`

- [#3186184](https://www.drupal.org/node/3186184)

Начиная с этого изменения класс `Drupal\media\OEmbed\ProviderRepository` инициализируется с сервисами `keyvalue`
и `logger.factory`. Он больше не принимает `CacheBackendInterface` и при попытке передачи такого аргумента будет
выведено сообщение об устаревшем параметре.

Ранее, если вы инициализировали `ProviderRepository` передавая время истечения кеша, например 1000 секунд, вы могли
сделать следующее:

```php
new ProviderRepository($http_client, $config_factory, $time, $cache_backend, 1000);
```

Чтобы сделать то же самое сейчас:

```php
new ProviderRepository($http_client, $config_factory, $time, $key_value_factory, $logger_factory, 1000);
```

## ESLint теперь используется для валидации YAML

- [#2591827](https://www.drupal.org/node/2591827)

Плагин [eslint-plugin-yml](https://github.com/ota-meshi/eslint-plugin-yml) был добавлен в ESLint и скрипт прекоммита
ядра, чтобы убедиться, что весь YAML в кодовой корректно отформатировано.

Запуск `core/scripts/dev/commit-code-check.sh` проверит все измененные файлы YAML.

Запуск `yarn lint:yaml` из каталога core проверит все YAML файлы в ядре.

## Контроллеры, которые обращаются к `$_SESSION`, теперь принимают параметр `Request`

- [#2473875](https://www.drupal.org/node/2473875)

Контроллеры, которые ранее обращались к суперглобальному `$_SESSION`, теперь принимают параметр Symfony `Request` для
своих методов контроллера. Теперь они получают доступ к сессии с помощью `$request->getSession()`.

**Ранее:**

```php
\Drupal\dblog\Controller\DbLogController::overview()
\Drupal\migrate_drupal_ui\Controller\MigrateController::showLog()
\Drupal\user\Controller\UserController::resetPassLogin()
```

**Сейчас:**

```php 
\Drupal\dblog\Controller\DbLogController::overview(Request $request)
\Drupal\migrate_drupal_ui\Controller\MigrateController::showLog(Request $request)
\Drupal\user\Controller\UserController::resetPassLogin(Request $request)
```

## Настройка «Роль администратора» перемещена в новую форму «Настройки роли» по адресу /admin/people/role-settings

- [#987978](https://www.drupal.org/node/987978)

Новая форма «Настройки роли» теперь доступна по пути `admin/people/role-settings` как еще одна вкладка в разделе
«Пользователи» (рядом с «Роли» и «Разрешения»), для доступа к которой требуется разрешение `'administer permissions'`.

Это новый адрес для настройки «Роль администратора», которая раньше находилась по пути `admin/config/people/accounts` в
форме «Настройки учетной записи». Эта настройка позволяет дать управление над правами доступа, поэтому для ее изменения
пользователю теперь необходимо иметь права администратора.

## Кеш-теги и кеш-контексты могут быть не отсортированы

- [#3225328](https://www.drupal.org/node/3225328)

Теги и контексты кеша всегда сортировались независимо от использования, чтобы сделать отладку и тестирование более
удобными, однако это добавляло нагрузку на производительность при каждом HTML-запросе.

В целом это не повлияет на код во время выполнения, однако в некоторых тестах может потребоваться учитывать различные
сортировки тегов/контекстов, в большинстве случаев изменения будут выглядеть следующим образом:

**Ранее:**

```php
$this->assertEquals($expected_top_level_contexts, $element['#cache']['contexts'], 'Expected cache contexts found.');
```

**Сейчас:**

```php
$this->assertEqualsCanonicalizing($expected_top_level_contexts, $element['#cache']['contexts'], 'Expected cache contexts found.');
```

## Возможность указывать префиксы таблиц объявлена устаревшей

- [#2768219](https://www.drupal.org/node/2768219)

Возможность задавать префиксы для каждой таблицы индивидуально объявлена устаревшей и будет удалена в Drupal 10. После
этого будет поддерживаться только один префикс для всех таблиц.

Альтернативы:

* **Пользователи, сессии и/или пользовательские роли общие для нескольких сайтов.** В качестве альтернативы можете
  использовать модули, которые позволяют организовать авторизацию в обход Drupal. Несколько примеров подобных
  модулей: [Lightweight Directory Access Protocol (LDAP)](https://www.drupal.org/project/ldap)
  , [CAS](https://www.drupal.org/project/cas)
  , [simpleSAMLphp Authentication](https://www.drupal.org/project/simplesamlphp_auth)
  , [OpenID Connect](https://www.drupal.org/project/openid_connect)
  и [Bakery Single Sign-On System](https://www.drupal.org/project/bakery).
* Для более продвинутых сценариев, где таблицы должны копироваться или мигрировать, но вы хотите использовать API ядра:
  добавьте настройки подключения для глобального префикса, затем заинжектите эти подключения в необходимые классы.
* **Синхронизация содержимого и/или конфигураций.** Синхронизация конфигураций поддерживается Drupal ядром, но
  синхронизация содержимого чуть сложнее. На данный момент
  существует [инициатива для добавления синхронизации содержимого в ядро](https://www.drupal.org/project/ideas/issues/2721129)
  . Как альтернативу, вы можете рассмотреть модуль [domain](https://www.drupal.org/project/domain)
  или [deploy](https://www.drupal.org/project/deploy).

## Информация о количестве данных в источнике миграции теперь кешируется

- [#3190818](https://www.drupal.org/node/3190818)

Плагины-источники теперь могут кешировать результат подсчёта количества элементов используя метод `::doCount()`. Это
изменение повлияет на плагины расширяющие `SourcePluginBase` и переопределяющие метод `::count()`. Если вы используете
данный метод, конвертируйте его в `::doCount()` чтобы получить положительный эффект от нового кеширования.

<Aside>

Технически, это изменение API. Старое поведение было задумано быть таким, как сейчас. Но в [#3190815](https://www.drupal.org/project/drupal/issues/3190815) выяснилось что все классы наследующиеся от `SqlBase` или `DrupalSqlBase` не кешировали результат, потому что `SqlBase::count()` переопределял `SourcePluginBase::count()`.

</Aside>

**Ранее:**

```php
  /**
   * {@inheritdoc}
   */
  public function count($refresh = FALSE) {
    ...
  }
```

**Сейчас:**

```php
  /**
   * {@inheritdoc}
   */
  protected function doCount() {
    ...
  }
```

## `Drupal.tabbingManager` теперь позволяет перехватывать фокус

- [#3229828](https://www.drupal.org/node/3229828)

До этого изменения `Drupal.tabbingManager` мог ограничить переход по определённым элементам, но не мог перехватить фокус
внутри этих элементов. Например, переход к последнему элементу или предыдущему, находясь на первом элементе, приводил к
тому, что фокус полностью покидал область просмотра браузера.

`Drupal.tabbingManager` теперь имеет возможность перехватывать фокус, поэтому переход к последнему элементу установит
фокус на первый элемент набора, а переход с первого элемента, сместит фокус на последний элемент. Это полезно для таких
элементов как диалоговые окна, в которых переходы должны быть цикличны до тех пор, пока диалог не будет закрыть.

Поведение по умолчанию для `Drupal.tabbingManager` остаётся неизменным. Перехват фокусировки должен быть явно настроен
при вызове `Drupal.tabbingManager`. `Drupal.tabbingManager.constrain` теперь принимает второй аргумент: объект, который
может включать свойство `trapFocus`. При значении `true` фокусировка будет перехвачена в пределах выбранных элементов.

**Пример:**

```
Drupal.tabbingManager.constrain(element, { trapFocus: true });
```

## Плагины-источники oEmbed теперь принимают `Token`

- [#3026184](https://www.drupal.org/node/3026184)

Данное изменение добавляет ограниченную поддержку токенов в пути для хранения превью oEmbed ресурсов, например, у
внешних видео.

В связи с данным изменением, класс `\Drupal\media\Plugin\media\Source\OEmbed` теперь
ожидает `token` [сервис](../../../../9/services/index.md) в своём конструкторе.

## `\Drupal\Core\Cache\DatabaseCacheTagsChecksum::catchException()` объявлен устаревшим

- [#3240601](https://www.drupal.org/node/3240601)

Защищённый метод `\Drupal\Core\Cache\DatabaseCacheTagsChecksum::catchException()` объявлен устаревшим без замены. Если у
вас имеется код, который расширяет `\Drupal\Core\Cache\DatabaseCacheTagsChecksum` и
вызывает `\Drupal\Core\Cache\DatabaseCacheTagsChecksum::ensureTableExists()`, в таком случае ваш код должен
самостоятельно выбрасывать исключение.

**Ранее:**

```php
    try {
      foreach ($tags as $tag) {
        $this->connection->merge('cachetags')
          ->insertFields(['invalidations' => 1])
          ->expression('invalidations', '[invalidations] + 1')
          ->key('tag', $tag)
          ->execute();
      }
    }
    catch (\Exception $e) {
      // Create the cache table, which will be empty. This fixes cases during
      // core install where cache tags are invalidated before the table is
      // created.
      if (!$this->ensureTableExists()) {
        $this->catchException($e);
      }
    }
```

**Сейчас:**

```php
    try {
      foreach ($tags as $tag) {
        $this->connection->merge('cachetags')
          ->insertFields(['invalidations' => 1])
          ->expression('invalidations', '[invalidations] + 1')
          ->key('tag', $tag)
          ->execute();
      }
    }
    catch (\Exception $e) {
      // Create the cache table, which will be empty. This fixes cases during
      // core install where cache tags are invalidated before the table is
      // created.
      if (!$this->ensureTableExists()) {
        throw $e;
      }
    }
```

## Миграции переводимых конфигураций должны указывать свойство `translations` равным `true`

- [#3239298](https://www.drupal.org/node/3239298)

YAML файлы миграций для переводов конфигураций теперь требуют указывать свойство `translations` равным `true` в своём
«назначении».

Следующие миграции из ядра были обновлены должным образом:

* `d6_block_translation.yml`
* `d6_taxonomy_vocabulary_translation.yml`
* `d7_block_translation.yml`
* `d7_field_instance_label_description_translation.yml`
* `d7_menu_translation.yml`
* `d7_taxonomy_vocabulary_translation.yml`

Благодаря данному изменению, данные миграции теперь поддерживают возможность отката миграций.

**Ранее:**

```yaml
 destination:
   plugin: entity:taxonomy_vocabulary
   destination_module: config_translation
```

**Сейчас:**

```yaml
 destination:
   plugin: entity:taxonomy_vocabulary
   destination_module: config_translation
   translations: true
```

## Система плагинов-контекстов теперь воспринимает `FALSE` как валидное значение

- [#3194768](https://www.drupal.org/node/3194768)

Ранее, система плагинов-контекстов возвращала `FALSE` при вызове метода `::hasContextValue()` на объект контекста
который возвращал значение FALSE, даже если данное значение является валидным для контекста.

Теперь, метод `::hasContextValue()` будет возвращать `TRUE` в таких случаях, только значение контекста `NULL` приведёт к
результату `FALSE`.

**Ранее:**

```php
$definition = new \Drupal\Core\Plugin\Context\ContextDefinition('boolean');
$context = new \Drupal\Component\Plugin\Context\Context($definition, FALSE);
$context->hasContextValue(); // Would return FALSE
```

**Теперь:**

```php
$definition = new \Drupal\Core\Plugin\Context\ContextDefinition('boolean');
$context = new \Drupal\Component\Plugin\Context\Context($definition, FALSE);
$context->hasContextValue(); // Now returns TRUE
```

## Функция `_file_save_upload_single()` объявлена устаревшей и перенесена в сервис `file.upload_handler`

- [#3232248](https://www.drupal.org/node/3232248)

Функция `_file_save_upload_single()` объявлена устаревшей и перенесена в сервис `file.upload_handler`.

**Ранее:**

```php
$file = _file_save_upload_single($file_info, $form_field_name);
```

**Теперь:**

```php
/** @var \Drupal\file\Upload\FileUploadHandler $file_upload_handler */
$file_upload_handler = \Drupal::service('file.upload_handler');
$result = $file_upload_handler->handleFileUpload($uploadedFile);
$file = $result->getFile();
```

## Плагин-источник `MenuLink` теперь имеет настройку `menu_name`

- [#3064016](https://www.drupal.org/node/3064016)

Плагин источник `menu_link` теперь имеет настройку `menu_name`, которая может быть использована для фильтрации ссылок
источника по названию меню. Значение может быть либо строкой, либо массивом.

**Примеры:**

```yaml
source:
  plugin: menu_link
  menu_name: main-menu
```

```yaml
source:
  plugin: menu_link
  menu_name: [ main-menu, navigation ]
```

## Разрешения теперь могут быть просмотрены и отредактированы для конкретного модуля или набора модулей

- [#3236443](https://www.drupal.org/node/3236443)

Ранее, администраторы сайта могли просматривать и редактировать все разрешения для всех ролей по
адресу `/admin/people/permissions`, или для одной конкретной роли по `/admin/people/roles/manage/authenticated` (для
роли «Авторизованный»).

Начиная с текущего изменения, администраторы сайта могу просматривать и редактировать разрешения для одного
единственного модуля. Например, разрешения для модуля Node будут доступны по
адресу `/admin/people/permissions/module/node`. Если модуль не предоставляет никаких разрешений, например как модуль
«Custom Block», в таком случае страница будет отвечать HTTP 403 даже дял администраторов.

Ссылки на странице «Расширения» (`/admin/modules`) теперь ведут на соответствующие страницы конкретного модуля, вместо
страницы со всеми правами и якорем на модуль.

Ссылки в документациях теперь также ведут на соответствующие страницы вместо якорных ссылок.

Данный маршрут также поддерживает возможность передачи нескольких модулей для того чтобы показать только их разрешения,
например `/admin/people/permissions/module/action,node` — покажет разрешения только для модулей «Node» и «Action».

Когда администратор включает один и более модулей, и как минимум один из них предоставляет разрешения, сообщение об
успешной активации будет иметь ссылку на настройку разрешения только что включённых модулей.

## Проверка доступа `_node_add_access` объявлена устаревшей в пользу `_entity_create_access` и `_entity_create_any_access`

- [#2744381](https://www.drupal.org/node/2744381)

Проверка доступа `_node_add_access` объявлена устаревшей в пользу `_entity_create_access` и `_entity_create_any_access`.

Пример изменений:

```diff
@@ -13,7 +13,7 @@ node.add_page:
   options:
     _node_operation_route: TRUE
   requirements:
-    _node_add_access: 'node'
+    _entity_create_any_access: 'node'
 
 node.add:
   path: '/node/add/{node_type}'
@@ -21,7 +21,7 @@ node.add:
     _controller: '\Drupal\node\Controller\NodeController::add'
     _title_callback: '\Drupal\node\Controller\NodeController::addPageTitle'
   requirements:
-    _node_add_access: 'node:{node_type}'
+    _entity_create_access: 'node:{node_type}'
   options:
     _node_operation_route: TRUE
     parameters:
```

## `LayoutTempstoreParamConverter` был разделён на `LayoutSectionStorageParamConverter` и `LayoutTempstoreRouteEnhancer`

- [#3032433](https://www.drupal.org/node/3032433)

`\Drupal\layout_builder\Routing\LayoutTempstoreParamConverter` был удалён и разделён на несколько классов:

* `\Drupal\layout_builder\Routing\LayoutSectionStorageParamConverter`: загружает хранилище секции для указанного маршрута, основываясь на типе и доступных контекстах.
* `\Drupal\layout_builder\Routing\LayoutTempstoreRouteEnhancer`: загружает версию хранилища секций из временного хранилища.

## Теперь для валидации в `EntityContentBase` используется другой пользователь

- [#3134470](https://www.drupal.org/node/3134470)

Плагины назначений для миграции сущностей позволяют разработчикам включить валидацию сущности в процессе миграции.

Ранее сущность могла не пройти проверку, если пользователь, под которым выполнялась миграция, мог не иметь прав на установку значения конкретного поля, даже если пользователь, изначально создавший сущность, имел такие права. Эта проблема часто встречается при запуске миграций при помощи Drush, так как пользователь, из-под которого запускаются миграции в таком случае, является анонимным. Например, если анонимный пользователь не имеет права использовать определённый текстовый формат, но исходная сущность использует его в текстовом поле, импортированная сущность не пройдёт проверку, поскольку формат не является допустимым для анонимного пользователя.

После введения этого изменения, система миграций будет временно переключать учетную запись текущего пользователя на владельца сущности перед проверкой. Это поведение будет доступно для всех сущностей что реализуют `EntityOwnerInterface`. Для подобного переключения учетных записей используется сервис `account_switcher`. Поэтому все плагины миграции, которые расширяют `Drupal\migrate\Plugin\migrate\destination\EntityContentBase` или его производные, будут требовать передачи сервиса `account_switcher` в своих конструкторах в [Drupal 10](../../../../10/index.md).

## Бандлы (подтипы) сущностей теперь могут иметь свои собственные классы

- [#2570593](https://www.drupal.org/node/2570593) 

Бандлы (подтипы) сущностей по сути являются бизнес-объектами, и теперь они могут объявлять свой собственный класс, инкапсулируя необходимую бизнес-логику. Класс бандла должен быть подклассом базового класса сущности, например,  `\Drupal\node\Entity\Node`. Модули могут определять классы для бандлов для своих сущностей при помощи свойства `class` в `hook_entity_bundle_info()`, а также изменять существующие классы бандлов для сущностей, определённых другими модулями при помощи `hook_entity_bundle_info_alter()`. Каждый подкласс бандла должен быть уникальным. Если вы попытаетесь переиспользовать один и тот же подкласс для нескольких бандлов, будет вызвано исключение.

### Возможности

Инкапсуляция всей необходимой логики для каждого бандла в отдельный подкласс открывает множество возможностей для создания более понятного, простого, удобного в обслуживании и тестировании кода. Сам Drupal не имеет представления о том, как вы должны структурировать свой код, но это изменение позволяет вам использовать любой объектно-ориентированный стиль/дизайн/парадигму, которую вы предпочитаете.

#### Переход от прерпроцессинга шаблонов к get*() методам

Класс Drupal `TwigSandboxPolicy` позволяет шаблонам Twig напрямую вызывать любой публичный метод сущности, начинающийся со слова `get`. Таким образом, вместо того, чтобы распылять сложную бизнес-логику в различных функциях препроцессинга по всему сайту, вы можете поместить эту логику непосредственно в подкласс `bundle` и затем вызывать ее прямо из ваших шаблонов Twig.

Например, если подкласс бандла определяет публичную функцию `getByline()`: метод, возвращающий строку, то шаблон Twig для режима отображения может вывести значение напрямую с помощью: `{{ node.getByline() }}`.

#### Повторное использование кода

Поскольку это обычные PHP-классы, существует множество способов повторного использования кода между подклассами пакета. Вы можете определить абстрактный базовый класс для вашего проекта (и расширить его из класса сущности по умолчанию), а затем определить подклассы для каждого бандла и зарегистрировать их все. Тогда общий код будет находиться в базовом классе, а код, специфичный для бандла — в дочерних классах.

Вы можете активно использовать интерфейсы и трейты PHP. Каждый подкласс бандла будет иметь свой собственный интерфейс, расширяющий `\Drupal\node\NodeInterface` и любые другие пользовательские интерфейсы, подходящие для данного подтипа. Каждый пользовательский интерфейс будет сопровождаться трейтом, который содержит весь общий код для реализации интерфейса. Подклассы бандла затем расширяют базовый класс сущности и используют все трейты для всех пользовательских интерфейсов, которые они дополнительно реализуют.

#### Написание автоматизированных тестов

Размещение пользовательской логики в подклассах бандлов значительно упрощает написание автоматизированных тестов, поскольку код находится в классе, а не разбросан по многочисленным реализациям процедурных хуков, методов препроцесса и так далее. Теперь вы можете активно использовать тесты ядра и даже юнит-тесты, вместо того чтобы полагаться на сквозные функциональные тесты или тесты `FunctionalJavascript` (которые требуют гораздо больше ресурсов и выполняются медленнее). Теперь делать это стало намного проще.

### Примеры

Например, пользовательский модуль может объявить класс бандла для типа материала (`node`) следующим образом:

```php
use Drupal\mymodule\Entity\BasicPage;

/**
 * Implements hook_entity_bundle_info_alter().
 */
function mymodule_entity_bundle_info_alter(array &$bundles): void {
  if (isset($bundles['node']['page'])) {
    $bundles['node']['page']['class'] = BasicPage::class;
  }
}
```

#### Интерфейсы и трейты

Вы можете определить интерфейс для конкретной пользовательской логики, например, для поддержки поля `body`:

```php
namespace Drupal\mymodule\Entity;

interface BodyInterface {

  /**
   * Returns the body.
   *
   * @return string
   */
  public function getBody(): string;

}
```

Затем можно создать интерфейс для связки в пользовательском модуле, расширяющий как `NodeInterface`, так и пользовательский `BodyInterface`:

```php
namespace Drupal\mymodule\Entity;

use Drupal\node\Entity\NodeInterface;

interface BasicPageInterface extends NodeInterface, BodyInterface {

}
```

Сами классы бандлов расширяют класс сущности, но реализуют любые дополнительные необходимые методы из других интерфейсов, которые они предоставляют.

```php
namespace Drupal\mymodule\Entity;

use Drupal\node\Entity\Node;

class BasicPage extends Node implements BasicPageInterface {

  // Implement whatever business logic specific to basic pages.
  public function getBody(): string {
    return $this->get('body')->value;
  }

}
```

В качестве альтернативы, реализация `getBody()` может быть в `BodyTrait`, который был бы общим для нескольких подклассов бандлов.

#### Использование абстрактного базового класса

Вы можете начать вводить общую функциональность для всех типов материалов с помощью абстрактного класса. Этот подход требует определения подкласса bundle для каждого типа материала на сайте (или каждого бандла любого типа сущностей, для которых вы используете этот класс: `media` и т.д.).

Ввод общего интерфейса:

```php
namespace Drupal\myproject\Entity;

use Drupal\node\NodeInterface;

class MyProjectNodeInterface extends NodeInterface {

  public function getTags(): array;

  public function hasTags(): bool;

}
```

Затем вводим абстрактный базовый класс:

```php
namespace Drupal\myproject\Entity;

use Drupal\node\Entity\Node;

abstract class MyProjectNodeBase extends Node implements MyProjectNodeInterface {
  public function getTags: array {
    return [];
  }
  public function hasTags: bool {
    return FALSE;
  }
}
```

И даже трейты:

```php
namespace Drupal\myproject\Entity;

trait NodeWithTagsTrait {

  public function getTags(): array {
   // Put a real implementation here.
    return $this->get('tags')->getValue();
  }

  public function hasTags(): bool {
    return TRUE;
  }

}
```

После чего вы можете переписать класс для обычной страницы следующим образом:

```php
namespace Drupal\mymodule\Entity;

use Drupal\node\Entity\Node;
use Drupal\myproject\Entity\NodeWithTagsTrait;
use Drupal\myproject\Entity\MyProjectNodeBase;

class BasicPage extends MyProjectNodeBase implements BasicPageInterface {

  use NodeWithTagsTrait

  // Implement whatever business logic specific to basic pages.
  public function getBody(): string {
    return $this->get('body')->value;
  }

}
```

#### Как код может реагировать на загруженные сущности

Обработчики хранения сущностей всегда будут возвращать классы бандлов, если это возможно. Поэтому на любом уровне системы после загрузки сущности, если её тип и бандл определяют подкласс, ваш загруженный объект сущности будет экземпляром определенного вами подкласса. Это будет работать во многих ситуациях:

* Внутри Twig шаблона `{{ node.getWhateverYouNeed() }}`.
* Объект, возвращаемый `\Drupal::routeMatch()->getParameter('node');` для маршрута с параметром  `{node}`.
* Когда вы загружаете сущность `$node = $this->entityTypeManager->getStorage('node')->load($nid);`
* и т.д.

Таким образом, мы можем полагаться на типы данных, вместо вызова `$node->bundle()`. Если понадобится, мы можем проверить, реализует ли тип материала определенный интерфейс бандла:

```php
if ($node instanceof BasicPageInterface) {
  // Do something specific to basic pages:
  ...
}
```

Мы также можем проверить, реализует ли данный подтип интерфейс конкретного поля:

```php
if ($node instanceof BodyInterface) {
  // Do something since we know there's a body, regardless of what node type it is.
  $body = $node->getBody();
  ...
}
```

Или же, если мы используем абстрактный класс из примеров выше, мы можем сделать следующее:

```php
if ($node->hasTags()) {
  // Some logic which applies only to nodes with tags.
  $tags = $node->getTags();
}
```

### Изменения API

Все эти преимущества потребовали внесения некоторых изменений в API ядра Drupal. В некоторых редких случаях пользовательскому или стороннему коду может потребоваться знать об этих изменениях. Ничего не сломается, некоторый код может вызывать предупреждения об устаревании и потребовать его обновления до релиза Drupal 10.

#### Изменения в том, как вызывается `::postLoad()` в классах сущностей

После того как сайт начнёт определять и использовать подклассы бандлов, при одновременной загрузке нескольких сущностей из разных бандлов изменился способ вызова `EntityInterface::postLoad()`. Раньше, поскольку существовал только один класс, в `::postLoad()` всегда передавался полный массив `$entities`, включая все сущности, загруженные в этой операции. Теперь метод `::postLoad()` для каждого подкласса бандла будет вызываться с подмассивом, включающим только сущности того же самого бандла. Метод `::postLoad()` в этом случае больше не может манипулировать всеми возможными сущностями (только сущностями своего вида) и не должен пытаться изменить порядок элементов (что и не предполагалось ранее, но некоторый код полагался на эту особенность).

#### Влияние на пользовательские классы хранилищ сущностей

Защищённое свойство `\Drupal\Core\Entity\EntityStorageBase::entityClass` было удалено.

##### Получение класса сущности

Если в хранилище потребуется узнать класс основной сущности, необходимо использовать метод `\Drupal\Core\Entity\EntityStorageBase::getEntityClass()`.

**Ранее:**

```php
$entity = new $this->entityClass($values, $this->entityTypeId);
```

**Сейчас:**

```php
$entity_class = $this->getEntityClass();
$entity = new $entity_class($values, $this->entityTypeId);
```

##### Установка класса сущности

Хранилища сущностей, которым необходимо изменить класс, используемый для создания сущностей, больше не могут устанавливать свойство `$this->entityClass` напрямую. Это будет вызывать предупреждение об устаревании в Drupal 9.3.0 и выше, и не будет иметь никакого эффекта, начиная с Drupal 10.0.0. Вот поддерживаемые альтернативы:

* Если возможно, используйте классы бандлов, как указано выше.
* В противном случае пусть хранилище вызывает `$this->entityType->setClass()` с нужным классом.
* Или же, подменить хранилище и переопределить `::getEntityType()`.

#### Новые исключения

Два новых исключения теперь могут быть выброшены если вы попытаетесь использовать бандлы с подклассами неправильно:

* `Drupal\Core\Entity\Exception\AmbiguousBundleClassException`: Выбрасывается когда несколько бандлов пытаются использовать один и тот же подкласс.
* `Drupal\Core\Entity\Exception\BundleClassInheritanceException`: Выбрасывается когда подкласс бандла не расширяет класс сущности, бандлом которой он является.

#### Другие изменения API

* Добавлен `Drupal\Core\Entity\BundleEntityStorageInterface` который определяет метод `::getBundleFromClass()`.
* Добавлена реализация `Drupal\Core\Entity\ContentEntityStorageBase::getBundleFromClass()`.
* `Drupal\Core\Entity\ContentEntityBase` теперь реализует `public static create()`.
* `Drupal\Core\Entity\ContentEntityStorageBase` теперь реализует `public static create()`.

## Функции `file_save_date()`, `file_copy()` и `file_move()` объявлены устаревшими

- [#3223209](https://www.drupal.org/node/3223209)

Функции `file_save_date()`, `file_copy()` и `file_move()` объявлены устаревшими и заменены сервисом реализующим `\Drupal\file\FileRepositoryInterface`.

Вам следует заменить их вызовы новыми:

* `file_save_data()` → `\Drupal::service('file.repository')->writeData()`
* `file_move()` → `\Drupal::service('file.repository')->move()`
* `file_copy()` → `\Drupal::service('file.repository')->copy()`

Данные методы принимают аналогичные аргументы, за исключением того что `$destination` теперь является обязательным аргументом. Если `$destination` не задан, будет использовано значение по умолчанию `\Drupal::config('system.file')->get('default_scheme') . '://'`, что обычно приводит к результату `public://`.

В новом сервисе также присутствует новый метод `::loadByUri()` который позволяет найти сущность File по URI.

Новые методы не выводят сообщения при помощи `\Drupal::messenger()`, следовательно, код, вызывающий данные методы должен сам позаботиться о сообщениях, если это необходимо.

Данные методы могут выбросить следующие исключения:

* `\Drupal\Core\File\Exception\FileException`: Выбрасывается при возникновении проблем во время записи в файловую систему.
* `\Drupal\Core\File\Exception\InvalidStreamWrapperException`: Выбрасывается, если у пути назначения невалидная схема.
* `\Drupal\Core\Entity\EntityStorageException`: Выбрасывается при возникновении проблем во время обновления файла в хранилище.

## В административный интерфейс сущностей добавлена новая вкладка «Управление правами доступа»

- [#2934995](https://www.drupal.org/node/2934995)

При редактировании типов материалов, словарей и т.д., администраторы сайта уже имеются следующие вкладки: Редактировать, Управление полями, Управление отображением и т.д.

Начиная с текущего изменения добавлена новая вкладка «Управление правами доступа», которая позволяет настраивать права доступа связанные с текущей сущностью.

### Разработчикам модулей

Новая вкладка для настройки прав доступа появляется автоматически при следующих условиях:

* Сущность имеет бандлы объявленные при помощи конфигурационных сущностей. Например `node` и `node_type`.
* Тип сущности должен иметь значение `bundle_entity_type`, а бандл иметь значение `bundle_of`.
* Тип сущности должен иметь значение `field_ui_base_route`.
* Есть права доступа зависящие от бандлов сущности. Например, право доступа `create article content` зависящее от `article` бандла типа материала.

Во всех остальных случаях вы можете объявить форму добавив новый маршрут. Например, если модуль `filter` желает добавить данную вкладку для единственного права доступа `use text format basic_html`, это может быть сделано следующим образом:

```yaml
entity.filter_format.permission_form:
  path: '/admin/config/content/formats/manage/{bundle}/permissions'
  defaults:
    _title: 'Manage permissions'
    _form: 'Drupal\user\Form\UserPermissionsBundleForm'
    bundle_entity_type: filter_format
  requirements:
    _permission: 'administer permissions'
    _custom_access: '\Drupal\user\Form\UserPermissionsBundleForm::access'
  options:
    parameters:
      bundle:
        type: 'entity:filter_format'
        with_config_overrides: true
```

Для того чтобы пользователь получил доступ к данной форме, у пользователя должно быть право доступа `administer permissions`. Проверка прав доступа разрешит доступ к форме если удовлетворено одно из следующих условий:

* Тип сущности имеет `permission_granularity = "bundle"`.
* Имеются права доступа, зависящие от текущего бандла.

## Olivero — новая стабильная тема Drupal

- [#3243041](https://www.drupal.org/node/3243041) 

Тема [Olivero](../../../../olivero/index.md) помечена как стабильная и будет использоваться как тема по умолчанию начиная с Drupal 10.

## CKEditor 5 — новый экспериментальный модуль

- [#3231364](https://www.drupal.org/node/3231364)

В ядро добавлен новый экспериментальный модуль — CKEditor 5, который добавляет поддержку новой версии редактора.

## Aggregator

- [#3239552](https://www.drupal.org/node/3239552) Внесены улучшения в вызовы `has()` для совместимости с PHP 8.1.
- [#3240148](https://www.drupal.org/node/3240148) Внесены улучшения в `\Drupal\aggregator\Entity\Item::getLink()` для
  совместимости с PHP 8.1.

## Bartik

- [#2725539](https://www.drupal.org/node/2725539) Улучшена контрастность различных состояний при наведении и фокусировке
  элементе.

## Batch System

- [#2875279](https://www.drupal.org/node/2875279) Обновлён код в модулях Drupal-ядра который использовал `batch_set()`.
  Теперь для формирования [пакетной обработки](../../../../9/batches/index.md) ядро использует `BatchBuilder`.

## Block

- [#2268787](https://www.drupal.org/node/2268787) Для форм плагинов блоков в `$form_state` больше не
  передаётся `block_theme` значение, так как оно было крайне ненадёжно и приводило только к проблемам.
- [#2839558](https://www.drupal.org/node/2839558) Блокам добавлена контекстуальная ссылка «Удалить».
- [#3240165](https://www.drupal.org/node/3240165) Внесены улучшения в `\Drupal\block\Plugin\migrate\process\BlockTheme` для совместимости с PHP 8.1.
- [#3056409](https://www.drupal.org/node/3056409) Добавлено отсутствующее свойство `BlockRepository::$contextHandler`.

## Book

- [#2412669](https://www.drupal.org/node/2412669) `BookManager` больше не использует `drupal_static_reset()`, вместо
  этого используйте `\Drupal::service('book.manager')->resetCache();`.

## Bootstrap System

- [#2293257](https://www.drupal.org/node/2293257) Добавлены подсказки типов для переменных в `DrupalKernel`.

## Big Pipe

- [#3238941](https://www.drupal.org/node/3238941) Внесены улучшения
  в `\Drupal\big_pipe\Render\BigPipe::splitHtmlOnPlaceholders()` для совместимости с PHP 8.1.
- [#3204273](https://www.drupal.org/node/3204273) BigPipe больше не использует jQuery.

## CKeditor 5

- [#3249263](https://www.drupal.org/node/3249263) Исправлен тест `ValidatorsTest`.
- [#3249240](https://www.drupal.org/node/3249240) Внесены улучшения в `HTMLRestrictionsUtilities::providedElementsAttributes()` для совместимости с PHP 8.1.
- [#3221082](https://www.drupal.org/node/3221082) Плагины для CKEditor 5 собираются со сборкой ядра (`yarn build`).
- [#3250587](https://www.drupal.org/node/3250587) Исправлена неполадка из-за которой тест `\Drupal\Tests\ckeditor5\FunctionalJavascript\CKEditor5Test::testEditorFileReferenceIntegration()` проваливался на PostgreSQL.
- [#3251034](https://www.drupal.org/node/3251034) Yarn скрипт `watch:ckeditor5` теперь собирает плагины в режиме разработки.

## Claro

- [#3154539](https://www.drupal.org/node/3154539) Добавлена новые градации серого.

## Color

- [#3240176](https://www.drupal.org/node/3240176) Внесены улучшения в `_color_pack()` для совместимости с PHP 8.1.

## Comment

- [#2927874](https://www.drupal.org/node/2927874) Исправлена неполадка, из-за которой предпросмотр комментария
  показывался в неположенном месте.
- [#3240171](https://www.drupal.org/node/3240171) Внесены улучшения в `\Drupal\comment\Entity\Comment::getSubject()` для
  совместимости с PHP 8.1.
- [#3240167](https://www.drupal.org/node/3240167) Внесены улучшения в код, обращающийся
  к `\Drupal\comment\CommentStorage::getMaxThread()` и `\Drupal\comment\Entity\Comment::getThread()` для совместимости с
  PHP 8.1.
- [#3236540](https://www.drupal.org/node/3236540) Улучшена документация для шаблона `comment.html.twig`.
- [#2886615](https://www.drupal.org/node/2886615) Исправлена ошибка в реализации хука `hook_preprocess_comment()` в тестовом модуле. 

## Composer

- [#3224000](https://www.drupal.org/node/3224000) Зависимости ядра обновлены на 27.07.2021.
- [#3225733](https://www.drupal.org/node/3225733) Удалены следующие зависимости ядра: `fabpot/goutte`
  и `behat/mink-goutte-driver`.
- [#3230562](https://www.drupal.org/node/3230562) Зависимости ядра обновлены на 31.08.2021.
- [#3232571](https://www.drupal.org/node/3232571) Зависимости ядра обновлены на 13.09.2021.
- [#3236249](https://www.drupal.org/node/3236249) Зависимости ядра обновлены на 17.09.2021.
- [#3238201](https://www.drupal.org/node/3238201) Зависимости ядра обновлены на 21.09.2021.
- [#3239270](https://www.drupal.org/node/3239270) Зависимости ядра обновлены на 26.09.2021.
- [#3239772](https://www.drupal.org/node/3239772) Зависимости ядра обновлены на 29.09.2021.
- [#3242889](https://www.drupal.org/node/3242889) Зависимости ядра обновлены на 13.10.2021.
- [#3245724](https://www.drupal.org/node/3245724) Зависимости ядра обновлены на 25.10.2021.
- [#3248156](https://www.drupal.org/node/3248156) Зависимости ядра обновлены на 08.11.2021.
- [#3248600](https://www.drupal.org/node/3248600) Зависимости ядра обновлены на 10.11.2021.
- [#3249233](https://www.drupal.org/node/3249233) Компоненты Symfony обновлены до версий 5.4.
- [#3061074](https://www.drupal.org/node/3061074) Пакет `egulias/email-validator` обновлён до версии 2.1. В этой версии
  валидация email-адреса с пробелом будет провалена, до этого он считался валидным.
- [#3251000](https://www.drupal.org/node/3251000) Зависимости ядра обновлены на 24.11.2021.
- [#3238763](https://www.drupal.org/node/3238763) Пакет `egulias/email-validator` обновлён до 3 версии. Поддержка версии 2.1 сохранена до EOL 2 версии в январе 2022.
- [#3251768](https://www.drupal.org/node/3251768) Компоненты Symfony обновлены до версий 5.4.

## Configuration Entity System

- [#3240800](https://www.drupal.org/node/3240800) Создание конфигурационных сущностей через `new ConfigEntityClass()`
  заменено на `ConfigEntityClass::create()` для совместимости с PHP 8.1.

## Configuration System

- [#2926729](https://www.drupal.org/node/2926729) `ConfigManagerInterface::findConfigEntityDependents()`
  и `ConfigManagerInterface::findConfigEntityDependentsAsEntities()`
  теперь `ConfigManagerInterface::findConfigEntityDependencies()`
  и `ConfigManagerInterface::findConfigEntityDependenciesAsEntities()` соответственно.
- [#2870874](https://www.drupal.org/node/2870874) `EntityBase::getTypedData()` теперь корректно возвращает данные для
  конфигурационных сущностей.
- [#3233480](https://www.drupal.org/node/3233480) Исправлена опечатка в название
  класса `InstallerExistingConfigSyncDriectoryProfileMismatchTest`.
- [#3232695](https://www.drupal.org/node/3232695) `Condition` с операторами `IS NULL` и `IS NOT NULL` теперь
  используется только при наличии значения для сравнения.
- [#3240173](https://www.drupal.org/node/3240173) Внесены улучшения
  в `\Drupal\KernelTests\Core\Config\ConfigImporterTest::testIsInstallable()` для совместимости с PHP 8.1.
- [#3240174](https://www.drupal.org/node/3240174) Внесены улучшения в `\Drupal\config_translation\FormElement\Textarea`
  для совместимости с PHP 8.1.
- [#3240455](https://www.drupal.org/node/3240455) Внесены улучшения
  в `\ГDrupal\Tests\content_translation\Functional\ContentTranslationSettingsTest` для совместимости с PHP 8.1.
- [#2852557](https://www.drupal.org/node/2852557) Сортировка ключей в конфигурациях теперь учитывает сортировку ключей в конфигурационной схеме.

## Contact

- [#3240191](https://www.drupal.org/node/3240191) Внесены улучшения в `\Drupal\KernelTests\KernelTestBase::bootKernel()`
  для совместимости с PHP 8.1.

## Content Moderation

- [#3211072](https://www.drupal.org/node/3211072)
  Плагин `\Drupal\content_moderation\Plugin\Derivative\DynamicLocalTasks` теперь требует передавать `Router` в
  конструктор.
- [#3226516](https://www.drupal.org/node/3226516) Удалён дублирующий вызов `::drupalGet()`
  в `ModerationStateNodeTypeTest`.

## Database Logging

- [#3240182](https://www.drupal.org/node/3240182) Внесены улучшения
  в `\Drupal\dblog\Controller\DbLogController::createLink()` для совместимости с PHP 8.1.
- [#2909805](https://www.drupal.org/node/2909805) Исправлена неполадка, из-за которой `LogMessageParser` ломал сообщения
  содержащие фигурные скобки (`{}`).

## Database System

- [#3211780](https://www.drupal.org/node/3211780) `Connection::queryTemporary()` объявлен устаревшим.
- [#3224199](https://www.drupal.org/node/3224199) Свойство `Connection::$temporaryNameIndex` помечено устаревшим.
- [#838992](https://www.drupal.org/node/838992) Поле UID для таблицы пользователей изменено с целого числа на
  последовательное.
- [#3230714](https://www.drupal.org/node/3230714) Тест `ConnectionUnitTest` пропускается если база данных не psql или
  mysql.
- [#3241306](https://www.drupal.org/node/3241306) Внесены улучшения в `ConnectionTest` для совместимости с PHP 8.1.
- [#3124674](https://www.drupal.org/node/3124674) В драйвер SQLite внесены изменения связанные с прекращением поддержки опции подключения `extra_prefix`.
- [#3247414](https://www.drupal.org/node/3247414) Исправлены ошибки в документации для `Connection::statementClass` и `Connection::statementWrapperClass`.

## Datetime

- [#3236798](https://www.drupal.org/node/3236798) В методе `\Drupal\Core\Datetime\DateFormatter::formatInterval()`
  улучшена совместимость с PHP 8.1.
- [#3236796](https://www.drupal.org/node/3236796) Улучшена совместимость с PHP 8.1 в классах `Drupal\Core\Datetime`.
- [#3240181](https://www.drupal.org/node/3240181) Внесены улучшения в `\Drupal\datetime\DateTimeComputed::getValue()`
  для совместимости с PHP 8.1.
- [#3250335](https://www.drupal.org/node/3250335) Исправлена неполадка, из-за которой перестал работать `#date_date_callbacks`.
- [#3250349](https://www.drupal.org/node/3250349) Функции обратного вызова для `#date_time_callbacks` и `$date_date_callbacks` теперь должны реализовывать `TrustedCallbackInterface`.

## Editor

- [#3240183](https://www.drupal.org/node/3240183) Внесены улучшения в `\Drupal\Tests\editor\Functional\EditorAdminTest`
  для совместимости с PHP 8.1.

## Entity Reference

- [#3225947](https://www.drupal.org/node/3225947) Удалён бесполезный файл `entity_reference.install`
  и `simpletest_install()`.

## Entity System

- [#3226487](https://www.drupal.org/node/3226487) Вкладка ревизий для материалов (`node`) теперь всегда показывается
  если у пользователя есть права на их просмотр и материал имеет больше одной ревизии.
- [#3232673](https://www.drupal.org/node/3232673) `\Drupal\Core\Entity\EntityInterface::label()` может возвращать `NULL`
  , в связи с чем, весь код в ядре что использует это значение, теперь будет использовать ID сущности если заголовок
  недоступен.
- [#3241308](https://www.drupal.org/node/3241308) Внесены улучшения в `DefaultTableMappingTest` для совместимости с PHP
  8.1.
- [#3043321](https://www.drupal.org/node/3043321) UI для ревизий нод и медиа сущностей теперь использует общие сервисы проверки прав доступа API.
- [#3246150](https://www.drupal.org/node/3246150) Исправлена неполадка, из-за которой некорректно работал `hook_entity_type_alter()` после введения поддержки классов для бандлов сущности.

## Extension System

- [#3225779](https://www.drupal.org/node/3225779) Функция `system_sort_modules_by_info_name()` объявлена устаревшей. В
  качестве замены используйте `Drupal\Core\Extension\ExtensionList::sortByName()`.
- [#3236446](https://www.drupal.org/node/3236446) Внесены улучшения в `ModilesListConfirmForm` и `ModulesListForm` для
  уменьшения дублирующего кода.

## Field System

- [#3184542](https://www.drupal.org/node/3184542) Максимальная длина для ввода метки поля увеличена со 128 до 255
  символов.
- [#2226811](https://www.drupal.org/node/2226811) Исправлен тайпхинт для параметра `$definition` в `FieldItemBase`.
  Ранее он указывал что тип должен быть `DataDefinitionInterface`, но на самом деле
  ожидал `ComplexDataDefinitionInterface`.
- [#3218711](https://www.drupal.org/node/3218711) Добавлен тест покрывающий максимальный размер загрузки равный '300 0'.
  Данное значение невалидно и должно выдавать ошибку.
- [#2508866](https://www.drupal.org/node/2508866) Исправлена неполадка из-за которой описание поля не показывалось для
  полей с датой.
- [#3238227](https://www.drupal.org/node/3238227) Внесены улучшения
  в `\Drupal\Core\Field\Plugin\Field\FieldType\PasswordItem` для совместимости с PHP 8.1.
- [#3239744](https://www.drupal.org/node/3239744) Внесены улучшения
  в `\Drupal\Core\Field\WidgetBase::getFilteredDescription()` для совместимости с PHP 8.1.
- [#2817081](https://www.drupal.org/node/2817081) Добавлена поддержка редактирования сводки для множественных
  многострочных текстовых полей.

## Field UI

- [#2726881](https://www.drupal.org/node/2726881) Удалена пагинация со страницы `admin/reports/fields`.
- [#3240185](https://www.drupal.org/node/3240185) Внесены улучшения
  в `\Drupal\Tests\field\Functional\FieldDefaultValueCallbackTest` для совместимости с PHP 8.1.
- [#3240186](https://www.drupal.org/node/3240186) Внесены улучшения
  в `\Drupal\field\Plugin\migrate\source\d6\Field::prepareRow()` для совместимости с PHP 8.1.

## File System

- [#3224420](https://www.drupal.org/node/3224420) `throw new FileTransferException()` теперь возвращает `0`
  вместо `NULL`.
- [#2032893](https://www.drupal.org/node/2032893) Функция `_views_file_status()` объявлена устаревшей.
- [#3239831](https://www.drupal.org/node/3239831) Удалён устаревший `@todo`
  из `\Drupal\Core\StreamWrapper\LocalStream::getDirectoryPath()`.
- [#3239761](https://www.drupal.org/node/3239761) Внесены улучшения
  в `\Drupal\Core\StreamWrapper\PrivateStream::basePath()` для совместимости с PHP 8.1.
- [#3240220](https://www.drupal.org/node/3240220) Внесены улучшения в `\Drupal\file\Entity\File::getSize()` для
  совместимости с PHP 8.1.
- [#3245244](https://www.drupal.org/node/3245244) `FileUploadHandler` теперь может быть использован не только для обработки файлов загруженных через форму.

## Filter

- [#3224478](https://www.drupal.org/node/3224478) Ссылка в `/filter/tip` ведущая на <http://www.w3.org/TR/html/>
  изменена на новую — <https://html.spec.whatwg.org/>.
- [#3240228](https://www.drupal.org/node/3240228) Внесены улучшения в `_filter_url_trim()` для совместимости с PHP 8.1.
- [#3240247](https://www.drupal.org/node/3240247) Внесены множественные улучшения в код модуля для совместимости с PHP
  8.1.

## Forms System

- [#3219541](https://www.drupal.org/node/3219541) Удалён избыточный вызов `$this->requestStack->getCurrentRequest()`
  в `FormBuilder::buildForm()`.

## Image

- [#3216106](https://www.drupal.org/node/3216106) Улучшено описание для плагина эффекта изображения `image_convert`.
- [#3240906](https://www.drupal.org/node/3240906) Внесены улучшения в `template_preprocess_image_formatter()` для
  совместимости с PHP 8.1.

## Image System

- [#3239935](https://www.drupal.org/node/3239935) (отменено) Произведён рефакторинг `ToolkitGdTest`.

## Install system

- [#3185768](https://www.drupal.org/node/3185768) Из установщика удалены `InlineServiceDefinitionsPass`
  , `RemoveUnusedDefinitionsPass`, `AnalyzeServiceReferencesPass` и `ReplaceAliasByActualDefinitionPass` для того чтобы
  избежать бесполезную работу. Это позволяет ускорить
  установку ([-19%](https://blackfire.io/profiles/compare/612e435c-5c03-48b3-99a3-80c1846396e1/graph?settings%5Bdimension%5D=wt&settings%5Bdisplay%5D=landscape&settings%5BtabPane%5D=nodes&selected=&callname=main()))
  путём снижения количества вызовов функций.
- [#3251625](https://www.drupal.org/node/3251625) Исправлена неполадка, из-за которой подключение `settings.php` могло происходить повторно и приводить к ошибкам.
- [#2871357](https://www.drupal.org/node/2871357) Исправлена неполадка, из-за которой множественные операции [пакетной обработки](../../../../9/batches/index.md) могли не выполняться.
- [#2925203](https://www.drupal.org/node/2925203) Исправлена неполадка, из-за которой могла произойти потеря данных в процессе завершения переводов сайта.

## JavaScript

- [#3212747](https://www.drupal.org/node/3212747) Удалено присвоение `BABEL_ENV` для скриптов сборки CSS и jQuery UI.
- [#3228351](https://www.drupal.org/node/3228351) В ядро добавлена новая библиотека
  — [loadjs](https://github.com/muicss/loadjs). На данный момент она будет использоваться в `Drupal.ajax` чтобы
  убедиться что библиотеки, запрошенные с AJAX ответом, подключились.
- [#3217355](https://www.drupal.org/node/3217355) Добавлена новая конфигурация `skip_testcases_on_fail: false`
  в `core/tests/Drupal/Nightwatch/nightwatch.conf.js`.
- [#3238863](https://www.drupal.org/node/3238863) Произведён рефакторинг кода, который использует функцию `merger` от
  jQuery.
- [#3239132](https://www.drupal.org/node/3239132) Произведён рефакторинг кода, который использует функцию `trim` от
  jQuery.
- [#3239507](https://www.drupal.org/node/3239507) Добавлен полифил `CustomEvent`.
- [#3246141](https://www.drupal.org/node/3246141) JavaScript зависимости обновлены на 28.10.21.
- [#3225811](https://www.drupal.org/node/3225811) Библиотека `js-cookie` обновлена до версии 3.0.1.
- [#3244855](https://www.drupal.org/node/3244855) Некоторым системным библиотекам добавлена зависимость на `core/jquery.once.bc`.
- [#3239509](https://www.drupal.org/node/3239509) Добавлена новая библиотека `drupal.string.includes` содержащая полифил для `String.includes.`

## JSON:API

- [#3036593](https://www.drupal.org/node/3036593) ID сущности теперь содержится в `meta.drupal_internal__target_id`. Это
  позволяет фильтровать значения по данному свойству и получать внутренний ID, а не только UUID.
- [#3147244](https://www.drupal.org/node/3147244) Сервис `jsonapi.field_resolver` теперь принимает
  аргумент `@current_user`.
- [#3231040](https://www.drupal.org/node/3231040) Исключения JSON:API больше не
  используют `DependencySerializationTrait`.
- [#3236255](https://www.drupal.org/node/3236255) Из тестов, где возможно, удалены вызовы `::rebuildAll()`.


## Language System

- [#3208373](https://www.drupal.org/node/3208373) Улучшено описание для `LanguageNegotiationContentEntity`. Теперь в нём
  говорится что за определение языка материала отвечают [сервисы](../../../../9/services/index.md) с
  метками `language_content_entity`.
- [#3240905](https://www.drupal.org/node/3240905) Внесены улучшения
  в `\Drupal\language\Plugin\LanguageNegotiation\LanguageNegotiationSession` для совместимости с PHP 8.1.
- [#3240911](https://www.drupal.org/node/3240911) Внесены улучшения в `language_test_page_top()` для совместимости с PHP
  8.1.
- [#3247901](https://www.drupal.org/node/3247901) `ContentTranslationUITestBase` теперь использует Stark тему вместо Classy.

## Layout Builder

- [#3035174](https://www.drupal.org/node/3035174) Трейт `SectionStorageTrait` объявлен устаревшим в
  пользу `SectionListTrait`.
- [#3230928](https://www.drupal.org/node/3230928) Удалена зависимость на Quick Edit
  из `LayoutBuilderTest::testRemovingAllSections()`.
- [#3239436](https://www.drupal.org/node/3239436) Внесены улучшения
  в `\Drupal\Tests\layout_builder\FunctionalJavascript\LayoutBuilderDisableInteractionsTest` для большей совместимости с
  различными chromedriver.
- [#3240909](https://www.drupal.org/node/3240909) Внесены улучшения в `DefaultPluginManager`, `SectionStorageManagerTest`, `LayoutPluginManagerTest` и `DefaultPluginManagerTest` для совместимости с PHP 8.1.

## Locale

- [#3240201](https://www.drupal.org/node/3240201) Внесены улучшения
  в `\Drupal\Tests\Component\Gettext\PoStreamWriterTest::setUp()` для совместимости с PHP 8.1.
- [#3240958](https://www.drupal.org/node/3240958) Внесены улучшения
  в `template_preprocess_locale_translation_last_check()` для совместимости с PHP 8.1.

## Media System

- [#3222486](https://www.drupal.org/node/3222486) Метки для удалённых видео (remote video) обновлены таким образом, что
  они теперь более последовательны и менее многословны.
- [#3222282](https://www.drupal.org/node/3222282) Из файла `media_library.module` удалён `@todo` на
  ишью [#2964789](https://www.drupal.org/project/drupal/issues/2964789).
- [#3028664](https://www.drupal.org/node/3028664) Ошибке oEmbed провайдера теперь логируются.
- [#3240955](https://www.drupal.org/node/3240955) Внесены улучшения
  в `\Drupal\media\Plugin\Filter\MediaEmbed::applyPerEmbedMediaOverrides()` для совместимости с PHP 8.1.
- [#3231731](https://www.drupal.org/node/3231731) При получении превью oEmbed содержимого, для определения типа файла
  теперь используется заголовок `Content-Type`.

## Menu System

- [#3212021](https://www.drupal.org/node/3212021) Обновлён код `MenuTreeParameters` для поддержки PHP 8.1.

## Menu UI

- [#3221493](https://www.drupal.org/node/3221493) Добавлены тесты покрывающие порядок меню в форме настроек типов
  материалов (`node`).
- [#3222465](https://www.drupal.org/node/3222465) `MenuUiNodeTypeTest` теперь использует специальную тестовую
  конфигурацию вместо системной.

## Migrate Drupal UI

- [#3240959](https://www.drupal.org/node/3240959) Внесены улучшения
  в `\Drupal\migrate_drupal_ui\Form\ReviewForm::prepareOutput()` для совместимости с PHP 8.1.

## Migration system

- [#3222168](https://www.drupal.org/node/3222168) Везде где в качестве сигнатуры использовался `\GuzzleHttp\Client`
  теперь используется `\GuzzleHttp\ClientInterface`.
- [#3215836](https://www.drupal.org/node/3215836) Добавлена новая константа `MigrateSourceInterface::NOT_COUNTABLE`
  которую необходимо использовать для неисчисляемых источников.
- [#3227549](https://www.drupal.org/node/3227549) `\Drupal\migrate\Plugin\migrate\id_map\Sql::getRowByDestination()`
  теперь всегда возвращает массив.
- [#3239556](https://www.drupal.org/node/3239556) Исправлен тип возвращаемых данных
  для `\Drupal\Tests\migrate\Kernel\TestFilterIterator::accept()`.
- [#3222844](https://www.drupal.org/node/3222844) Добавлена документация о возвращаемом
  значение `MigrateExecutableInterface::import()`.
- [#3241130](https://www.drupal.org/node/3241130) Внесены улучшения в `MakeUniqueEntityFieldTest`
  и `MigrateSqlIdMapEnsureTablesTest` для совместимости с PHP 8.1.
- [#3241275](https://www.drupal.org/node/3241275) Внесены улучшения
  в `\Drupal\user\Plugin\migrate\source\d6\User::prepareRow()` для совместимости с PHP 8.1.
- [#2976098](https://www.drupal.org/node/2976098) Теперь `MigrateExecutable` логирует более детально информацию об
  ошибках связанных с обработчиками строк в `migration` и `destination`.
- [#3095237](https://www.drupal.org/node/3095237) Добавлена поддержка миграции значений `todate` из Drupal 7 поля даты.
- [#3212891](https://www.drupal.org/node/3212891) Исправлены некорректные тайпхинты в комментариях к свойствам некоторых плагинов.
- [#2975461](https://www.drupal.org/node/2975461) Плагин `menu_link` теперь конвертирует query строки в массивы.

## MySQL DB driver

- [#3224245](https://www.drupal.org/node/3224245) Подключение к MySQL теперь открывается с
  использованием `\PDO::ATTR_STRINGIFY_FETCHES`.

## Node system

- [#3220956](https://www.drupal.org/node/3220956) Удалён `@todo` из шаблона `node.html.twig` напоминающий удалить `id`
  аттрибут, который был уже удалён.
- [#3037202](https://www.drupal.org/node/3037202) `node_mark()` больше не использует `drupal_static()`, соответственно,
  вызов `drupal_static()` с аргументом `node_mark` объявлен устаревшим.
- [#3156244](https://www.drupal.org/node/3156244) `SyndicateBlock` теперь задаёт заголовок равный названию сайта.
- [#3241268](https://www.drupal.org/node/3241268) Внесены улучшения в `node_test.module` для совместимости с PHP 8.1.

## Olivero

- [#3200370](https://www.drupal.org/node/3200370) Улучшено оформление `drop-button` элемента, для того чтобы он
  соответствовал новому оформлению форм.
- [#3174107](https://www.drupal.org/node/3174107) Добавлены тесты для темы Olivero.
- [#3223314](https://www.drupal.org/node/3223314) Библиотекам `olivero.libraries.yml` добавлены версии и отсортированы в
  алфавитном порядке.
- [#3226865](https://www.drupal.org/node/3226865) В
  шаблоне `block--secondary-menu--plugin-id--search-form-block.html.twig` `<div>` внутри `<button>` заменён на `<span>`.
- [#3226019](https://www.drupal.org/node/3226019) Протокол URL-адресов в `block--secondary-menu.html.twig` изменено на
  HTTPS.
- [#3216489](https://www.drupal.org/node/3216489) Исправлена неполадка из-за которой предзагрузка шрифта не работала
  если сайту задан `base path`.
- [#3228140](https://www.drupal.org/node/3228140) `aria-label` для кнопки мобильной навигации в `page.html.twig`
  изменено на «Main Menu».
- [#3212975](https://www.drupal.org/node/3212975) Селекторы в `messages.es6.js` заменены с классов
  на `data-drupal-selector`.
- [#3223332](https://www.drupal.org/node/3223332) Основная кнопка поиска теперь инициализируется
  с `aria-expanded="false"`.
- [#3205597](https://www.drupal.org/node/3205597) Форме комментария добавлен заголовок.
- [#3224958](https://www.drupal.org/node/3224958) Раскрытые фильтры Views теперь отображаются в строке, а не в рядах.
- [#3226785](https://www.drupal.org/node/3226785) Поисковая форма теперь закрывается при потери фокуса.
- [#3194560](https://www.drupal.org/node/3194560) Добавлено оформления для страницы «Сайт на технических работах».
- [#3225241](https://www.drupal.org/node/3225241) Исправлена неполадка, из-за которой переставал работать JavaScript
  основной навигации если вложенность достигала более двух уровней.
- [#3214191](https://www.drupal.org/node/3214191) Исправлена неполадка, из-за которой шапка была поверх наложения (
  overlay) от модальных окон jQuery UI.
- [#3231416](https://www.drupal.org/node/3231416) Библиотеки для основного и вторичного меню теперь грузятся только при наличии данных меню.
- [#3244621](https://www.drupal.org/node/3244621) Исправлена JavaScript ошибка, возникающая при появлении secondary вкладок.
- [#3171570](https://www.drupal.org/node/3171570) Удалено захардкоженое использование стиля изображения для материала `wide`. Теперь будет использоваться стиль из форматтера.

## Path

- [#3224592](https://www.drupal.org/node/3224592) `\Drupal\path_alias\AliasManager::cacheClear()` больше не вызывает
  предупреждения об устаревшем коде на PHP 8.1 и не пытается очистить кеш при `NULL` значении.
- [#3241296](https://www.drupal.org/node/3241296) Внесены улучшения в `PathValidatorTest` для совместимости с PHP 8.1.

## Plugin System

- [#1932810](https://www.drupal.org/node/1932810) Плагин-условия `NodeType` упразднён в
  пользу `\Drupal\entity\Plugin\Core\Condition\EntityBundle`, который был перенесён в ядро из модуля ctools.

## PostgreSQL DB driver

- [#3230801](https://www.drupal.org/node/3230801) Драйвер больше не записывает `NULL` в blob поля.
- [#3214921](https://www.drupal.org/node/3214921) Теперь, при использовании PostgreSQL будет выводиться предупреждение, если расширение `pg_trgm` не создано.

## Quick Edit

- [#3231071](https://www.drupal.org/node/3231071) Удалены аннотации `quickedit` из
  форматтеров-полей `TestTextTrimmedFormatter` и `DummyImageFormatter`.
- [#3227161](https://www.drupal.org/node/3227161) Тесты, что не тестируют Quick Edit больше не используют его селекторы.
- [#3252214](https://www.drupal.org/node/3252214) Интеграционные тесты для QuickEdit и CKEditor 5 перенесены в модуль QuickEdit.

## Render System

- [#2794261](https://www.drupal.org/node/2794261) Функция `render()` объявлена устаревшей. В качестве замены используйте
  сервис `renderer`.
- [#3192839](https://www.drupal.org/node/3192839) Некоторые проверки в тестах для `Renderer` теперь
  используют `assert()`.
- [#3239762](https://www.drupal.org/node/3239762) Внесены улучшения
  в `\Drupal\Core\Template\AttributeString::__toString()` для совместимости с PHP 8.1.
- [#3240960](https://www.drupal.org/node/3240960) Внесены улучшения
  в `\Drupal\KernelTests\Core\Render\Element\ActionsTest::getFormId()` для совместимости с PHP 8.1.

## Responsive image

- [#3241300](https://www.drupal.org/node/3241300) Внесены улучшения в `template_preprocess_responsive_image_formatter()`
  для совместимости с PHP 8.1.
- [#3248816](https://www.drupal.org/node/3248816) `ResponsiveImageFieldUiTest` перемещён в директорию с тестами.

## REST

- [#3002352](https://www.drupal.org/node/3002352) `CacheableHttpException` теперь передаёт `$headers` аргумент
  в `HttpException`.

## Routing System

- [#3183036](https://www.drupal.org/node/3183036) Сервисы проверки прав доступа, что не используются ни одним маршрутом,
  больше не инициализируются.
- [#3236789](https://www.drupal.org/node/3236789) Улучшен код в `Drupal\Core\Controller\TitleResolver::getTitle()` для
  совместимости с PHP 8.1.
- [#3233047](https://www.drupal.org/node/3233047) `Drupal\Core\Routing\RequestContext::fromRequest()` теперь
  возвращает `$this`.
- [#3238942](https://www.drupal.org/node/3238942) Внесены улучшения в `\Drupal\Core\Routing\RedirectDestination::get()`
  для совместимости с PHP 8.1.
- [#3239553](https://www.drupal.org/node/3239553) Внесены улучшения
  в `\Symfony\Component\Routing\Route::getRequirement()` для совместимости с PHP 8.1.
- [#3240194](https://www.drupal.org/node/3240194) Внесены улучшения
  в `\Drupal\KernelTests\Core\Routing\RouteProviderTest::testDuplicateRoutePaths()` для совместимости с PHP 8.1.
- [#3110580](https://www.drupal.org/node/3110580)
  Решён `@todo - remove ::processOutbound() when we remove UrlGenerator::fromPath()`.
- [#3184619](https://www.drupal.org/node/3184619) Метод `UrlGenerator::getRoute()` теперь возвращает `NULL` если не получилось получить маршрут.

## Search

- [#3239558](https://www.drupal.org/node/3239558) Счётчик отправлений
  в `\Drupal\search_embedded_form\Form\SearchEmbeddedForm` теперь имеет значение 0 по умолчанию вместо `NULL`.

## Serialization

- [#2997123](https://www.drupal.org/node/2997123) `PrimitiveDataNormalizer` теперь может передавать кеш-метаданные.

## Session

- [#3241267](https://www.drupal.org/node/3241267) Внесены улучшения в `WriteSafeSessionHandlerTest::testOtherMethods()`
  для совместимости с PHP 8.1.

## Shortcut

- [#3241271](https://www.drupal.org/node/3241271) Внесены улучшения в `shortcut_preprocess_page_title()` для
  совместимости с PHP 8.1.

## System

- [#778346](https://www.drupal.org/node/778346) Функция `system_sort_modules_by_info_name()` объявлена устаревшей и
  заменена идентичной `system_sort_by_info_name()`. Это переименование сделано так как старое название не совсем
  подходящее.
- [#3240364](https://www.drupal.org/node/3240364) Внесены улучшения
  в `\Drupal\Tests\system\Functional\Pager\PagerTest::testMultiplePagers()` для совместимости с PHP 8.1.
- [#3241272](https://www.drupal.org/node/3241272) Внесены улучшения в `TestFileTransfer` для совместимости с PHP 8.1.

## SQLite DB driver

- [#3232699](https://www.drupal.org/node/3232699) Драйвер теперь использует `NULL` там где это нужно, вместо кастинга
  его в строку.

## Taxonomy

- [#3039055](https://www.drupal.org/node/3039055) Удалён вызов `drupal_static_reset('taxonomy_term_count_nodes');` так
  как данное значение никем не устанавливается.
- [#3056258](https://www.drupal.org/node/3056258) В форму добавления и редактирования термина таксономии добавлено новое
  действие «Сохранить и перейти к списку» — которое после сохранения материала перенаправит на страницу со всеми
  терминами словаря.
- [#3037157](https://www.drupal.org/node/3037157) Исправлена неполадка из-за которой переводы словаря игнорировались в
  заголовках страниц «entity.taxonomy_vocabulary.overview_form» и «entity.taxonomy_vocabulary.reset_form».
- [#3221149](https://www.drupal.org/node/3221149) Валидатор аргументов
  Views `\Drupal\taxonomy\Plugin\views\argument_validator\Term` объявлен устаревшим. Вместо него
  используйте `\Drupal\views\Plugin\views\argument_validator\Entity`.
- [#2133215](https://www.drupal.org/node/2133215) Улучшена производительность плагина аргументов `IndexTidDepth` и
  плагина фильтра `TaxonomyIndexTidDepth`. Это на 98% сокращает время выполнения запроса!
- [#3229665](https://www.drupal.org/node/3229665) Исправлена неполадка
  в `\Drupal\Tests\taxonomy\Functional\Views\TaxonomyTermFilterDepthTest` которая могла приводить к непредсказуемым
  провалам тестирования.
- [#3229686](https://www.drupal.org/node/3229686) Произведены микро-оптимизации для тестов функциональных и Kernel
  тестов `TaxonomyTermFilterDepthTest`.

## Text

- [#3067116](https://www.drupal.org/node/3067116) Функция `text_summary()` теперь корректно закрывает HTML теги когда
  используется фильтр `filter_html`.

## Theme system

- [#3239859](https://www.drupal.org/node/3239859) Внесены улучшения
  в `\Drupal\Core\Template\Loader\ThemeRegistryLoader::getCacheKey()` для совместимости с PHP 8.1.
- [#3239860](https://www.drupal.org/node/3239860) Внесены улучшения в `\Drupal\Core\Template\TwigExtension::renderVar()`
  для совместимости с PHP 8.1.

## Tour

- [#3240362](https://www.drupal.org/node/3240362) Внесены улучшения в `\Drupal\tour\TipPluginBase::getLocation()` для
  совместимости с PHP 8.1.

## Symfony 6

- [#3209617](https://www.drupal.org/node/3209617) `Symfony\Component\HttpFoundation\RequestStack::getMasterRequest()`
  объявлен устаревшим, необходимо использовать `::getMainRequest()`. Добавлен
  прокси-класс `Drupal\Core\Http\RequestStack`, который теперь возвращается сервисом `request_stack`.
- [#3231668](https://www.drupal.org/node/3231668) Добавлен тайпхинт `Definition`
  для `Drupal\Core\DependencyInjection\ContainerBuilder::register()`.
- [#3231669](https://www.drupal.org/node/3231669) Добавлен тайпхинт `Alias`
  для `Drupal\Core\DependencyInjection\ContainerBuilder::setAlias()`.
- [#3231672](https://www.drupal.org/node/3231672) Добавлен тайпхинт `Definition`
  для `Drupal\Core\DependencyInjection\ContainerBuilder::setDefinition()`.
- [#3231676](https://www.drupal.org/node/3231676) Добавлены тайпхинты
  для `Drupal\Core\TypedData\Validation\RecursiveValidator::inContext()`
  и `Drupal\Core\TypedData\Validation\RecursiveValidator::startContext()`.
- [#3209619](https://www.drupal.org/node/3209619) Передача `NULL` в качестве аргумента для исключения, помечено
  устаревшим. Там где передавался такой аргумент теперь передаётся пустая строка.
- [#3233464](https://www.drupal.org/node/3233464) Добавлен тайпхинт `ExecutionContextInterface` для методов
  переопределяющих `Symfony\Component\Validator\Context\ExecutionContextFactoryInterface::createContext()`.
- [#3233481](https://www.drupal.org/node/3233481) Добавлены тайпхинты для методов
  переопределяющих `Symfony\Component\Validator\Mapping\Factory\MetadataFactoryInterface::getMetadataFor()`
  и `::hasMetadataFor()`.
- [#3233023](https://www.drupal.org/node/3233023) Добавлен тайпхинт `RouteCollection` для методов
  переопределяющих `Symfony\Component\Routing\RouterInterface::getRouteCollection()`.
- [#3231686](https://www.drupal.org/node/3231686) Добавлен тайпхинт `ConstraintViolationBuilderInterface` для
  метода `Drupal\Core\TypedData\Validation\ExecutionContext::buildViolation()`.
- [#3233045](https://www.drupal.org/node/3233045) Добавлен тайпхинт `array` для методов
  переопределяющих `Symfony\Component\Routing\Matcher\RequestMatcherInterface::matchRequest()`.
- [#3232888](https://www.drupal.org/node/3232888) Добавлен тайпхинт `array` для
  метода `Drupal\Core\Routing\UrlMatcher::getAttributes()`.
- [#3232110](https://www.drupal.org/node/3232110) Добавлены тайпхинты для
  методов `Drupal\Component\EventDispatcher\ContainerAwareEventDispatcher::getListeners()`, `::getListenerPriority()`
  и `::hasListeners()`.
- [#3231688](https://www.drupal.org/node/3231688) (откачено) Добавлены тайпхинты для
  методов `Drupal\Core\TypedData\Validation\ExecutionContext::getViolations()`, `::getValidator()`, `::getRoot()`
  и `::getValue()`.
- [#3231689](https://www.drupal.org/node/3231689) Добавлены тайпхинты для
  методов `Drupal\Core\TypedData\Validation\ExecutionContext::getObject()`, `::getMetadata()`, `::getGroup()`
  , `::getClassName()`, `::getPropertyName()` и `::getPropertyPath()`.
- [#3231690](https://www.drupal.org/node/3231690) Добавлены тайпхинты для
  методов `Drupal\Core\TypedData\Validation\TypedDataMetadata::findConstraints()`, `::getConstraints()`
  , `::getTraversalStrategy()` и `::getCascadingStrategy()`.
- [#3231393](https://www.drupal.org/node/3231393)
  Вызовы `Symfony\Component\DependencyInjection\Alias::getDeprecationMessage()`
  и `Symfony\Component\DependencyInjection\Definition::getDeprecationMessage()` заменены на `::getDeprecation()`.
- [#3231390](https://www.drupal.org/node/3231390) Добавлен тайпхинт для
  метода `Drupal\Tests\DrupalTestBrowser::doRequest()`.
- [#3232082](https://www.drupal.org/node/3232082) Добавлен тайпхинт`Response` для методов
  реализующих `Symfony\Component\HttpKernel\HttpKernelInterface::handle()`.
- [#3233466](https://www.drupal.org/node/3233466) Добавлен тайпхинт`ConstraintValidatorInterface` для методов
  реализующих `Symfony\Component\Validator\ConstraintValidatorFactoryInterface::getInstance()`.
- [#3233482](https://www.drupal.org/node/3233482) Добавлены тайпхинты для
  методов `Symfony\Component\Validator\Constraint::getDefaultOption()` и `::getRequiredOptions()`.
- [#3231682](https://www.drupal.org/node/3233466) Добавлен тайпхинт`ConstraintViolationListInterface` для
  методов `Drupal\Core\TypedData\Validation\RecursiveValidator::validate()`, `::validateProperty()`
  и `::validatePropertyValue()`.
- [#3232895](https://www.drupal.org/node/3232895) Добавлен тайпхинт`string` для методов
  переопределяющих `Symfony\Component\Routing\Generator\UrlGeneratorInterface::generate()`.
- [#3233041](https://www.drupal.org/node/3233041) Добавлен тайпхинт`array` для методов
  переопределяющих `Symfony\Component\Routing\Matcher\UrlMatcherInterface::match()`.
- [#3232893](https://www.drupal.org/node/3232893) Добавлен тайпхинт`ArrayIterator` для
  метода `Drupal\Core\Routing\LazyRouteCollection::getIterator()`.
- [#3231683](https://www.drupal.org/node/3231683) Следующие методы класса `Drupal\Core\TypedData\Validation\ExecutionContext` объявлены устаревшими: `::setNode()`, `::setGroup()`, `::setConstraint()`, `::markConstraintAsValidated()`, `::isConstraintValidated()`, `::markGroupAsValidated()`, `::isGroupValidated()`, `::markObjectAsInitialized()`, `::isObjectInitialized()`.
- [#3248801](https://www.drupal.org/node/3248801) Внесены улучшения в `Drupal\Tests\jsonapi\Functional\JsonApiFunctionalTest` чтобы он не проваливался на Symfony 5+.
- [#3248809](https://www.drupal.org/node/3248809) Внесены улучшения в `Drupal\Tests\file\Kernel\FileItemValidationTest` чтобы он не проваливался на Symfony 5+.
- [#3248013](https://www.drupal.org/node/3248013) Внесены улучшения в `Drupal\Tests\views\Unit\Plugin\argument_default\QueryParameterTest` чтобы он не проваливался на Symfony 5+.
- [#3248810](https://www.drupal.org/node/3248810) Внесены улучшения в `Drupal\Tests\jsonapi\Kernel\EventSubscriber\ResourceObjectNormalizerCacherTest` чтобы он не проваливался на Symfony 5+.
- [#3248014](https://www.drupal.org/node/3248014) Внесены изменения улучшения в `OEmbedIframeController` для совместимости с Symfony 6.

## Umami demo

- [#3129666](https://www.drupal.org/node/3129666) В блоке брендирования для названия сайта больше не добавляется
  класс `visually-hidden`, который прятал заголовок даже если его необходимо показывать.
- [#3227513](https://www.drupal.org/node/3227513) QuickEdit удалён
  из [установочного профиля](../../../../9/distributions/index.md) [Umami](../../../../9/distributions/demo-umami/index.md)
  .
- [#3230554](https://www.drupal.org/node/3230554) Установщик демо-содержимого больше не использует
  сервис `path_alias.manager`.
- [#3072374](https://www.drupal.org/node/3072374) Выбор категории для рецепта теперь выпадающий список.

## Update

- [#3039074](https://www.drupal.org/node/3039074) `drupal_static()` больше не используется в
  функциях `_update_manager_unique_identifier()`, `_update_manager_extract_directory()`
  и `_update_manager_cache_directory()`.
- [#3206293](https://www.drupal.org/node/3206293) Добавлен класс `ProjectRelease`, который является обёрткой для Update
  XML файла с релизами.
- [#2715145](https://www.drupal.org/node/2715145) Удалена конфигурация `system.authorize`.
- [#3180382](https://www.drupal.org/node/3180382) `UpdateManagerUpdate.php` больше не использует сервис `renderer` для
  элемента `last_check`.
- [#3239471](https://www.drupal.org/node/3239471) Исправлен неправильный тип `KeyValueFactoryInterface`
  в `UpdateProcessor`.

## User

- [#2819585](https://www.drupal.org/node/2819585) Исправлен дублирующий `switch case` в `core/modules/user/user.js`.
- [#2946](https://www.drupal.org/node/2946) Теперь, при попытке авторизоваться с отключенными Cookies, будет показано
  соответствующее сообщение, что авторизация невозможна.
- [#3221258](https://www.drupal.org/node/3221258) Роль редактора присваивает только те права доступа, что доступны на
  момент установки.
- [#3240192](https://www.drupal.org/node/3240192) Внесены улучшения в `\Drupal\user\AccountForm::buildEntity()` для
  совместимости с PHP 8.1.
- [#240361](https://www.drupal.org/node/240361) Внесены улучшения в `\Drupal\user\Entity\User::checkExistingPassword()`
  для совместимости с PHP 8.1.
- [#3240180](https://www.drupal.org/node/3240180) Внесены улучшения в код,
  вызывающий `\Drupal\user\Entity\User::getEmail()`, для совместимости с PHP 8.1.
- [#3241265](https://www.drupal.org/node/3241265) Внесены улучшения в `user_user_view()` для совместимости с PHP 8.1.
- [#3199972](https://www.drupal.org/node/3199972) Внесены улучшения в опции блокировки и удалении пользователя.

## Views

- [#2511892](https://www.drupal.org/node/2511892) Исправлена неполадка, приводящая к
  исключению `MissingMandatoryParametersException` при использовании вкладки меню и `%` в пути представления.
- [#2681947](https://www.drupal.org/node/2681947) Представления типа «Блок» теперь поддерживают настройку «Put the
  exposed form in a block».
- [#1551534](https://www.drupal.org/node/1551534) Views AJAX теперь поддерживают элемент `<button>` в качестве кнопки
  отправки, который может появиться в случае переопределения стандартного `<input type="submit">`.
- [#2560447](https://www.drupal.org/node/2560447) `views_form_callback` больше не поддерживается.
- [#3239313](https://www.drupal.org/node/3239313) Внесены улучшения в `\Drupal\views\Controller\ViewAjaxController` для
  совместимости с PHP 8.1.
- [#3241280](https://www.drupal.org/node/3241280) Внесены улучшения в `PathPluginBase`, `NumericField`, `HandlerBase`
  и `QueryGroupByTest` для совместимости с PHP 8.1.
- [#3008138](https://www.drupal.org/node/3008138) Кастомные ссылки теперь являются переводимыми.
- [#3248649](https://www.drupal.org/node/3248649) Внесены улучшения
  в `Drupal\views\Plugin\views\display\PathPluginBase::alterRoutes()` для увеличения производительности.
- [#3250482](https://www.drupal.org/node/3250482) Исправлена документация для `\Drupal\views\Plugin\views\cache\CachePluginBase::cacheSetMaxAge()`.

## Workspaces

- [#3112783](https://www.drupal.org/node/3112783) Добавлены страницы для отображения изменений и их количества внесённых
  в рабочей области.

## Тестирование

- [#3091870](https://www.drupal.org/node/3091870) Ошибки JavaScript выброшенные в `FunctionalJavascript` тестах теперь
  отлавливаются. Начиная с Drupal 10 они будут проваливать тесты.
- [#2758357](https://www.drupal.org/node/2758357) Добавлена документация о том, что `core/phpunit.xml.dist` должен быть
  скопирован в `core/phpunit.xml` для последующей модификации.
- [#3131900](https://www.drupal.org/node/3131900) Исправлены сравнения чьи результаты записываются в переменную.
- [#3196470](https://www.drupal.org/node/3196470) Доработан пустой тест `KernelTestBaseTest::testOutboundHttpRequest()`.
- [#3226106](https://www.drupal.org/node/3226106)
  Из `Drupal\Tests\node\Kernel\Migrate\d7\MigrateNodeTypeTest::assertEntity()` удалён `@dataProvider`.
- [#3139409](https://www.drupal.org/node/3139409) Использование устаревшего `AssertLegacyTrait::assertRaw()` заменено на
  современные подходы.
- [#3227501](https://www.drupal.org/node/3227501) Удалены оставшиеся вызовы `t()` в тестах.
- [#3233010](https://www.drupal.org/node/3233010) Внесены изменения
  в `drupal_phpunit_contrib_extension_directory_roots()` для совместимости с PHP 8.1.
- [#3231781](https://www.drupal.org/node/3231781) Удалены оставшиеся вызовы `t()` в тестах.
- [#3138078](https://www.drupal.org/node/3138078) Переопределения методов `::assert*()` теперь должны иметь тайпхинты, включая `void` для возвращаемого типа.
- [#3250629](https://www.drupal.org/node/3250629) Использование `MockBuilder::setMethods()` заменено на `MockBuilder::onlyMethods()`.
- [#3032275](https://www.drupal.org/node/3032275) Для JavaScript тестов добавлены новые методы, которые лучше обрабатывают клики по ссылкам и ввод данных.

## Прочие изменения

- [#3218968](https://www.drupal.org/node/3218968) Drupal теперь поддерживает `NULL`-сервисы. Например: `Acme\Foo: ~`.
- [#2902540](https://www.drupal.org/node/2902540) Исправлены ошибки стандарта
  кодирования `Drupal.NamingConventions.ValidGlobal`.
- [#1306624](https://www.drupal.org/node/1306624) Файл `router_installer_test.install`
  переименован `router_installer_test.module`.
- [#1884836](https://www.drupal.org/node/1884836) В `DiffEngine` вызовы `md5()` заменены на `crc32b()`.
- [#2725435](https://www.drupal.org/node/2725435) Удалён устаревший `@todo` ведущий
  на [#2364011](https://www.drupal.org/node/2364011).
- [#2830352](https://www.drupal.org/node/2830352) Обновлены ссылки ведущие на документацию Drupal 7.
- [#3127716](https://www.drupal.org/node/3127716) Исправлена опечатка в документации `PathValidator`.
- [#3228396](https://www.drupal.org/node/3228396) Актуализирована ссылка на ChromeDriver.
- [#3227386](https://www.drupal.org/node/3227386) Упрощен тест `BaseThemeMissingTest`.
- [#2639382](https://www.drupal.org/node/2639382) Исправлена неполадка из-за которой было невозможно перевести строки
  для некоторых относительных дат.
- [#3233015](https://www.drupal.org/node/3233015) Произведён рефакторинг `\Drupal\Component\Utility\Random::image()`
  чтобы не было уведомлений об устаревшем коде на PHP 8.1.
- [#3212498](https://www.drupal.org/node/3212498) Исправлены некорректные `</br>`.
- [#3224523](https://www.drupal.org/node/3224523) Для части методов добавлен аттрибут `#[ReturnTypeWillChange]`.
- [#3232691](https://www.drupal.org/node/3232691) Произведён рефакторинг `\Drupal\Core\Ajax\AjaxHelperTrait` для
  совместимости с PHP 8.1.
- [#3232687](https://www.drupal.org/node/3232687) Произведён
  рефакторинг `\Drupal\Core\Config\Entity\ConfigEntityStorage::save()` для совместимости с PHP 8.1.
- [#3233012](https://www.drupal.org/node/3233012) Произведён рефакторинг `\Drupal\Core\Render\Element\HtmlTag` для
  совместимости с PHP 8.1.
- [#3224421](https://www.drupal.org/node/3224421) Добавлена прослойка для Guzzle 6 для работы на PHP 8.1.
- [#3238210](https://www.drupal.org/node/3238210) Метод-двойник для `Drupal\Tests\Core\Routing\LazyRouteCollectionTest`
  теперь возвращает `ArrayIterator`.
- [#3236284](https://www.drupal.org/node/3236284) В качестве значения по умолчанию при обращении к значению заголовка
  запроса теперь используется строка.
- [#3238452](https://www.drupal.org/node/3238452) Исключения теперь передают пустую строку по умолчанию вместо `NULL`
  для совместимости с PHP 8.1.
- [#3236769](https://www.drupal.org/node/3236769) Произведён рефакторинг `\Drupal\Component\Gettext\PoItem` для
  совместимости с PHP 8.1.
- [#3238457](https://www.drupal.org/node/3238457) Внесены улучшения
  в `\Drupal\Core\EventSubscriber\ActiveLinkResponseFilter::setLinkActiveClass()` для совместимости с PHP 8.1.
- [#3239285](https://www.drupal.org/node/3239285) Внесены улучшения в `SelectLanguageForm` для совместимости с PHP 8.1.
- [#3239294](https://www.drupal.org/node/3239294) Улучшены вызовы `preg_split()` для совместимости с PHP 8.1.
- [#3239295](https://www.drupal.org/node/3239295) Улучшены вызовы `str_replace()` и `preg_replace()` для совместимости с
  PHP 8.1.
- [#3238936](https://www.drupal.org/node/3238936) Исправлена неполадка, из-за которой testbot не запускать ESLint на все
  файлы при изменении `core/.eslintrc*`.
- [#3239442](https://www.drupal.org/node/3239442) Убраны вызовы статичных методов от трейтов для совместимости с PHP
  8.1.
- [#3239292](https://www.drupal.org/node/3239292) Внесены улучшения в кернел тесты, которые не
  вызывали `::installConfig()` для совместимости с PHP 8.1.
- [#3239746](https://www.drupal.org/node/3239746) Внесены улучшения в `\Drupal\Core\Flood\MemoryBackend` для
  совместимости с PHP 8.1.
- [#3239758](https://www.drupal.org/node/3239758) Внесены исправления в
  тест `\Drupal\Tests\field\Functional\ReEnableModuleFieldTest` для совместимости с PHP 8.1.
- [#3239710](https://www.drupal.org/node/3239710) Внесены улучшения
  в `\Drupal\Core\Menu\StaticMenuLinkOverrides::loadOverride()` для совместимости с PHP 8.1.
- [#3240456](https://www.drupal.org/node/3240456) `E_DEPRECATED` добавлен в список на пропуск во время выполнения тестов
  для совместимости с PHP 8.1.
- [#3240888](https://www.drupal.org/node/3240888) Создание моков которые реализуют `Serializable` заменены
  на `__serialize()` для совместимости с PHP 8.1.
- [#3240915](https://www.drupal.org/node/3240915) Внесены улучшения в `\Drupal\Component\Utility\Unicode::truncate()`
  для совместимости с PHP 8.1.
- [#3209934](https://www.drupal.org/node/3209934) Исправлены опечатки в 46 словах.
- [#2909370](https://www.drupal.org/node/2909370) Внесены исправления связанные со стандартами `Drupal.Commenting.VariableComment.IncorrectVarType`.
- [#3244533](https://www.drupal.org/node/3244533) Внесены улучшения в вызовы `usleep()` для совместимости с PHP 8.1.
- [#3161223](https://www.drupal.org/node/3161223) Для сортировки значений, там где возможно теперь используется spaceship оператор (`<=>`).
- [#3244592](https://www.drupal.org/node/3244592) Внесены улучшения в `run-tests.sh` для совместимости с PHP 8.1.
- [#3226052](https://www.drupal.org/node/3226052) CSpell обновлён с 4 до 5 версии.
- [#3028837](https://www.drupal.org/node/3028837) Из `views.api.php` заменены некорректные документационные комментарии.
- [#3222251](https://www.drupal.org/node/3222251) Проверки формата `isset($foo) ? $foo : $bar` заменены на оператор объединения с `NULL` (`??`).
- [#3222769](https://www.drupal.org/node/3222769) Использование `list()` заменено на деструктурирующее присваивание (`[$foo, $bar] = $array`).
- [#2707163](https://www.drupal.org/node/2707163) В файл `USAGE.txt` добавлены ссылки описывающие API для расширения и изменения Drupal, вместо старого описания про хуки.
- [#3207567](https://www.drupal.org/node/3207567) Внесены исправления в код связанные с ошибками `Drupal.Commenting.FunctionComment.MissingParamComment`.
- [#3250743](https://www.drupal.org/node/3250743) Внесены улучшения в `NumberFieldTest` для совместимости с PHP 8.1.
- [#3214924](https://www.drupal.org/node/3214924) Рекомендуемая версия PHP (`DRUPAL_RECOMMENDED_PHP`) установлена как 8.0.

## Ссылки

- [Drupal 9.3.0](https://www.drupal.org/project/drupal/releases/9.3.0) (англ.), drupal.org, 8 декабря 2021
- [Bundle classes — киллер фича Drupal 9.3](http://xandeadx.ru/blog/drupal/1002) xandeadx.ru, 2021
