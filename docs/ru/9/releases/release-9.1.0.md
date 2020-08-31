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

## Изменения в системе событий Symfony

- [#3055194](https://www.drupal.org/project/drupal/issues/3055194), [#3055198](https://www.drupal.org/project/drupal/issues/3055198), [#3153803](https://www.drupal.org/project/drupal/issues/3153803)
- [Simpler event dispatching](https://symfony.com/blog/new-in-symfony-4-3-simpler-event-dispatching) (англ.), Symfony.

Сигнатура метода `Drupal\Component\EventDispatcher\ContainerAwareEventDispatcher::dispatch()` была обновлена для соответствия `Symfony\Component\EventDispatcher\EventDispatcherInterface::dispatch()`. Это значит, что теперь для вызова события, первым аргументом передаётся объект события, а его название вторым.

Это позволяет вызывать событие без передачи названия события. Например:

```php
// Было
$dispatcher->dispatch(MyEvents::EVENT_NAME, new MyEvent());
// Стало
$dispatcher->dispatch(new MyEvent());
```

Из этого изменения также следует то, что теперь можно подписываться не на конкретные события, а также на объекты событий. Например:

```php
public static function getSubscribedEvents() {
  return [
    // Было
    MyEvents::EVENT_NAME => 'onMyEvent',
    // Стало
    MyEventName::class => 'onMyEvent',
  ];
} 
```

В связи с этим, класс события `Symfony\Component\EventDispatcher\Event` помечен устаревшим и вместо него используется `Symfony\Contracts\EventDispatcher\Event` при создании своих событий.

Для поддержки двух вариантов, в Drupal добавлен собственный класс `Drupal\Component\EventDispatcher\Event`. Его рекомендуется использовать вместо старого и нового от Symfony. В таком случае, чтобы обновить код, вам всего лишь потребуется заменить в `use` строке файла события `Symfony` на `Drupal`.

```php
// Было
use Symfony\Component\EventDispatcher\Event;
// Стало
use Drupal\Component\EventDispatcher\Event;
```

## Использование Drupal::theme() заменено на DI в ViewEditForm и HtmlRenderer

- [#3123210](https://www.drupal.org/project/drupal/issues/3123210)

В классах `ViewEditForm` и `HtmlRenderer` использование `\Drupal::theme()` заменено на [Dependency Injection](../services/dependency-injection.md), в связи с чем, в консторе появился новый аргумент.

## Изменена сигнатура конструктора LayoutBuilder и добавено новое событие LayoutBuilderEvents::PREPARE_LAYOUT

- [#3143635](https://www.drupal.org/project/drupal/issues/3143635)

Класс `LayoutBuilder` теперь принимает `EventDispatcherInterface $event_dispatcher` вместо `LayoutTempstoreRepositoryInterface $layout_tempstore_repository`. Также удалено внедрение `messenger` [сервиса](../services/services.md).

**Раньше:**

```php
  public function __construct(array $configuration, $plugin_id, $plugin_definition, LayoutTempstoreRepositoryInterface $layout_tempstore_repository, MessengerInterface $messenger) {
    parent::__construct($configuration, $plugin_id, $plugin_definition, $layout_tempstore_repository, $messenger);
  }
```

**Теперь:**

```php
  public function __construct(array $configuration, $plugin_id, $plugin_definition, EventDispatcherInterface $event_dispatcher) {
    parent::__construct($configuration, $plugin_id, $plugin_definition, $event_dispatcher);
  }
```

Также было добавлено новое событие `LayoutBuilderEvents::PREPARE_LAYOUT` которое вызываетяс в момент подготовки макета и передаёт `Drupal\layout_builder\Event\PrepareLayoutEvent` в качестве события. Это событие позволяет модулям взаимодействовать между собой в процессе `#pre_render` элемента.

## Заголовок ответа теперь содержит X-Drupal-Cache-Max-Age

- [#3089957](https://www.drupal.org/project/drupal/issues/3089957)

Теперь, при включении отладки для сайта, будет дополнительно добавляться заголовок `X-Drupal-Cache-Max-Age` к ответу. Данный заголовок будет содержать в качестве значения минимальное значение `max-age` среди всех рендер массивов отображаемых на странице.

## Настройки ядра теперь могут быть помечены как устаревшие

- [#3163226](https://www.drupal.org/project/drupal/issues/3163226)

Для того чтобы пометить [настройку ядро](../settings-php.md) устаревшей необходимо добавить информацию в массив `Settings::$deprecatedSettings`.

В качестве ключа должно быть название устаревшей настройки, а в качестве значения массив:

- `replacement`: Название новой настройки, что заменяет устаревшую.
- `message`: Сообщение выводимое при использовании старой настройки.

Пример:

```php
  private static $deprecatedSettings = [
    'old_setting' => [
      'replacement' => 'new_setting',
      'message' => 'The "old_setting" setting is deprecated in drupal:9.1.0 and is removed from drupal:10.0.0. Use "new_setting" instead. See https://www.drupal.org/node/CR-NID.',
    ],
  ];
```

## Библиотекам теперь указывается прямой путь на файл лицензии

- [#2897837](https://www.drupal.org/project/drupal/issues/2897837)

Все пути до лицензий в `core.libraries.yml` заменены на прямые адреса. Таким образом, по адресу будет открываться только содержание лицензии.

**Ранее:**

```yaml
url: https://github.com/jquery/jquery-ui/blob/1.12.1/LICENSE.txt
```

**Теперь:**

```yaml
url: https://raw.githubusercontent.com/jquery/jquery-ui/1.12.1/LICENSE.txt
```

Контрибным и собственым решениям рекомендуется делать подобным образом.

## Создание экземпляров Drupal\Core\Database\Query\Condition через new помечено устаревшим

- [#3130655](https://www.drupal.org/project/drupal/issues/3130655)

Создание экземпляров `Drupal\Core\Database\Query\Condition` при помощи `new` помечено устаревшим. Экземпляр необходимо создавать через вызов метода `Drupal\Core\Database\Connection::condition()`. Это связано с тем что для различных типов БД, могут быть разные `Condition` объекты.

**Ранее:**

```php
  use Drupal\Core\Database\Query\Condition;

  $condition = new Condition('OR');
```

**Теперь:**

```php
  use Drupal\Core\Database\Database;

  $condition = Database::getConnection()->condition('OR');
```

## Функция user_password() заменена новым сервисом password_generator

- [#3153085](https://www.drupal.org/project/drupal/issues/3153085)

Функция `user_password()` поемечена устаревшей, взамен добавлен новый сервис `passowrd_generator`.

**Ранее:**

```php
user_password()
```

**Теперь:**

```php
\Drupal::service('password_generator')->generate()
```

## Добавлен новый класс Drupal\jsonapi\CacheableResourceResponse

- [#3072076](https://www.drupal.org/project/drupal/issues/3072076)

В предыдущих версиях JSON:API возвращал объекты `ResourceResponce` на все запросы, при этом он реализует `CacheableResponseInterface`, даже для ответов которые не должны кэшироваться. Это могло приводить к фатальным ошибкам при определённых условиях.

Сейчас JSON:API конкретизирует ответы что кэшируются и не кэшируются, в связи с этим представлен новый класс `CacheableResourceResponse`. Данный класс реализует `CacheableResponseInterface`. Таким образом `ResourceResponce` более не реализует `CacheableResponseInterface`.

`ResourceResponce` помечен для внутреннего использования и у вас не должно быть кода который может задеть данное изменение. Тем не менее, если по каким-то причинам ваш код полагается на данные классы, пожалуйста, произведите рефактор кода с использованием нового `CacheableResourceResponse` (это касается ответов `HEAD`, `OPTIONS` и `GET`).

## Добавлено новое исключение \Drupal\Core\Queue\DelayedRequeueException

- [#3116478](https://www.drupal.org/project/drupal/issues/3116478)

Новое исключение `DelayedRequeueException` позволяет обработчику очереди откладывать дальнейшую обработку элементов до следующего цикла или на определенный период пока не истечёт время блокировки элемента. Очереди, поддерживающие данную возможность должны реализовывать интерфейс `DelayableQueueInterface`.

Ранее было невозможно отложить обработку конкретного элемента очереди без выбрасывания исключения с последующие логированием или приостановкой всей очереди. Новый обработчик исключений работает «тихо» по умолчанию и позволяет отложить конкретный элемнет для последующей обработки.

Для этого достаточно выбросить исключение `\Drupal\Core\Queue\DelayedRequeueException` в обработчике очереди.

```php
// Delay processing of this item until the next queue worker cycle.
throw new DelayedRequeueException();

// Delay processing of this item for 10 seconds (if supported by the queue).
throw new DelayedRequeueException(10);
```

Если очередь не поддерживает данную особенность, то исключение будет проигнорирвоано и элемент останется заблокированным на стандартный пероид.

`\Drupal\Core\Queue\DatabaseQueue` теперь реализует `DelayableQueueInterface`.

## Block

- [#3105976](https://www.drupal.org/project/drupal/issues/3105976) В `BlockViewBuilder::buildPreRenderableBlock()` для аргумента `$entity` добавлен тайпхинт `\Drupal\block\BlockInterface`.
- [#2151001](https://www.drupal.org/project/drupal/issues/2151001) Для административной страницы «Схема блоков» добавлен Tour.
- [#2890758](https://www.drupal.org/project/drupal/issues/2890758) Видимость блока по типу ноды теперь работает на маршрутах с предварительным просмотром и ревизии.

## CKeditor

- [#3099662](https://www.drupal.org/project/drupal/issues/3099662) `ckeditor_stylesheets` теперь могут указывать путь относительно корня Drupal.
- [#2911527](https://www.drupal.org/project/drupal/issues/2911527) Добавлена возможность использовать `/` при добавлении собственных вариантов стилей.

## Claro

- [#3060697](https://www.drupal.org/project/drupal/issues/3060697) Claro теперь использует `#dropbutton_type` для вариантов `dropbutton` элемента, вместо классов.
- [#3154425](https://www.drupal.org/project/drupal/issues/3154425) Удалён комментарий «@todo Remove this after 8.6.x is out of support.» и код для него.
- [#3105575](https://www.drupal.org/project/drupal/issues/3105575) HTML классы перенесены из `claro_preprocess_textarea()` в шаблон.

## Comment

- [#2984243](https://www.drupal.org/project/drupal/issues/2984243) Кнопка фильтрации в представлении для вывода комментариев теперь содержит значение «Filter» вместо «Apply».
- [#3163685](https://www.drupal.org/project/drupal/issues/3163685) Удалена неиспользуемая переменная `$block` в `CommentBlockTest`.
- [#3163686](https://www.drupal.org/project/drupal/issues/3163686) Удалена неиспользуемая переменная `$comment` в `CommentLinksAlterTest`.
- [#3163425](https://www.drupal.org/project/drupal/issues/3163425) Удалена неиспользуемая переменная `$fields` в `CommentViewsData`.

## Composer

- [#3156558](https://www.drupal.org/project/drupal/issues/3156558) Обновлены зависимости.
- [#3133903](https://www.drupal.org/project/drupal/issues/3133903) Добавлены проверка что все пакеты из `composer.lock` файла ядра имеются и имеют конкретные версии.
- [#3121847](https://www.drupal.org/project/drupal/issues/3121847) В шаблоны проектов [drupal/recommended-project](drupal-recommended-project.md) и [drupal/legacy-project](drupal-legacy-project.md) теперь добавляется новый путь установки `drupal-custom-profile` (`profiles/custom/{$name}/`).
- [#3164349](https://www.drupal.org/project/drupal/issues/3164349) `symfony/var-dumper` теперь указан как dev зависимость в корневом composer.json Drupal.

## Contact

- [#3150227](https://www.drupal.org/project/drupal/issues/3150227) Удалены неиспользуемые переменные `$contact_form` и `$recipients_str`.

## Content Moderation

- [#3044292](https://www.drupal.org/project/drupal/issues/3044292) (откачено) Добавлен новый метод `::isModeratedEntity` для хендлеров moderation сущностей.
- [#3164498](https://www.drupal.org/project/drupal/issues/3164498) Удалена неиспользуемая переменная `$entity_type_ids` в `content_moderation.module`.
- [#3155022](https://www.drupal.org/project/drupal/issues/3155022) Изменена сигнатура `EntityModerationForm::__construct()`. Параметр `$time` теперь имеет более слабые требования и вместо `Time` объекта ожидает экземпляр `TimeInterface`.

## Content Translation

- [#2972308](https://www.drupal.org/project/drupal/issues/2972308) Добавлено новое разрешение `translate editable entities` позволяющее переводить сущности, которые пользователь может редактировать.

## Database System

- [#2278971](https://www.drupal.org/node/2278971) `Connection::supportsTransactions` помечен устаревшим. Таким образом [настройка](../settings-php.md) подключения к БД `transactions` также становится устаревшей.
- [#3143618](https://www.drupal.org/project/drupal/issues/3143618) Обычные пробелы (`U+0020`) теперь заменяются на неделимые пробелы (`U+00A0`), для минимизации ложных срабатываний.
- [#3151990](https://www.drupal.org/project/drupal/issues/3151990) Запросы к БД переписаны на EntityQuery в `NodeRevisionPermissionsTest`.
- [#3151981](https://www.drupal.org/project/drupal/issues/3151981) Запросы к БД переписаны на EntityQuery в `NodeRevisionsAllTest`.
- [#3152001](https://www.drupal.org/project/drupal/issues/3152001) Запросы к БД переписаны на EntityQuery в `NodeAccessBaseTableTest`.
- [#3151959](https://www.drupal.org/project/drupal/issues/3151959) Запросы к БД переписаны на EntityQuery в `PathTaxonomyTermTest`.
- [#3151990](https://www.drupal.org/project/drupal/issues/3151990) Запросы к БД переписаны на EntityQuery в `NodeRevisionPermissionsTest`.
- [#3151968](https://www.drupal.org/project/drupal/issues/3151968) Запросы к БД переписаны на EntityQuery в `NodeTranslationUITest`.
- [#3128616](https://www.drupal.org/project/drupal/issues/3128616) `Drupal\Core\Database\Connection::destroy` помечен устаревшим. Вместо него используется нативный `__destruct()`.
- [#3152415](https://www.drupal.org/project/drupal/issues/3152415) Ключевые имена в статичных запросах `core/lib/Drupal/Core` теперь обёрнуты в квадратные скобки для избежания проблем с зарезервированными именами баз данных.
- [#3151981](https://www.drupal.org/project/drupal/issues/3151981) В `NodeRevisionsAllTest` использование статических запросов заменено на Entity Query.
- [#3152398](https://www.drupal.org/project/drupal/issues/3152398) Статические запросы в `core/tests/Drupal` переписаны на динамические.
- [#3123461](https://www.drupal.org/project/drupal/issues/3123461) Возможность располагать драйвера баз данных в `DRUPAL_ROOT/drivers` помечена устаревшей и будет удалена в Drupal 10.
- [#2999569](https://www.drupal.org/project/drupal/issues/2999569) Теперь, при попытке вставить (`INSERT`) запись в несуществующую колонку и без указания значения по умолчанию в схеме, драйвер MySQL будет выбрасывать исключение `IntegrityConstraintViolationException` в дополнение к текущему `DatabaseExceptionWrapper`.
- [#3120892](https://www.drupal.org/project/drupal/issues/3120892) Для драйвера SQL Lite добавлена поддержа функции `LEAST()`.

## Entity System

- [#3033986](https://www.drupal.org/node/3033986) Удалено перезаписывание `$limit` в некоторых классах расширяющих `EntityListBuilder`. Оно было без значения.
- [#2927077](https://www.drupal.org/project/drupal/issues/2927077) `Entity::toUrl` теперь передает параметр ревизии на все маршруты, чьё название начинается с `revision`. Таким образом, это автоматизирует передачу параметров для кастомных маршрутов типа `revision_revert` и `revision_delete`.
- [#2656570](https://www.drupal.org/project/drupal/issues/2656570) `DraggableListBuilder` теперь рендерит метку через `#plain_text`.
- [#2955442](https://www.drupal.org/project/drupal/issues/2955442) Добавлен новый метод `TableMappingInterface::getAllFieldTableNames()` который возвращает название всех таблиц в которых хранится информация для конкретного поля.

## Extension System

- [#3150726](https://www.drupal.org/project/drupal/issues/3150726) Функция `update_check_incompatibility()` помечена устаревшей.

## Field System

- [#2893789](https://www.drupal.org/project/drupal/issues/2893789) `WidgetBase` теперь использует свой собственный метод `::getFilteredDescription()` для получения описания.
- [#3165188](https://www.drupal.org/project/drupal/issues/3165188) Удалена неиспользуемая переменная `$i` из `FieldOptionTranslation`.
- [#3165191](https://www.drupal.org/project/drupal/issues/3165191) Удалена неиспользуемая переменная `$field_ids` из `FieldAttachStorageTest`.

## File

- [#3070902](https://www.drupal.org/project/drupal/issues/3070902) Для исключения вызываемого в `prepareDestination()` улучшено описание лога.
- [#2991219](https://www.drupal.org/project/drupal/issues/2991219) `template_preprocess_file_link()` больше не добавляет `length` параметр после MIME в `type` аттрибуте.

## Filter

- [#3151101](https://www.drupal.org/project/drupal/issues/3151101) Употребление слов «whitelist» и «blacklist» в Filter модуле заменено на более подходящие.

## Forum

- [#3067622](https://www.drupal.org/project/drupal/issues/3067622) Справка из `hook_help()` конвертирована в Help Topics.

## JSON:API

- [#3165794](https://www.drupal.org/project/drupal/issues/3165794) Удалена неиспользуемая переменная `$account_bundle` в `ResourceTestBase`.
- [#3093757](https://www.drupal.org/project/drupal/issues/3093757) Убраны вызовы `testRelated()` из тестов так как ишьюсы решены.

## Help Topic

- [#3087879](https://www.drupal.org/project/drupal/issues/3087879) Для поиска по Help Topic теперь используется административная [тема оформления](../themes/themes.md).
- [#3095740](https://www.drupal.org/project/drupal/issues/3095740) Справка для `menu_link_content` и `menu_ui` модулей конвертирована в Help Topic.
- [#3164965](https://www.drupal.org/project/drupal/issues/3164965) Удалена неиспользуемая переменная `$source` в `HelpTopicTwigLoaderTest`.
- [#3166763](https://www.drupal.org/project/drupal/issues/3166763) Исправлены двойные пробелы в `help.help_topic_search.html.twig`.
- [#3047703](https://www.drupal.org/project/drupal/issues/3047703) Справки модулей `basic_auth`, `hal`, `jsonapi`, `rdf`, `rest` и `serialization` конвертированы в Help Topic.

## Image

- [#3153009](https://www.drupal.org/project/drupal/issues/3153009) Добавлен новый стиль изображения устанавливаемый с модулем — «Wide (1090)». Он будет использоваться как Hero стиль в будущей теме Olivero.
- [#3097797](https://www.drupal.org/project/drupal/issues/3097797) Улучшена документация для функции `image_filter_keyword()`.
- [#3165350](https://www.drupal.org/project/drupal/issues/3165350) Удалена неиспользуемая переменная `$key` в `MigrateImageCacheTest`.

## Install System

- [#3157895](https://www.drupal.org/project/drupal/issues/3157895) Обновление состояния `install_time` перенесено в `installed_finished()`.
- [#3086307](https://www.drupal.org/project/drupal/issues/3086307) Производительность установки увеличена примерно на ~20%, путем сброса кэша маршрутов после установки всех модулей, а не после каждого.

## JavaScript

- [#3145930](https://www.drupal.org/project/drupal/issues/3145930) Размер «липкого» заголовка теперь пересчитывается после сворачивания и разворачивания тулбара.
- [#3096516](https://www.drupal.org/project/drupal/issues/3096516) В `domready` внесены улучшения, которые решают проблему с [race condition](https://ru.wikipedia.org/wiki/%D0%A1%D0%BE%D1%81%D1%82%D0%BE%D1%8F%D0%BD%D0%B8%D0%B5_%D0%B3%D0%BE%D0%BD%D0%BA%D0%B8).
- [#3152473](https://www.drupal.org/project/drupal/issues/3152473) Улучшена работа обратного вызова domready.

## Install system

- [#2691389](https://www.drupal.org/project/drupal/issues/2691389) Строка «Save and continue» в `InstallerTestBase::setUpLanguage` больше не является переводимой так как на данном этапе только английский язык.

## Layout Builder

- [#3053887](https://www.drupal.org/project/drupal/issues/3053887) В код добавлена документация почему блоки требуют создание новой ревизии при изменении.

## Locale

- [#2925318](https://www.drupal.org/project/drupal/issues/2925318) Для таблицы `locales_location` удалён индекс `sid` так как он покрыт в `string_type`.
- [#3167600](https://www.drupal.org/project/drupal/issues/3167600) Удалена неиспользуемая переменная `$config` в `locale.bulk.inc`.
- [#3167599](https://www.drupal.org/project/drupal/issues/3167599) Удалена неиспользуемая переменная `$frequency` в `locale.module`.

## Media

- [#3142818](https://www.drupal.org/project/drupal/issues/3142818) Из ссылок удалён аттрибут `target=_blank`.
- [#3159793](https://www.drupal.org/project/drupal/issues/3159793) Исправлена опечатка в форме настройки Media Library.
- [#3146492](https://www.drupal.org/project/drupal/issues/3146492) Удалены неиспользуемые переменные в модуле.

## Menu UI

- [#3158562](https://www.drupal.org/project/drupal/issues/3158562) Теперь в интерфейсе всегда ссылка меню упоминается как «menu link», вместо «menu item».
- [#3153394](https://www.drupal.org/project/drupal/issues/3153394) Добавлена документация что меню поддерживает маршрут типа `route:<button>`.

## Migration System

- [#3024682](https://www.drupal.org/node/3024682) На странице со списком миграций теперь показываются человекопонятные названия, вместо машинных.
- [#3143719](https://www.drupal.org/project/drupal/issues/3143719) В `MigrateUpgradeTestBase` добавлен новый метод `getCredentials()`.
- [#2993367](https://www.drupal.org/project/drupal/issues/2993367) Добавлена миграция из Drupal 7 Picture (контрибный модуль) в Responsive Image.
- [#3133139](https://www.drupal.org/project/drupal/issues/3133139) Удалена `is_array` проверка в `getProcessPlugins`.
- [#3134300](https://www.drupal.org/project/drupal/issues/3134300) Упрощена разметка и код в `ReviewForm::buildForm()`.
- [#3154398](https://www.drupal.org/project/drupal/issues/3154398) Миграции теперь могут указывать требуемые плагины для своей работы при помощи нового метода `getRequirements()`.
- [#3110669](https://www.drupal.org/project/drupal/issues/3110669) Добавлена поддержка миграции мультиязычных меню из Drupal 7.
- [#2845485](https://www.drupal.org/project/drupal/issues/2845485) Улучшена документация для плагина `MenuLinkParent`.
- [#3160323](https://www.drupal.org/project/drupal/issues/3160323) Название переменных в исключениях `Row` теперь обёрнуты в одинакрные кавычки.
- [#3143717](https://www.drupal.org/project/drupal/issues/3143717) Добавлены новые хелперы `MigrateUpgradeTestBase::assertIdConflictForm()` и `MigrateUpgradeTestBase::assertReviewForm()`.
- [#3164120](https://www.drupal.org/project/drupal/issues/3164120) Исправлен пример кода в документации плагина `MenuLinkParent`.
- [#2447727](https://www.drupal.org/project/drupal/issues/2447727) Добавлен абстрактный `ReferenceBase` для миграции связующих полей.
- [#3112249](https://www.drupal.org/project/drupal/issues/3112249) Добавлена новая миграция `d7_menu_translation` для миграции переводов меню из Drupal 7.
- [#3164652](https://www.drupal.org/project/drupal/issues/3164652) Для плагина обработчика `Substr` включено исключение cspell для игнорирования `skÅ‚odowska`.
- [#3158277](https://www.drupal.org/project/drupal/issues/3158277) Удалена неиспользуемая переменная `EntityLinkTest`.

## Node System

- [#2830504](https://www.drupal.org/project/drupal/issues/2830504) Исправлена неполадка из-за которой `Drupal\node\Plugin\Action\AssignOwnerNode` позволяла выбрать гостя в качестве владельца ноды.
- [#3165950](https://www.drupal.org/project/drupal/issues/3165950) Из `NodeTypeForm` удалено упоминание что нижние подчёркивания будут конвертированы в дифисы, так как для путей форм сущностей уже используются нижине подчёркивания.

## Route System

- [#3074201](https://www.drupal.org/project/drupal/issues/3074201) Методы `RouteCompiler::getDefaults()`, `RouteCompiler::getRequirements()` и `RouteCompiler::getRequirements()` признаны устаревшими.

## Help Topics

- [#3047723](https://www.drupal.org/project/drupal/issues/3047723) Документация модулей views, views_ui конвертирована в Help Topics.
- [#3067614](https://www.drupal.org/project/drupal/issues/3067614) Документация модулей filter, ckeditor, editor конвертирована в Help Topics.

## PosgreSQL драйвер

- [#3129560](https://www.drupal.org/project/drupal/issues/3129560) Удалена реализация `Upsert`.

## REST

- [#3152848](https://www.drupal.org/project/drupal/issues/3152848) Код связанный с `bc_entity_resource_permissions` настройкой удалён, так как она больше не используется.

## Routing System

- [#3158708](https://www.drupal.org/project/drupal/issues/3158708) Возвращено поведение, что `RouteProvider::getAllRoutes()` возвращает `iterable` результат, которое было изменено в [#2917331](https://www.drupal.org/project/drupal/issues/2917331).

## Search

- [#3086794](https://www.drupal.org/project/drupal/issues/3086794) Плагины результатов поиска теперь могут указывать, какую тему использовать для отрисовки страниц.
- [#3086795](https://www.drupal.org/project/drupal/issues/3086795) «Search help» на странице поиска заменён на «About searching» для избежания двусмысленности.
- [#3155221](https://www.drupal.org/project/drupal/issues/3155221) Удален устаревший «@todo».
- [#3075703](https://www.drupal.org/project/drupal/issues/3075703) Функции для обработки поискового запроса `search_index_split()`, `search_simplify()` и `search_expand_cjk()` перенесены в [сервис](../services/services.md) `search.text_processor`.

## Serialization

- [#3135304](https://www.drupal.org/project/drupal/issues/3135304) Удалён слой обратной совместимости с Symfony 3 из `JsonEncoder`.

## Seven

- [#3054196](https://www.drupal.org/node/3054196) Исправлена проблема с белым фоном у кнопки в таблице.

## Simpletest

- [#3112432](https://www.drupal.org/project/drupal/issues/3112432) Добавлена реализация `hook_requirements()` которая будет постоянно блокировать включение данного модуля на новых сайтах.

## System

- [#3077938](https://www.drupal.org/project/drupal/issues/3077938) Добавлена функция `tableDragHandle` для `Drupal.theme`. Теперь темы могут менять разметку управления сортировкой таблицы.

## Taxonomy

- [#3122511](https://www.drupal.org/node/3122511) На странице редактирования добавлен пункт удаления во вкладки.
- [#3151953](https://www.drupal.org/project/drupal/issues/3151953) В тесте `TermTranslationUITest` использование прямого запроса заменено на Entity Query.

## Update

- [#2303323](https://www.drupal.org/project/drupal/issues/2303323) `update_delete_file_if_stale()` теперь возвращает булевое значение.

## User

- [#3082006](https://www.drupal.org/node/3082006) Поле пароля больше нельзя использовать в Views для вывода. Ранее он не показывал ничего, сейчас отключена возможность выбора данного значения.
- [#3150070](https://www.drupal.org/project/drupal/issues/3150070) Видимость свойств в новых тестах изменена с `public` на `protected`.
- [#2847808](https://www.drupal.org/project/drupal/issues/2847808) Метка для прав доступа `administer permissions` изменена на «Administer roles and permissions».
- [#2193803](https://www.drupal.org/project/drupal/issues/2193803) Переход по невалидной ссылке выхода из аккаунта больше не выдаёт 403, а редиректит на главную страницу сайта.

## Views

- [#3139353](https://www.drupal.org/project/drupal/issues/3139353) Добавлен новый публичный метод `Drupal\views\Plugin\views\query\Sql::getConnection()`.
- [#3150490](https://www.drupal.org/project/drupal/issues/3150490) Улучшено именование переменных в `Drupal\views\ViewExecutableFactory::get`.
- [#2838555](https://www.drupal.org/project/drupal/issues/2838555) Views больше не позволит добавлять связи на данные у которых нет базовой таблицы для джоина (например, конфигурационные сущности).
- [#2780869](https://www.drupal.org/project/drupal/issues/2780869) Исправлена неполадка, при которой невозможно было сохранить представление, если в значении опции для фильтра была точка.
- [#2625136](https://www.drupal.org/project/drupal/issues/2625136) Раскрытые фильтры для `numeric` и `date` полей теперь имеют обертку, для того чтобы поля были на одном уровне.

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
- [#3000762](https://www.drupal.org/project/drupal/issues/3000762) Для `WebAssert` добавлен новый метод `pageContainsNoDuplicateId()`.
- [#3082859](https://www.drupal.org/project/drupal/issues/3082859) `AssertMailTrait::assertMailPattern()` теперь преобразует значение `$regex_found` в булевый тип.
- [#3155761](https://www.drupal.org/project/drupal/issues/3155761) В `BlockFormMessagesTest` использование `assertTrue()` с `stristr()` заменено на `assertStringContainsString()`.
- [#3139440](https://www.drupal.org/project/drupal/issues/3139440) Использование устаревшего `AssertLegacyTrait::buildXPathQuery()` заменено на `$this->assertSession()->buildXPathQuery()`.
- [#3139426](https://www.drupal.org/project/drupal/issues/3139426) Использование устаревшего `AssertLegacyTrait::assertOptionSelected()` заменено на `$this->assertSession()->optionExists()`.
- [#3123120](https://www.drupal.org/project/drupal/issues/3123120) Использование устаревшего `AssertLegacyTrait::pass()` заменено на новые решения.
- [#3155760](https://www.drupal.org/project/drupal/issues/3155760) Использование `array_key_exists()` заменено на `assertArrayHasKey()`.
- [#3139428](https://www.drupal.org/project/drupal/issues/3139428) Использование устаревших `AssertLegacyTrait::assertFieldChecked()` и `AssertLegacyTrait::assertNoFieldChecked()` заменено на `$this->assertSession()->checkboxChecked()`.
- [#3158286](https://www.drupal.org/project/drupal/issues/3158286) Удалены неиспользуемые локальные переменные из `BubbleableMetadataTest`.
- [#3156998](https://www.drupal.org/project/drupal/issues/3156998) `symfony/phpunit-bridge` обновлён с версии 4.4.10 до 5.1.2. Добавлены тесты для `@requires` аннотаций с использованием новой версии пакета.
- [#3142755](https://www.drupal.org/project/drupal/issues/3142755) Прекращена передача устаревшего аргумента `$message` в `AssertLegacyTrait::assertField()` и `AssertLegacyTrait::assertNoField()`.
- [#3158266](https://www.drupal.org/project/drupal/issues/3158266) Удалены неиспользуемые переменные в `TranslationTest`.
- [#2664322](https://www.drupal.org/project/drupal/issues/2664322) Вызов `KernelTestBase::installSchema()` для установки таблиц `key_value` и `key_value_expire` помечен устаревшим. Данные таблицы создаются лениво.
- [#3164161](https://www.drupal.org/project/drupal/issues/3164161) Аннотация `@runInSeparateProcess` перенесена в `SettingsTest` чтобы покрывало все тесты.
- [#3157434](https://www.drupal.org/project/drupal/issues/3157434) Вместо `\Drupal\Tests\Traits\ExpectDeprecationTrait` теперь используется `\Symfony\Bridge\PhpUnit\ExpectDeprecationTrait`.
- [#3139408](https://www.drupal.org/project/drupal/issues/3139408) Использование устаревших `AssertLegacyTrait::assertField()` и `AssertLegacyTrait::assertNoField()` заменено на `$this->assertSession()->fieldExists()`.
- [#3139433](https://www.drupal.org/project/drupal/issues/3139433) Использование устаревших `AssertLegacyTrait::assertEscaped()` и `AssertLegacyTrait::assertNoEscaped()` заменено на `$this->assertSession()->assertEscaped()`.
- [#3139436](https://www.drupal.org/project/drupal/issues/3139436) Использование устаревшего `AssertLegacyTrait::assertPattern()` заменено на `$this->assertSession()->responseMatches()`.
- [#3133355](https://www.drupal.org/project/drupal/issues/3133355) Добавлены новые методы `WebAssert::responseHeaderExists()` и `WebAssert::responseHeaderDoesNotExist()`.
- [#3164589](https://www.drupal.org/project/drupal/issues/3164589) Использование `assertSame()` для заголовков ответов заменено на `$this->assertSession()->responseHeaderEquals()`.
- [#3158280](https://www.drupal.org/project/drupal/issues/3158280) Удалена неиспользуемая переменная в `DefaultLazyPluginCollectionTest`.
- [#3158291](https://www.drupal.org/project/drupal/issues/3158291) Удалены неиспользуемые переменные из `ContainerTest`.
- [#3164686](https://www.drupal.org/project/drupal/issues/3164686) `WebAssert::addressEquals()` и `AssertLegacyTrait::assertUrl()` теперь учитывают query строку.
- [#3163924](https://www.drupal.org/project/drupal/issues/3163924) Изменена сигнатура `EntityViewTrait::buildEntityView()`. Больше он не принимает параметр `$reset`.
- [#3158278](https://www.drupal.org/project/drupal/issues/3158278) Удалены неиспользуемые переменные из `LocalTaskManagerTest`.
- [#3143870](https://www.drupal.org/project/drupal/issues/3143870) Исправлены некорректные вызовы `AssertLegacyTrait::assertUrl()`.
- [#3160405](https://www.drupal.org/project/drupal/issues/3160405) Удалена перегрузка аргументами при вызове методов `WebAssert` в тестах, которые были конвертированы из Simpletest.
- [#3153150](https://www.drupal.org/project/drupal/issues/3153150) Удалено использование `t()` в вызовах `::assertTrue()` и `::assertFalse()`.
- [#3153143](https://www.drupal.org/project/drupal/issues/3153143) Удалено использование `t()` в вызовах `::linkExists()` и `::linkNotExists()`.
- [#3166349](https://www.drupal.org/project/drupal/issues/3166349) Удалено использование `t()` в вызовах `::assertNoText()`.
- [#3131186](https://www.drupal.org/project/drupal/issues/3131186) Сравнения с использованием `::drupalGetHeader()` заменены на `$this->assertSession()->responseHeaderEquals()`.
- [#3158290](https://www.drupal.org/project/drupal/issues/3158290) Удалены неиспользуемые переменные в `ActiveLinkResponseFilterTest`.

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
- [#3055189](https://www.drupal.org/project/drupal/issues/3055189) Маппинг ключей в несколько строк помечен устаревшим в Symfony 4.3, соответствующие изменения внесены в ядро.
- [#2937844](https://www.drupal.org/project/drupal/issues/2937844) Внесены исправления для соответствия стандарту `Squiz.PHP.NonExecutableCode`.
- [#3143713](https://www.drupal.org/project/drupal/issues/3143713) Функция `drupal_get_schema_versions()` теперь всегда возвращает целые числа.
- [#3154594](https://www.drupal.org/project/drupal/issues/3154594) `composer.json` и `composer.lock` будут пропускаться CSpell.
- [#3154665](https://www.drupal.org/project/drupal/issues/3154665) Из словаря CSpell удалены названия модулей и плагинов.
- [#2807743](https://www.drupal.org/project/drupal/issues/2807743) Тригерры ошибок для `FormattableMarkup::placeholderFormat()` приведены к единому стилю.
- [#2619482](https://www.drupal.org/project/drupal/issues/2619482) Использование `get_called_class()` и `get_class($this)` заменены на `static::class`.
- [#2928960](https://www.drupal.org/project/drupal/issues/2928960) Длина слогана сайта увеличена со 128 символов до 255.
- [#3155770](https://www.drupal.org/project/drupal/issues/3155770) Удалены избыточные указания реализации `ContainerFactoryPluginInterface` когда класс расширял уже класс реализующий интерфейс.
- [#3154914](https://www.drupal.org/project/drupal/issues/3154914) Исправлены грамматически ошибки при употреблении множественного и единственного числа.
- [#3157546](https://www.drupal.org/project/drupal/issues/3157546) В `MAINTAINERS.txt` добавлен mondrake в качестве мейнтейнера тест фреймворка.
- [#3157954](https://www.drupal.org/project/drupal/issues/3157954) Из тестов удалены избыточные ребилды маршрутов.
- [#2989262](https://www.drupal.org/project/drupal/issues/2989262) В генератор `.htaccess` файла добавлены экранирования для точек и запятых.
- [#3116858](https://www.drupal.org/project/drupal/issues/3116858) `ExtensionDiscovery` теперь кэширует не объект расширения целиком, а только информацию о нём.
- [#2836194](https://www.drupal.org/project/drupal/issues/2836194) Для ajax throbber увеличен паддинг, чтобы не обрезало край анимации.
- [#3138781](https://www.drupal.org/project/drupal/issues/3138781) Стандартизовано употребление слов «ORed» и «ANDed» в ядре.
- [#3022551](https://www.drupal.org/project/drupal/issues/3022551) Исправлен артикль в документации после притяжательного местоимения.
- [#3151092](https://www.drupal.org/project/drupal/issues/3151092) Слова «whitelist» и «blacklist» в `Drupal\Core\Extension` заменены на `$skippedFolder` и `$allowedExtensionTypes`, соответственно.
- [#3143087](https://www.drupal.org/project/drupal/issues/3143087) В `ModulesListForm` теперь явно объявлено свойство `accessManager`.
- [#3162045](https://www.drupal.org/project/drupal/issues/3162045) Для совместимости с Symfony 5 вместо `new Process()` используется `\Symfony\Component\Process\Process::fromShellCommandline()`.
- [#3120222](https://www.drupal.org/project/drupal/issues/3120222) Ссылки ведущие на документацию Drupal 7 заменены на актуальные.
- [#3151095](https://www.drupal.org/project/drupal/issues/3151095) Употребление «whitelist» и «blacklist» в `\Drupal\Core\Utility\Error` заменены на более подходящие.
- [#3008140](https://www.drupal.org/project/drupal/issues/3008140) Добавлен новый тест `ShutdownFunctionsTest` для тестирования функции отключения Drupal.
- [#3151094](https://www.drupal.org/project/drupal/issues/3151094) Слова «whitelist» и «blacklist» в классах `\Drupal\Core\Template` и их тестах заменены на более подходящие.
- [#3142934](https://www.drupal.org/project/drupal/issues/3142934) Метод `\Drupal\Component\Utility\Bytes::toInt()` помечен устаревшим в пользу `\Drupal\Component\Utility\Bytes::toNumber()`.
- [#3164211](https://www.drupal.org/project/drupal/issues/3164211) Удалены 8 исправленных опечаток из `misc/cspell/dictionary.txt`.
- [#3084441](https://www.drupal.org/project/drupal/issues/3084441) У ссылки на главную страницу сайта в блоке брендирования удалён тег `title`.
- [#3085245](https://www.drupal.org/project/drupal/issues/3085245) Добавлен PostCSS плагин [postcss-url](https://github.com/postcss/postcss-url) который автоматически делает все использования SVG в качестве фона инлайн строкой с оптимизацией.
- [#3123285](https://www.drupal.org/project/drupal/issues/3123285) В `robots.txt` исправлены некорректные правила для страниц регистрации, авторизации, выхода и восстановления пароля.
- [#3151093](https://www.drupal.org/project/drupal/issues/3151093) Употребление «whitelist» и «blacklist» в `\Drupal\Core\Security\RequestSanitizer` и его тесте заменены на более подходящие.
- [#3166317](https://www.drupal.org/project/drupal/issues/3166317) Удалены оставшиеся отсылки к XCache.
- [#2819245](https://www.drupal.org/project/drupal/issues/2819245) «Javascript» теперь упоминается в коде как «JavaScript».