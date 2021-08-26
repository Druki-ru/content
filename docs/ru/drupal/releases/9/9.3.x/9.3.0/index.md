---
title: 'Drupal 9.3.0'
slug: drupal/releases/9.3.0
core: 9
metatags:
  title: 'Drupal 9.3.0: Список изменений' 
  description: 'Список изменений Drupal 9.3.0.'
---

> [!WARNING]
> Данная версия находится в разработке. Список изменений может меняться и дополняться, даты могут смещаться.

**Дата релиза**: декабрь 2021\
**Активная поддержка до**: июнь 2022\
**Поддержка безопасности до**: декабрь 2022

## Маршруты ревизий нод теперь конвертируют параметры {node} и {node_revision} в сущности

* [#2730631](https://www.drupal.org/node/2730631)

Маршруты ревизий нод, такие, как `/node/123/revisions/456/view`, теперь возвращают объекты сущности Node при обращении к параметрам `\Drupal::routeMatch()->getParameter('node')` и `\Drupal::routeMatch()->getParameter('node_revision')`.

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

* [#3160307](https://www.drupal.org/node/3160307)

Сигнатура метода `ThemeManagerInterface::getActiveTheme()` была обновлена следующим образом:

```php
public function getActiveTheme(RouteMatchInterface $route_match = NULL) {
```

Отсутствие данного параметра было недосмотром при введении данного интерфейса в ядро. Все реализации данного интерфейса ядром ожидают данный аргумент.

## Представлен новый метод Connection::lastInsertId(), опция запроса 'return' и константы Database::RETURN_* были помечены устаревшими

* [#3177660](https://www.drupal.org/node/3177660)

Опция запроса `return` и константы `Database::RETURN_*` помечены устаревшими. В [Drupal 10](../../../../10/index.md) они будут удалены, а `Connection::query()` всегда будет возвращать объект `StatementInterface`.

В целом, `Connection::query()` не должен использоваться для операций манипулирования данных (`INSERT`, `DELETE`, `UPSERT`, `MERGE`, `TRUNCATE`). Для данных целей Drupal предоставляет API для построения динамических запросов, который позволяет абстрагироваться от базы данных.

Для крайних случаев, когда `INSERT` требуется использовать при помощи `Connection:query()`, добавлен новый метод `Connection::lastInsertId()` который возвращает ID последнего значения.

Для операций `UPDATE` и других, начиная с текущего изменения не следует использовать `Database::RETURN_AFFECTED`. Вместо этого используйте подсчёт строк, передав соответствующий аргумент в конструктор.

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

* [#3202818](https://www.drupal.org/node/3202818)

Начиная с Drupal 9.3.0, Drupal ядро больше не гарантирует поддержку на Opera Mini.

> [!NOTE]
> Не перепутайте Opera Mini с Opera Mobile — это два разных браузера. Второй будет поддерживаться корректно, так как он основан на Chromium.

## Специфичные для Drupal теги <link> были удалены со страниц нод и терминов таксономии.

* [#2922570](https://www.drupal.org/node/2922570)

В предыдущих версиях Drupal, на страницах материалов сущности `node` и `taxonomy_term` добавлялись `<link>` теги в шапку страницы:

```html
<link rel="delete-form" href="https://example.com/node/1/delete" />
<link rel="delete-multiple-form" href="https://example.com/admin/content/node/delete?node=1" />
<link rel="edit-form" href="https://example.com/node/1/edit" />
<link rel="version-history" href="https://example.com/node/1/revisions" />
<link rel="revision" href="https://example.com/node/1" />
```

Вывод данных ссылок — ресурсоёмкая и затратная операция, которая негативно сказывается на кешировании. Данные ссылки специфичны для Drupal и не имеют известных применений в реальных проектах, поэтому они были удалены.

`canonical` и `shortlink` по-прежнему будут добавляться без изменений, дополнительно, данные `<link>` теги будут теперь добавляться на страницы всех типов сущностей, а не только `node` и `taxonomy_term`.

Сайты, которым требуется старое поведение, могут вернуть ссылки добавив следующий код в `hook_entity_view()` или `hook_page_attachments()`:

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

Вы можете посмотреть на удалённые реализации в `taxonomy_page_attachments_alter()` и ` content_translation_page_attachments()`.

Авторы модулей, которые добавляли для своих сущностей `canonical` и `shortlink` теги, могут удалить их.

## Модули и темы теперь могут указывать уровень стабильности

* [#3124762](https://www.drupal.org/node/3124762), [#3225812](https://www.drupal.org/node/3225812)

Информационные файлы модулей и тем теперь поддерживают два новых параметра `lifecycle` и `lifecycle_link`.

Поддерживаемые значения для `lifecycle`:

* **experimental**: Что-то новое, не завершённое. Будет показано предупреждение при попытке включить подобное расширение, но оно будет работать.
* **stable**: (по умолчанию) Стабильное расширение, никаких дополнительных предупреждений показано не будет.
* **deprecated**: Что-то на пути к выводу из применения. Пользователь по-прежнему сможет устанавливать подобное расширение, но будут показываться предупреждения.
* **obsolete**: Поддержка прекращена. Пользователь должен удалить данное расширение. Ранее установленные расширения продолжат работать, но будет показано предупреждение. Установить данное расширение будет невозможно.

Для значений **deprecated** и **obsolete** состояний требуется указать значение для `lifecycle_link`. Данное значение должно быть URL на документ, где содержится информация для пользователя, которая может помочь сориентироваться что делать дальше.

Пример использования:

```yaml
name: Some core module
type: module
description: '...'
package: Core
version: VERSION
core: 9.x.x
hidden: true
lifecycle: [experimental|stable|deprecated|obsolete] 
lifecycle_link: 'https://www.drupal.org/about/core/policies/core-change-policies/deprecated-and-obsolete-modules-and-themes#s-entity-reference'
```

В связи с изменением, добавлено новое исключение `\Drupal\Core\Extension\Exception\ObsoleteExtensionException`, которое выбрасывается при попытке установить **obsolete** расширение.

Добавлен класс с константами `\Drupal\Core\Extension\ExtensionStatus` в которых перечислены допустимые статусы.

## Новый класс UpdateHookRegistry заменяет функции schema.inc

* [#2124069](https://www.drupal.org/node/2124069)

Константа `SCHEMA_UNINSTALLED` помечена устаревшей и была заменена `Drupal\Core\Update\UpdateHookRegistry::SCHEMA_UNINSTALLED`.

Следующие функции были помечены устаревшими и заменены соответствующими методами сервиса `update.update_hook_registry`:

* `drupal_get_schema_versions()` заменена `\Drupal::service('update.update_hook_registry')->getAvailableUpdates()`
* `drupal_get_installed_schema_version()` заменена `\Drupal::service('update.update_hook_registry')->getInstalledVersion()`
* `drupal_set_installed_schema_version()` заменена `\Drupal::service('update.update_hook_registry')->setInstalledVersion()`

## Стандартный профиль теперь предоставляет новую роль «Content Editor»

* [#3059984](https://www.drupal.org/node/3059984)

При установке [стандартного профиля](../../../../9/distributions/standard/index.md) теперь добавляется новая роль — «Content Editor».

## rel="shortcut icon" теперь rel="icon"

* [#3195222](https://www.drupal.org/node/3195222)

Исторически, иконка сайта (favicon) внедрялась в HTML следующим образом:

```html
<link rel="shortcut icon" href="favicon.ico">
```

Данная разметка больше не соответствует HTML спецификации поэтому начиная с Drupal 9.3.0 разметка будет следующей:

```html
<link rel="icon" href="favicon.ico">
```

## Создание меню для страниц представлений больше не требует включения модуля menu_ui

* [#3021804](https://www.drupal.org/node/3021804)

Следующие плагины представлений больше не требуют активного модуля `menu_ui`:

* `\Drupal\node\Plugin\views\wizard\Node`
* `\Drupal\views\Plugin\views\display\Page`
* `\Drupal\views\Plugin\views\wizard\WizardPluginBase`

Конструктор данных классов теперь требует опциональный аргумент с объектом, реализующий `\Drupal\Core\Menu\MenuParentFormSelectorInterface` (сервис `menu.parent_form_selector`).

## Идентификатор раскрытой сортировки Views теперь может быть настроен

* [#2897638](https://www.drupal.org/node/2897638)

До текущего релиза, только идентификаторы раскрытых фильтров Views могли быть настроены. С текущего релиза также можно указать идентификаторы для раскрытых сортировок. Это позволяет указать более понятное значение в качестве идентификатора для `sort_by`.

В связи с данным изменением, метод `\Drupal\views\Plugin\views\display\DisplayPluginBase::isIdentifierUnique()` теперь требует третий параметр `string $handler_type`.

## LayoutBuilderContextTrait::getAvailableContexts() помечен устаревшим в пользу LayoutBuilderContextTrait::getPopulatedContexts()

* [#3099968](https://www.drupal.org/node/3099968)

`\Drupal\layout_builder\Context\LayoutBuilderContextTrait::getAvailableContexts()` помечен устаревшим и будет удалён в [Drupal 10](../../../../../drupal/10/index.md). Вместо этого метода используйте новый метод `\Drupal\layout_builder\Context\LayoutBuilderContextTrait::getPopulatedContexts()`.

Это сделано потому что метод `::getAvailableContexts()` был предназначен, для того чтобы предоставить список доступных контекстов, но не предоставлял соответствующие значения для данных контекстов, так как некоторые значения вычисляются в рантайме. Это изменение позволяет Layout Builder использовать контексты, которые регистрируются в рантайме.

## #date_time_callback и #date_date_callbacks должны реализовывать TrustedCallbackInterface

* [#3217966](https://www.drupal.org/node/3217966)

В [Drupal 8.8.0](../../../8/8.8.x/8.8.0/index.md) функции обратного вызова `#access_callback`, `#lazy_builder`, `#pre_render` и `#post_render` стали требовать реализации `TrustedCallbackInterface`. Начиная с текущей версии, это требование распространяется на функции обратного вызова для `#date_time_callback` и `#date_date_callbacks`.

## Добавлены правила Eslint для ограничения использования jQuery в новом коде для дальнейшей совместимости

* [#3191023](https://www.drupal.org/node/3191023)

Для того чтобы ограничить использование jQuery в Drupal-ядре и улучшить совместимость с будущими версия, был добавлен плагин [eslint-plugin-jquery](https://github.com/dgraham/eslint-plugin-jquery) в конфигурацию `eslint core-jspassing`.

В настоящее время включено только небольшое количество правил в `eslint-plugin-jquery`. В основном это правила проверяющие на использование возможностей jQuery которые не используются ядром. По мере того как ядро исключает использование той или иной функции jQuery, в связи с тем что современный ES6 JavaScript имеет аналогичные нативные возможности, дополнительные правила могут быть включены. Это предотвратит повторное использование этих функций jQuery.

## Сервисам стало доступно автомонтирование

* [#3021803](https://www.drupal.org/node/3021803) 

Drupal использует контейнер сервисов Symfony, однако некоторые функции, такие как автомонтирование и автоконфигурация, не были задействованы в Drupal 8.

Сервисы могут быть автомонтированы, и ядро Drupal теперь включает тесты для этой функциональности. Это означает, что при определении [сервиса](../../../../9/services/index.md) в `*.services.yml`, во многих случаях аргументы могут быть автоматически выведены из подсказок типов аргументов конструктора. Чтобы активировать функцию автомонтирования, при определении сервиса необходимо указать `autowire: true`. (Включение autowiring по умолчанию для всех сервисов в файле пока не поддерживается).

Сервис будет создан точно так же, как и раньше, но нет необходимости явно указывать, какие сервисы требуются; интерфейсы, объявленные в конструкторе сервиса, будут использоваться для определения того, какие сервисы должны быть внедрены.

Хотя конструктор контейнеров/парсер файлов сервисов Yaml в Drupal теперь поддерживает автомонтирование, в ядре этот паттерн пока не реализован. Поскольку текущее соглашение Drupal об именовании сервисов использует строки, а не имена классов для идентификаторов сервисов, разработчики сайтов не сразу смогут использовать автомонтирование для сервисов из ядра. Для добавления уровня обратной совместимости псевдонимов сервисов существующих интерфейсов ядра существует [последующий issue](https://www.drupal.org/project/drupal/issues/3049525).

Для случаев, когда несколько сервисов реализуют один и тот же интерфейс, например кеш-бэкенды, мы не можем добавить синоним, так как мы не знаем какой из сервисов необходимо подключать. Для таких случаев добавлена возможность указать необходимый сервис, например:

```php
token:
  class: Drupal\Core\Utility\Token
  autowire: true
  arguments:
    $cache: '@cache.default'
```

В примере выше, аргумент `$cache` имеет тип `\Drupal\Core\Cache\CacheBackendInterface`, но у нас множество реализаций данного интерфейса и они имеют разные сервисы. В примере выше мы явно указали что данный параметр ожидает сервис `@cache.default` в качестве аргумента, а все остальные параметры будут автомонтированы, как и прежде.

Для более детальной информации об автомонтировании сервисов [изучите документацию Symfony](https://symfony.com/doc/current/service_container/autowiring.html).

## Модуль Update больше не зависит от модуля File

* [#3014051](https://www.drupal.org/node/3014051)

Модуль Update позволяет устанавливать модули и темы при помощи URL-адресов и загрузки файлов. Ранее, модуль File устанавливался как зависимость, хотя модуль Update может работать без него (загрузка на основе URL-адреса).

Теперь модуль File не является зависимостью для модуля Update, но всё также требуется если вы хотите включить установку модулей и тем при помощи загрузки архива.

## Функции menu_list_system_menus() и menu_ui_get_menus() помечены устаревшими

* [#1882552](https://www.drupal.org/node/1882552)

Функции `menu_list_system_menus()` и `menu_ui_get_menus()` помечены устаревшими. Вместо них используйте систему сущностей.

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

## Несколько процедурных функций из модуля Taxonomy устарели в пользу прямого использования Entity API

* [#3039039](https://www.drupal.org/node/3039039)

Некоторые функции `taxonomy.module` устарели. Также, использование `drupal_static_reset()` c значением `taxonomy_vocabulary_get_names` в качестве параметра устарело.

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

## Разрешения теперь могут объявлять зависимости 

* [#2571235](https://www.drupal.org/node/2571235)

Разрешения могут генерироваться динамически с помощью функций обратного вызова. Например, модуль Node генерирует разрешения на основе доступных типов содержимого. Если условия, вызывающие генерацию таких разрешений, изменяются, и они больше не существуют, то разрешение будет удалено из любой роли, которой оно было назначено.

Для поддержки удаления таких разрешений в массив информации о разрешении был добавлен новый ключ `dependencies`. Текущая структура этого массива такова:

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

### Добавление разрешений при помощи BundlePermissionHandlerTrait

Для того чтобы помочь вам добавлять подобные разрешения добавлен новый трейт `Drupal\Core\Entity\BundlePermissionHandlerTrait` с методом `::generatePermissions()`.

Например, `Drupal\node\NodePermissions` уже имеет метод `::buildPermissions()`, который возвращает массив массивов. Ключами внешнего массива являются машинные имена разрешений, а внутренние массивы имеют ключи `title` (обязательно) и `description` (необязательно).

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

## Функции file_create_url() и file_url_transform_relative() помечены устаревшими, добавлен новый сервис file_url_generator

* [#2669074](https://www.drupal.org/node/2669074)

Функции `file_crete_url()` и `file_url_transform_relative()` помечены устаревшими.

В качестве замены представлен новый сервис `file_url_generator`.

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

Существующие сервисы `asset.css.optimizer`, `asset.js.collection_renderer` и `asset.css.collection_renderer` теперь принимают сервис `file_url_generator` в качестве аргумента.

## Библиотека core/jquery.once помечена устаревшей

* [#3207782](https://www.drupal.org/node/3207782)

**Изменения API:**

* Добавлена новая библиотека `core/once` использующая [Drupal Once JavaScript API](../../../../../javascript/drupal/once/index.md).
* Библиотека `core/jquery.once` помечена устаревшей.
* Добавлена новая глобальная переменная `once` в конфигурацию eslint.
* Новая once библиотека предоставляет 4 функции:
  * `once` = `$.once`
  * `once.filter()` = `$.findOnce()`
  * `once.remove()` = `$.removeOnce()`
  * `once.find()` новая функция не имеющая аналога в jQuery.

### Обратная совместимость

Если текущий код использует `core/jquery.once` из ядра, добавлен слой обратной совместимости который автоматически при использовании устаревшей библиотеки. Вызовы `jQuery.once` будут учитывать предыдущие вызовы `once()`.

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
    js/myfeature.js: {}
  dependencies:
    - core/drupal
    - core/jquery
    - core/jquery.once
```

```javascript
# js/myfeature.js
(function ($, Drupal) {
  Drupal.behaviors.myfeature = {
    attach(context) {
      const $elements = $(context).find('[data-myfeature]').once('myfeature');
      // `$elements` is always a jQuery object.
      $elements.each(processingCallback);
    }
  };

  function processingCallback(index, value) {}
}(jQuery, Drupal));
```

**После (удаляем jQuery зависимость):**

```yaml
# mymodule.libraries.yml
myfeature:
  js: 
    js/myfeature.js: {}
  dependencies:
    - core/drupal
    - core/once
```

```javascript
# js/myfeature.js
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
  function processingCallback(value, index) {}
}(Drupal, once));
```

## drupal_get_path() и drupal_get_filename() помечены устаревшими в пользу сервисов расширений

* [#2347783](https://www.drupal.org/node/2347783)

Функции `drupal_get_path()` и `drupal_get_filename()` [помечены устаревшими](../../../../../deprecation/index.md). Вместо данных функций используйте, где это возможно, специальные методы `\Drupal\Core\Extension\ExtensionList::getPath()` и `\Drupal\Core\Extension\ExtensionList::getPathname()`. Вы можете использовать `extension.path.resolver:getPath()` и `extension.path.resolver:getPathname()` там где поддерживается [Dependency Injection](../../../../9/services/dependency-injection/index.md).

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
  * `drupal_get_filename('module', 'node')` → `\Drupal::service('extension.path.resolver')->getPathname('module', 'node')`
  * `drupal_get_filename('theme', 'seven')` → `\Drupal::service('extension.list.theme')->getPathname('seven')`
  * `drupal_get_filename('profile', 'standard')` → `\Drupal::service('extension.list.profile')->getPathname('standard')`

## Передача компилятора GuzzleMiddlewarePass была удалена

* [#3215397](https://www.drupal.org/node/3215397)

`GuzzleMiddlewarePass` отвечал за сбор сервисов с метками `http_client_middleware` и передавал их сервису с `Guzzle` (`http_client`).

`TaggedHandlersPass` выполняет ту же работу в общем виде - он работает для любого сервиса с меткой - поэтому теперь этот обработчик используется вместо него, а `GuzzleMiddlewarePass` был удален.

Если вы использовали или расширяли `GuzzleMiddlewarePass`, вам следует перейти на `TaggedHandlersPass`.

## Функция file_build_uri() помечена устаревшей

* [#3223016](https://www.drupal.org/node/3223016)

Функция `file_build_uri()` была помечена устаревшей. Прямой замены данной функции не предоставлено.

Данная функция делает две вещи: объединяет схему по умолчанию (как правило `public://`) с предоставленным путём, а затем нормализует его.

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

Однако нормализация пути не нужна, если это жестко заданная папка модуля, например `file_build_uri('my_module')` можно заменить лишь одной строкой `\Drupal::config('system.file')->get('default_scheme') . '://my_module'`.

Для настраиваемых путей рекомендуется включать обертку потока явно, либо как отдельный параметр конфигурации, подобно полям файлов, либо объединенный в текстовом поле.

## Добавлена поддержка oEmbed-ресурсов которые не имеют явной высоты

* [#2966043](https://www.drupal.org/node/2966043)

Ранее система oEmbed требовала, чтобы все ресурсы без ссылок (т.е. фотографии, видео, текст) имели явную, ненулевую ширину и высоту, определенные в соответствии со спецификацией oEmbed.

Однако некоторые поставщики oEmbed, такие как Twitter и Instagram, не следуют этому правилу, потому что ресурсы, которые они обслуживают, содержат текст и могут быть адаптивными, поэтому они не могут иметь известную, заранее определенную высоту.

Для того чтобы Drupal мог работать с такими провайдерами, начиная с версии 9.3.0 высота ресурса, не являющегося ссылкой, теперь является необязательной, и если она не указана, то будет возвращаться к максимальной высоте, установленной в форматере.

## oEmbed сервисы теперь запрашивают бэкенд кеша

* [#3222632](https://www.drupal.org/node/3222632)

Медиа система имеет три службы, которые вместе обеспечивают поддержку oEmbed:

* `\Drupal\media\OEmbed\ResourceFetcher`
* `\Drupal\media\OEmbed\ProviderRepository`
* `\Drupal\media\OEmbed\UrlResolver`

Ранее каждый из этих сервисов принимал необязательный параметр `$cache_backend`, который мог быть `NULL` или экземпляром `\Drupal\Core\Cache\CacheBackendInterface`. Они использовали `\Drupal\Core\Cache\UseCacheBackendTrait`, чтобы вызывающий код мог включать/выключать кеширование.

Начиная с текущего изменения, бэкенд кеша всегда должен быть предоставлен, и сервисы больше не используют `UseCacheBackendTrait`. Чтобы отключить кеширование, передайте экземпляр `\Drupal\Core\Cache\NullBackend`. Подклассы должны напрямую вызывать `$this->cacheBackend->get()` и `$this->cacheBackend->set()` соответственно.

## Типы ресурсов JSON:API теперь могут быть программно переименованы

* [#3105318](https://www.drupal.org/node/3105318)

Некоторые сайты могут захотеть раскрыть определенные типы ресурсов JSON:API под определенными именами.

В предыдущих версиях было невозможно переименовывать типы ресурсов, так как не было публичного PHP API, однако некоторые сторонние и пользовательские модули могли задекорировать класс `Drupal\jsonapi\ResourceType\ResourceTypeRepository`. Такой подход означал перезапись методов и приводил к сложному в поддержке решению. Например, `jsonapi_extras` значительно переопределяет класс, чтобы переименовать типы ресурсов.

JSON:API `ResourceTypeBuildEvent` теперь имеет новый метод: `::setResourceTypeName(string $resource_type_name)`. [Подписчики этого события](../../../../9/events/index.md) теперь могут переименовать ресурс, используя публично поддерживаемый PHP API.

Смотрите `\Drupal\jsonapi_test_resource_type_building\EventSubscriber\ResourceTypeBuildEventSubscriber` для примера того, как добавить синоним имени типа ресурса.

Была введена новая константа: `Drupal\jsonapi\ResourceType::TYPE_NAME_URI_PATH_SEPARATOR`. Эта константа представляет собой строку, которая используется в качестве разделителя путей в именах типов ресурсов. Функция `Drupal\jsonapi\ResourceType::getPath()` заменяет "--" на "/" в uri пути. Пример: `node--article` → `node/article`.

## Media Library будет передавать свое текущее состояние в hook_ENTITY_TYPE_create_access()

* [#3224530](https://www.drupal.org/node/3224530)

Модуль Media Library будет передавать в `hook_ENTITY_TYPE_create_access()` объект-значение, содержащий его текущее состояние (экземпляр `Drupal\media_library\MediaLibraryState`) в параметре `$context` при определении доступа к секции "add media" медиатеки. Это позволит сторонним и пользовательским модулям реализовать сложную логику доступа, например, для формы:

```php
use Drupal\Core\Session\AccountInterface;

function mymodule_media_create_access(AccountInterface $account, array $context, $entity_bundle) {
  if (isset($context['media_library_state'])) {
    // $context['media_library_state'] is an instance of Drupal\media_library\MediaLibraryState. Use it as needed to
    // make an access decision.
  }
}
```

Для ясности, полученное решение о доступе будет применяться только к разделу медиатеки "Добавить медиа" (т.е. к той части, которая позволяет пользователю создавать новые медиа элементы), как в виджете поля, так и в CKEditor. Оно НЕ влияет на доступ к разделу медиатеки, позволяющему выбирать существующие медиафайлы, или на доступ к медиатеке в целом.

## Сторонние JavaScript библиотеки теперь управляются через package.json

* [#3219088](https://www.drupal.org/node/3219088)

Для разработки ядра мы теперь используем `package.json` и дополнительный шаг сборки для добавления сторонних скриптов в ядро Drupal. Это сделано для того, чтобы облегчить обновление и упростить процесс обновления. В настоящее время все библиотеки управляются таким образом, кроме CKEditor и modernizr, для которых требуются пользовательские сборки, которые на данный момент не могут быть легко автоматизированы.

**Ранее:**

1. Проверить страницу каждой библиотеки, чтобы узнать, доступна ли новая версия.
2. Скачать последнюю версию библиотеки с github, npm или с сайта библиотеки.
3. Обновить ядро и создать патч.

**Теперь:**

1. Запустить `yarn outdated` чтобы проверить наличие обновлений.
2. Запустить `yarn upgrade <library>` для каждой библиотеки что необходимо обновить.
3. Запустить `yarn vendor-update` которая обновит библиотеку в `core/assets/vendor` директории, а также информацию в `core.libraries.yml`.
4. Отправить изменения в ядро.

## Drupal ядро больше не использует doctrine/reflection

* [#3180351](https://www.drupal.org/node/3180351)

Пакет `doctrine/reflection` больше не используется Drupal ядром потому что пакет заброшен и больше не поддерживается. Код из данного пакета на который опирается Drupal был скопирован в `Drupal\Component\Annotation\Doctrine`. Чтобы использовать этот код в сторонних и собственных модулях, вам требуется обновить пространства имён:

* `Doctrine\Common\Reflection\StaticReflectionParser` → `Drupal\Component\Annotation\Doctrine\StaticReflectionParser`
* `Doctrine\Common\Reflection\ClassFinderInterface` → `Drupal\Component\ClassFinder\ClassFinderInterface`

Пакет `doctrine/reflection` останется в зависимостях до [Drupal 10](../../../../10/index.md) где и будет удалён. Однако, если вы хотите получить полную совместимость со всеми версиями PHP, код придется отрефакторить раньше, так как пакет не совместим с PHP 8.1.

## Добавлен слой совместимости для InputBag Symfony

* [#3162016](https://www.drupal.org/node/3162016)

Symfony 5.2 представил `Symfony\Component\HttpFoundation\InputBag` для замены `Symfony\Component\HttpFoundation\ParameterBag` в некоторых случаях, и метод `Symfony\Component\HttpFoundation\InputBag::get()` был помечен устаревшим для нестроковых возвращаемых значений. Поэтому для получения нестрокового возвращаемого значения вместо него следует использовать `Drupal\Core\Http\InputBag::all()`. Класс `Drupal\Core\Http\InputBag` будет заменен классом `Symfony\Component\HttpFoundation\InputBag`, когда Symfony 4 перестанет поддерживаться Drupal.

Основное отличие в том что `::all()` теперь может принимать параметр.

**Ранее:**

```php
$ajax_page_state = $request->request->get('ajax_page_state', []);
```

Второй параметр опциональный и используется как значение по умолчанию если значение по ключу первого параметра отсутствует.

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

## Шаблоны полей теперь учитывают настраиваемое отображение поля

* [#3043840](https://www.drupal.org/node/3043840)

Это изменение затрагивает только те сайты, которые включили настраиваемое отображение базовых полей типов материалов (`title`, `uid` и `created`) с помощью хука:

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

Для этих базовых полей использовался специальный шаблон поля, который отображал значения в строке и не отображал метку или многие атрибуты и классы обычного поля.

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

Если включено настраиваемое отображение, будет использоваться стандартный шаблон поля. Для поддержания обратной совместимости это происходит только в том случае, если было установлено дополнительное свойство типа сущности (введенное [в этом изменении](https://www.drupal.org/node/2925634)):

```php
function mymodule_entity_type_build(array &$entity_types) {
  $entity_types['node']->set('enable_base_field_custom_preprocess_skipping', TRUE);
}
```

### Изменения в шаблонах

В стандартных темах (Stable, Classy, Bartik) шаблоны полей типов материалов `field--node--title.html.twig`, `field--node--created.html.twig`, `field--node--uid.html.twig` были изменены для проверки новой переменной `is_inline`:

```php
{% if not is_inline %}
  {% include "field.html.twig" %}
{% else %}
...
```

Разработчики тем, переопределяющие любой из этих шаблонов, должны внести соответствующие изменения. В прочих случаях, когда тема наследует эти шаблоны от одной из тем ядра, изменения не требуются.

## Bartik

* [#2725539](https://www.drupal.org/node/2725539) Улучшена контрастность различных состояний при наведении и фокусировке элементе.

## Batch System

* [#2875279](https://www.drupal.org/node/2875279) Обновлён код в модулях Drupal-ядра который использовал `batch_set()`. Теперь для формирования [пакетной обработки](../../../../9/batches/index.md) ядро использует `BatchBuilder`.

## Block

* [#2268787](https://www.drupal.org/node/2268787) Для форм плагинов блоков в `$form_state` больше не передаётся `block_theme` значение, так как оно было крайне ненадёжно и приводило только к проблемам.
* [#2839558](https://www.drupal.org/node/2839558) Блокам добавлена контекстуальная ссылка «Удалить».

## Book

* [#2412669](https://www.drupal.org/node/2412669) `BookManager` больше не использует `drupal_static_reset()`, вместо этого используйте `\Drupal::service('book.manager')->resetCache();`.

## Comment

* [#2927874](https://www.drupal.org/node/2927874) Исправлена неполадка из-за которой предпросмотр комментария показывался в неположенном месте.

## Composer

* [#3224000](https://www.drupal.org/node/3224000) Зависимости ядра обновлены на 27.07.2021.
* [#3225733](https://www.drupal.org/node/3225733) Удалены следующие зависимости ядра: `fabpot/goutte` и `behat/mink-goutte-driver`.

## Configuration System

* [#2926729](https://www.drupal.org/node/2926729) `ConfigManagerInterface::findConfigEntityDependents()` и `ConfigManagerInterface::findConfigEntityDependentsAsEntities()` теперь `ConfigManagerInterface::findConfigEntityDependencies()` и `ConfigManagerInterface::findConfigEntityDependenciesAsEntities()` соответственно.

## Content Moderation

* [#3211072](https://www.drupal.org/node/3211072) Плагин `\Drupal\content_moderation\Plugin\Derivative\DynamicLocalTasks` теперь требует передавать `Router` в конструктор.
* [#3226516](https://www.drupal.org/node/3226516) Удалён дублирующий вызов `::drupalGet()` в `ModerationStateNodeTypeTest`.

## CKeditor

* [#2556069](https://www.drupal.org/node/2556069) Исправлена неполадка приводящая к JS ошибке при использовании фильтра `filtered_html`.

## Database System

* [#3211780](https://www.drupal.org/node/3211780) `Connection::queryTemporary()` помечен устаревшим.
* [#3224199](https://www.drupal.org/node/3224199) Свойство `Connection::$temporaryNameIndex` помечено устаревшим.

## Entity Reference

* [#3225947](https://www.drupal.org/node/3225947) Удалён бесполезный файл `entity_reference.install` и `simpletest_install()`.

## Field System

* [#3184542](https://www.drupal.org/node/3184542) Максимальная длина для ввода метки поля увеличена со 128 до 255 символов.
* [#2226811](https://www.drupal.org/node/2226811) Исправлен тайпхинт для параметра `$definition` в `FieldItemBase`. Ранее он указывал что тип должен быть `DataDefinitionInterface`, но на самом деле ожидал `ComplexDataDefinitionInterface`.
* [#3218711](https://www.drupal.org/node/3218711) Добавлен тест покрывающий максимальный размер загрузки равный '300 0'. Данное значение невалидно и должно выдавать ошибку.

## Field UI

* [#2726881](https://www.drupal.org/node/2726881) Удалена пагинация со страницы `admin/reports/fields`.

## File System

* [#3224420](https://www.drupal.org/node/3224420) `throw new FileTransferException()` теперь возвращает `0` вместо `NULL`.

## Filter

* [#3224478](https://www.drupal.org/node/3224478) Ссылка в `/filter/tip` ведущая на <http://www.w3.org/TR/html/> изменена на новую — <https://html.spec.whatwg.org/>.

## Forms System

* [#3219541](https://www.drupal.org/node/3219541) Удалён избыточный вызов `$this->requestStack->getCurrentRequest()` в `FormBuilder::buildForm()`.

## Image

* [#3216106](https://www.drupal.org/node/3216106) Улучшено описание для плагина эффекта изображения `image_convert`.

## Install system

* [#3185768](https://www.drupal.org/node/3185768) Из установщика удалены `InlineServiceDefinitionsPass`, `RemoveUnusedDefinitionsPass`, `AnalyzeServiceReferencesPass` и `ReplaceAliasByActualDefinitionPass` для того чтобы избежать бесполезную работу. Это позволяет ускорить установку ([-19%](https://blackfire.io/profiles/compare/612e435c-5c03-48b3-99a3-80c1846396e1/graph?settings%5Bdimension%5D=wt&settings%5Bdisplay%5D=landscape&settings%5BtabPane%5D=nodes&selected=&callname=main())) путём снижения количества вызовов функций.

## JavaScript

* [#3212747](https://www.drupal.org/node/3212747) Удалено присвоение `BABEL_ENV` для скриптов сборки CSS и jQuery UI.

## JSON:API

* [#3036593](https://www.drupal.org/node/3036593) ID сущности теперь содержится в `meta.drupal_internal__target_id`. Это позволяет фильтровать значения по данному свойству и получать внутренний ID, а не только UUID.

## Language System

* [#3208373](https://www.drupal.org/node/3208373) Улучшено описание для `LanguageNegotiationContentEntity`. Теперь в нём говорится что за определение языка материала отвечают [сервисы](../../../../9/services/index.md) с метками `language_content_entity`. 

## Layout Builder

* [#3035174](https://www.drupal.org/node/3035174) Трейт `SectionStorageTrait` помечен устаревшим в пользу `SectionListTrait`.

## Media System

* [#3222486](https://www.drupal.org/node/3222486) Метки для удалённых видео (remote video) обновлены таким образом, что они теперь более последовательны и менее многословны.
* [#3222282](https://www.drupal.org/node/3222282) Из файла `media_library.module` удалён `@todo` на ишью [#2964789](https://www.drupal.org/project/drupal/issues/2964789).

## Menu UI

* [#3221493](https://www.drupal.org/node/3221493) Добавлены тесты покрывающие порядок меню в форме настроек типов материалов (`node`).
* [#3222465](https://www.drupal.org/node/3222465) `MenuUiNodeTypeTest` теперь использует специальную тестовую конфигурацию вместо системной.

## Migration system

* [#3222168](https://www.drupal.org/node/3222168) Везде где в качестве сигнатуры использовался `\GuzzleHttp\Client` теперь используется `\GuzzleHttp\ClientInterface`.
* [#3215836](https://www.drupal.org/node/3215836) Добавлена новая константа `MigrateSourceInterface::NOT_COUNTABLE` которую необходимо использовать для неисчисляемых источников.

## MySQL DB driver

* [#3224245](https://www.drupal.org/node/3224245) Подключение к MySQL теперь открывается с использованием `\PDO::ATTR_STRINGIFY_FETCHES`.

## Node system

* [#3220956](https://www.drupal.org/node/3220956) Удалён `@todo` из шаблона `node.html.twig` напоминающий удалить `id` аттрибут, который был уже удалён.
* [#3037202](https://www.drupal.org/node/3037202) `node_mark()` больше не использует `drupal_static()`, соответственно, вызов `drupal_static()` с аргументом `node_mark` помечен устаревшим.
* [#3156244](https://www.drupal.org/node/3156244) `SyndicateBlock` теперь задаёт заголовок равный названию сайта.

## Olivero

* [#3200370](https://www.drupal.org/node/3200370) Улучшено оформление `drop-button` элемента, для того чтобы он соответствовал новому оформлению форм.
* [#3174107](https://www.drupal.org/node/3174107) Добавлены тесты для темы Olivero.
* [#3223314](https://www.drupal.org/node/3223314) Библиотекам `olivero.libraries.yml` добавлены версии и отсортированы в алфавитном порядке.

## Path

* [#3224592](https://www.drupal.org/node/3224592) `\Drupal\path_alias\AliasManager::cacheClear()` больше не вызывает предупреждения об устаревшем коде на PHP 8.1 и не пытается очистить кеш при `NULL` значении.

## Plugin System

* [#1932810](https://www.drupal.org/node/1932810) Плагин-условия `NodeType` упразднён в пользу `\Drupal\entity\Plugin\Core\Condition\EntityBundle`, который был перенесён в ядро из модуля ctools.

## REST

* [#3002352](https://www.drupal.org/node/3002352) `CacheableHttpException` теперь передаёт `$headers` аргумент в `HttpException`.

## Routing System

* [#3183036](https://www.drupal.org/node/3183036) Сервисы проверки прав доступа, что не используются ни одним маршрутом, больше не инициализируются.

## System

* [#778346](https://www.drupal.org/node/778346) Функция `system_sort_modules_by_info_name()` помечена устаревшей и заменена идентичной `system_sort_by_info_name()`. Это переименование сделано так как старое название не совсем подходящее.

## Taxonomy

* [#3039055](https://www.drupal.org/node/3039055) Удалён вызов `drupal_static_reset('taxonomy_term_count_nodes');` так как данное значение никем не устанавливается.
* [#3056258](https://www.drupal.org/node/3056258) В форму добавления и редактирования термина таксономии добавлено новое действие «Сохранить и перейти к списку» — которое после сохранения материала перенаправит на страницу со всеми терминами словаря.
* [#3037157](https://www.drupal.org/node/3037157) Исправлена неполадка из-за которой переводы словаря игнорировались в заголовках страниц «entity.taxonomy_vocabulary.overview_form» и «entity.taxonomy_vocabulary.reset_form».
* [#3221149](https://www.drupal.org/node/3221149) Валидатор аргументов Views `\Drupal\taxonomy\Plugin\views\argument_validator\Term` помечен устаревшим. Вместо него используйте `\Drupal\views\Plugin\views\argument_validator\Entity`.

## Text

* [#3067116](https://www.drupal.org/node/3067116) Функция `text_summary()` теперь корректно закрывает HTML теги когда используется фильтр `filter_html`.

## Symfony 6

* [#3209617](https://www.drupal.org/node/3209617) `Symfony\Component\HttpFoundation\RequestStack::getMasterRequest()` помечен устаревшим, необходимо использовать `::getMainRequest()`. Добавлен прокси-класс `Drupal\Core\Http\RequestStack`, который теперь возвращается сервисом `request_stack`.

## Umami demo

* [#3129666](https://www.drupal.org/node/3129666) В блоке брендирования для названия сайта больше не добавляется класс `visually-hidden`, который прятал заголовок даже если его необходимо показывать.

## Update

* [#3039074](https://www.drupal.org/node/3039074) `drupal_static()` больше не используется в функциях `_update_manager_unique_identifier()`, `_update_manager_extract_directory()` и `_update_manager_cache_directory()`.
* [#3206293](https://www.drupal.org/node/3206293) Добавлен класс `ProjectRelease`, который является обёрткой для Update XML файла с релизами.
* [#2715145](https://www.drupal.org/node/2715145) Удалена конфигурация `system.authorize`.
* [#3180382](https://www.drupal.org/node/3180382) `UpdateManagerUpdate.php` больше не использует сервис `renderer` для элемента `last_check`.

## User

* [#2819585](https://www.drupal.org/node/2819585) Исправлен дублирующий `switch case` в `core/modules/user/user.js`.
* [#2946](https://www.drupal.org/node/2946) Теперь, при попытке авторизоваться с отключенными Cookies, будет показано соответствующее сообщение, что авторизация невозможна.
* [#3221258](https://www.drupal.org/node/3221258) Роль редактора присваивает только те права доступа, что доступны на момент установки.

## Views

* [#2511892](https://www.drupal.org/node/2511892) Исправлена неполадка, приводящая к исключению `MissingMandatoryParametersException` при использовании вкладки меню и `%` в пути представления.
* [#2681947](https://www.drupal.org/node/2681947) Представления типа «Блок» теперь поддерживают настройку «Put the exposed form in a block».
* [#1551534](https://www.drupal.org/node/1551534) Views AJAX теперь поддерживают элемент `<button>` в качестве кнопки отправки, который может появиться в случае переопределения стандартного `<input type="submit">`.
* [#2560447](https://www.drupal.org/node/2560447) `views_form_callback` больше не поддерживается.

## Workspaces

* [#3112783](https://www.drupal.org/node/3112783) Добавлены страницы для отображения изменений и их количества внесённых в рабочей области.

## Тестирование

* [#3091870](https://www.drupal.org/node/3091870) Ошибки JavaScript выброшенные в `FunctionalJavascript` тестах теперь отлавливаются. Начиная с Drupal 10 они будут проваливать тесты.
* [#2758357](https://www.drupal.org/node/2758357) Добавлена документация о том, что `core/phpunit.xml.dist` должен быть скопирован в `core/phpunit.xml` для последующей модификации.
* [#3131900](https://www.drupal.org/node/3131900) Исправлены сравнения чьи результаты записываются в переменную.

## Прочие изменения

* [#3218968](https://www.drupal.org/node/3218968) Drupal теперь поддерживает `NULL`-сервисы. Например: `Acme\Foo: ~`.
* [#2902540](https://www.drupal.org/node/2902540) Исправлены ошибки стандарта кодирования `Drupal.NamingConventions.ValidGlobal`.
* [#1306624](https://www.drupal.org/node/1306624) Файл `router_installer_test.install` переименован `router_installer_test.module`.
* [#1884836](https://www.drupal.org/node/1884836) В `DiffEngine` вызовы `md5()` заменены на `crc32b()`.
* [#2725435](https://www.drupal.org/node/2725435) Удалён устаревший `@todo` ведущий на [#2364011](https://www.drupal.org/node/2364011).
* [#2830352](https://www.drupal.org/node/2830352) Обновлены ссылки ведущие на документацию Drupal 7.
* [#3018091](https://www.drupal.org/node/3018091) Дополнена документация для `TaggedHandlersPass::process()`.
* [#3203416](https://www.drupal.org/node/3203416) Добавлено объяснение, что параметр $form_id может обнаружить рекурсию в `FormValidator::doValidateForm()`.
* [#3127716](https://www.drupal.org/node/3127716) Исправлена опечатка в документации `PathValidator`.