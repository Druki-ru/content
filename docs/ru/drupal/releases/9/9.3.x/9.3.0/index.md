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

* [#3124762](https://www.drupal.org/node/3124762)

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

В примере выше, аргумент `$cache` имеет тип `\Drupal\Core\Cache\CacheBackendInterface`, но у нас множество реализаций данного интерфейса и они имеют разные сервисы. В примере выше мы явно указали что данный параметр ожидает сервис `@cache.default` в качестве аргумента, а все остальные параметры будут автоматически подключены, как и прежде.

Для более детальной информации об автомонтировании сервисов [изучите документацию Symfony](https://symfony.com/doc/current/service_container/autowiring.html).

## Bartik

* [#2725539](https://www.drupal.org/node/2725539) Улучшена контрастность различных состояний при наведении и фокусировке элементе.

## Batch System

* [#2875279](https://www.drupal.org/node/2875279) Обновлён код в модулях Drupal-ядра который использовал `batch_set()`. Теперь для формирования [пакетной обработки](../../../../9/batches/index.md) ядро использует `BatchBuilder`.

## Block

* [#2268787](https://www.drupal.org/node/2268787) Для форм плагинов блоков в `$form_state` больше не передаётся `block_theme` значение, так как оно было крайне ненадёжно и приводило только к проблемам.
* [#2839558](https://www.drupal.org/node/2839558) Блокам добавлена контекстуальная ссылка «Удалить».

## Comment

* [#2927874](https://www.drupal.org/node/2927874) Исправлена неполадка из-за которой предпросмотр комментария показывался в неположенном месте.

## Content Moderation

* [#3211072](https://www.drupal.org/node/3211072) Плагин `\Drupal\content_moderation\Plugin\Derivative\DynamicLocalTasks` теперь требует передавать `Router` в конструктор.

## Database System

* [#3211780](https://www.drupal.org/node/3211780) `Connection::queryTemporary()` помечен устаревшим.

## Field System

* [#3184542](https://www.drupal.org/node/3184542) Максимальная длина для ввода метки поля увеличена со 128 до 255 символов.

## Field UI

* [#2726881](https://www.drupal.org/node/2726881) Удалена пагинация со страницы `admin/reports/fields`.

## Forms System

* [#3219541](https://www.drupal.org/node/3219541) Удалён избыточный вызов `$this->requestStack->getCurrentRequest()` в `FormBuilder::buildForm()`.

## JavaScript

* [#3212747](https://www.drupal.org/node/3212747) Удалено присвоение `BABEL_ENV` для скриптов сборки CSS и jQuery UI.

## JSON:API

* [#3036593](https://www.drupal.org/node/3036593) ID сущности теперь содержится в `meta.drupal_internal__target_id`. Это позволяет фильтровать значения по данному свойству и получать внутренний ID, а не только UUID.

## Image

* [#3216106](https://www.drupal.org/node/3216106) Улучшено описание для плагина эффекта изображения `image_convert`.

## Language System

* [#3208373](https://www.drupal.org/node/3208373) Улучшено описание для `LanguageNegotiationContentEntity`. Теперь в нём говорится что за определение языка материала отвечают [сервисы](../../../../9/services/index.md) с метками `language_content_entity`. 

## Layout Builder

* [#3035174](https://www.drupal.org/node/3035174) Трейт `SectionStorageTrait` помечен устаревшим в пользу `SectionListTrait`.

## Olivero

* [#3200370](https://www.drupal.org/node/3200370) Улучшено оформление `drop-button` элемента, для того чтобы он соответствовал новому оформлению форм.

## Routing System

* [#3183036](https://www.drupal.org/node/3183036) Сервисы проверки прав доступа, что не используются ни одним маршрутом, больше не инициализируются.

## Taxonomy

* [#3039055](https://www.drupal.org/node/3039055) Удалён вызов `drupal_static_reset('taxonomy_term_count_nodes');` так как данное значение никем не устанавливается.
* [#3056258](https://www.drupal.org/node/3056258) В форму добавления и редактирования термина таксономии добавлено новое действие «Сохранить и перейти к списку» — которое после сохранения материала перенаправит на страницу со всеми терминами словаря.
* [#3037157](https://www.drupal.org/node/3037157) Исправлена неполадка из-за которой переводы словаря игнорировались в заголовках страниц «entity.taxonomy_vocabulary.overview_form» и «entity.taxonomy_vocabulary.reset_form».

## Text

* [#3067116](https://www.drupal.org/node/3067116) Функция `text_summary()` теперь корректно закрывает HTML теги когда используется фильтр `filter_html`.

## Symfony 6

* [#3209617](https://www.drupal.org/node/3209617) `Symfony\Component\HttpFoundation\RequestStack::getMasterRequest()` помечен устаревшим, необходимо использовать `::getMainRequest()`. Добавлен прокси-класс `Drupal\Core\Http\RequestStack`, который теперь возвращается сервисом `request_stack`.

## Update

* [#3039074](https://www.drupal.org/node/3039074) `drupal_static()` больше не используется в функциях `_update_manager_unique_identifier()`, `_update_manager_extract_directory()` и `_update_manager_cache_directory()`.
* [#3206293](https://www.drupal.org/node/3206293) Добавлен класс `ProjectRelease`, который является обёрткой для Update XML файла с релизами.

## User

* [#2819585](https://www.drupal.org/node/2819585) Исправлен дублирующий `switch case` в `core/modules/user/user.js`.
* [#2946](https://www.drupal.org/node/2946) Теперь, при попытке авторизоваться с отключенными Cookies, будет показано соответствующее сообщение, что авторизация невозможна.

## Views

* [#2511892](https://www.drupal.org/node/2511892) Исправлена неполадка, приводящая к исключению `MissingMandatoryParametersException` при использовании вкладки меню и `%` в пути представления.
* [#2681947](https://www.drupal.org/node/2681947) Представления типа «Блок» теперь поддерживают настройку «Put the exposed form in a block».

## Workspaces

* [#3112783](https://www.drupal.org/node/3112783) Добавлены страницы для отображения изменений и их количества внесённых в рабочей области.

## Прочие изменения

* [#3218968](https://www.drupal.org/node/3218968) Drupal теперь поддерживает `NULL`-сервисы. Например: `Acme\Foo: ~`.
* [#2902540](https://www.drupal.org/node/2902540) Исправлены ошибки стандарта кодирования `Drupal.NamingConventions.ValidGlobal`.
* [#1306624](https://www.drupal.org/node/1306624) Файл `router_installer_test.install` переименован `router_installer_test.module`.