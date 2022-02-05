---
title: 'Drupal 9.1.0'
slug: wiki/drupal/releases/9.1.0
core: 9
metatags:
  title: 'Drupal 9.1.0: Список изменений'
  description: 'Список изменений Drupal 9.1.0.'
authors:
  - Niklan
  - arraksis
category:
  area: 'Drupal 9.1.x'
  title: Drupal 9.1.0
  order: 0
---

**Дата релиза**: 2 декабря 2020\
**Активная поддержка до**: 2 июня 2021\
**Поддержка безопасности до**: ноябрь 2021

## Cache::merge* методы теперь принимают неограниченное кол-во аргументов

- [#3125032](https://www.drupal.org/node/3125032)

Следующие методы теперь принимают неограниченное количество аргументов:

- `\Drupal\Core\Cache\Cache::mergeTags()`
- `\Drupal\Core\Cache\Cache::mergeMaxAges()`
- `\Drupal\Core\Cache\Cache::mergeContexts()`

### Пример

Имеем следующие кеш-теги:

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

В классах `ViewEditForm` и `HtmlRenderer` использование `\Drupal::theme()` заменено на [Dependency Injection](../../../../9/services/dependency-injection/index.md), в связи с чем, в конструкторе появился новый аргумент.

## Изменена сигнатура конструктора LayoutBuilder и добавлено новое событие LayoutBuilderEvents::PREPARE_LAYOUT

- [#3143635](https://www.drupal.org/project/drupal/issues/3143635)

Класс `LayoutBuilder` теперь принимает `EventDispatcherInterface $event_dispatcher` вместо `LayoutTempstoreRepositoryInterface $layout_tempstore_repository`. Также удалено внедрение `messenger` [сервиса](../../../../9/services/index.md).

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

Также было добавлено новое событие `LayoutBuilderEvents::PREPARE_LAYOUT` которое вызывается в момент подготовки макета и передаёт `Drupal\layout_builder\Event\PrepareLayoutEvent` в качестве события. Это событие позволяет модулям взаимодействовать между собой в процессе `#pre_render` элемента.

## Заголовок ответа теперь содержит X-Drupal-Cache-Max-Age

- [#3089957](https://www.drupal.org/project/drupal/issues/3089957)

Теперь, при включении отладки для сайта, будет дополнительно добавляться заголовок `X-Drupal-Cache-Max-Age` к ответу. Данный заголовок будет содержать в качестве значения минимальное значение `max-age` среди всех рендер массивов отображаемых на странице.

## Настройки ядра теперь могут быть помечены как устаревшие

- [#3163226](https://www.drupal.org/project/drupal/issues/3163226)

Для того чтобы пометить [настройку ядро](../../../../9/settings-php/index.md) устаревшей необходимо добавить информацию в массив `Settings::$deprecatedSettings`.

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

Контрибным и собственным решениям рекомендуется делать подобным образом.

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

Функция `user_password()` помечена устаревшей, взамен добавлен новый сервис `passowrd_generator`.

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

В предыдущих версиях JSON:API возвращал объекты `ResourceResponce` на все запросы, при этом он реализует `CacheableResponseInterface`, даже для ответов которые не должны кешироваться. Это могло приводить к фатальным ошибкам при определённых условиях.

Сейчас JSON:API конкретизирует ответы что кешируются и не кешируются, в связи с этим представлен новый класс `CacheableResourceResponse`. Данный класс реализует `CacheableResponseInterface`. Таким образом `ResourceResponce` более не реализует `CacheableResponseInterface`.

`ResourceResponce` помечен для внутреннего использования и у вас не должно быть кода который может задеть данное изменение. Тем не менее, если по каким-то причинам ваш код полагается на данные классы, пожалуйста, произведите рефакторинг кода с использованием нового `CacheableResourceResponse` (это касается ответов `HEAD`, `OPTIONS` и `GET`).

## Добавлено новое исключение \Drupal\Core\Queue\DelayedRequeueException

- [#3116478](https://www.drupal.org/project/drupal/issues/3116478)

Новое исключение `DelayedRequeueException` позволяет обработчику очереди откладывать дальнейшую обработку элементов до следующего цикла или на определенный период пока не истечёт время блокировки элемента. Очереди, поддерживающие данную возможность должны реализовывать интерфейс `DelayableQueueInterface`.

Ранее было невозможно отложить обработку конкретного элемента очереди без выбрасывания исключения с последующим логированием или приостановкой всей очереди. Новый обработчик исключений работает «тихо» по умолчанию и позволяет отложить конкретный элемент для последующей обработки.

Для этого достаточно выбросить исключение `\Drupal\Core\Queue\DelayedRequeueException` в обработчике очереди.

```php
// Delay processing of this item until the next queue worker cycle.
throw new DelayedRequeueException();

// Delay processing of this item for 10 seconds (if supported by the queue).
throw new DelayedRequeueException(10);
```

Если очередь не поддерживает данную особенность, то исключение будет проигнорировано и элемент останется заблокированным на стандартный период.

`\Drupal\Core\Queue\DatabaseQueue` теперь реализует `DelayableQueueInterface`.

## Добавлена возможность помечать конфигурационные схемы устаревшими

- [#3129881](https://www.drupal.org/node/3129881)

Для конфигурационных схем добавлена поддержка объявления схемы устаревшей при помощи ключи `deprecated`. Для этого в структуру добавляется новый ключ `deprecated`, а в качестве значения — его описание.

Пример как пометить схему `complex_structure` устаревшей:

```yaml
complex_structure:
  type: mapping
  label: Complex
  deprecated: "The 'complex_structure' config schema is deprecated in drupal:9.1.0 and and is removed from drupal:10.0.0. Use the 'complex' config schema instead. See http://drupal.org/node/the-change-notice-nid."
  mapping:
    key:
      type: ...
    ...
```

## Хук media_oembed_iframe() теперь получает объект Resource

- [#3009003](https://www.drupal.org/project/drupal/issues/3009003)

Когда за рендер медиа отвечает источник oEmbed (например YouTube ролики), в целях безопасности медиа модуль рендерит данное содержимое внутри iframe.

Содержимое iframe генерируется на основе шаблона `media-oembed-iframe.html.twig` и соответствующего тем хука `media_oembed_iframe`. Начиная с Drupal 9.1 данный тем хук также получает связанный объект с oEmbed Resource, который может быть полезен для препроцесс функций и шаблонов, которые хотят внести изменения для iframe на основе данного ресурса.

Пример добавления `rel=0` параметра к пути до YouTube ролика в iframe:

```php
function mytheme_preprocess_media_oembed_iframe(array &$variables) {
  /** @var \Drupal\media\OEmbed\Resource $resource */
  $resource = $variables['resource'];
  if ($resource->getProvider()->getName() === 'YouTube') {
    // We are rendering a YouTube video, so modify the URL of the video so that it only shows related videos from the same channel.
    // The video's markup is only available as a string, so we need to use str_replace() to modify the URL.
    $variables['media'] = str_replace('?feature=oembed', '?feature=oembed&rel=0', (string) $variables['media']);
  }
}
```

## Множественные изменения в виджет подтверждения пароля

- [#3067523](https://www.drupal.org/project/drupal/issues/3067523)

В JavaScript виджет подтверждения пароля внесены множественны изменения.

### Drupal.evaluatePasswordStrength теперь возвращает объект

`Drupal.evaluatePasswordStrength` теперь возвращает объект содержащий в себе `messageTips` как замену для `message`. В нём содержатся все сообщения с проблемами вместо готовой HTML разметки.

### Новые JavaScript theme функции для оформления виджета

Добавлены три новые функции темизации:

- `Drupal.theme.passwordConfirmMessage`
- `Drupal.theme.passwordStrength`
- `Drupal.theme.passwordSuggestions`

При их помощи вы можете переопределить внешний вид виджета.

### Селекторы по CSS классу заменены на data-drupal-selector аттрибуты

Некоторые классы с префиксом `js-*` заменены на `data-drupal-selector` аттрибуты.

- `js-password-strength__indicator` заменён на `password-strength-indicator`.
- `js-password-strength__text` заменён на `password-strength-text`.
- `js-password-confirm-message` заменён на `password-confirm-message`.

Для вывода статуса об идентичности паролей раньше использовался пустой `<span>`, теперь данный элемент ищется по `data-drupal-selector="password-match-status-text"`.

**Было:**

```javascript
  Drupal.theme.passwordConfirmMessage = passwordSettings => {
    const confirmTextWrapper =
      '<span></span>';
    return `<div aria-live="polite" aria-atomic="true" class="password-confirm-message js-password-confirm-message" data-drupal-selector="password-confirm-message">${passwordSettings.confirmTitle} ${confirmTextWrapper}</div>`;
  };
```

**Стало:**

```javascript
  Drupal.theme.passwordConfirmMessage = passwordSettings => {
    const confirmTextWrapper =
      '<span data-drupal-selector="password-match-status-text"></span>';
    return `<div aria-live="polite" aria-atomic="true" class="password-confirm-message js-password-confirm-message" data-drupal-selector="password-confirm-message">${passwordSettings.confirmTitle} ${confirmTextWrapper}</div>`;
  };
```

## Включена ленивая загрузка картинок по умолчанию

- [#3167034](https://www.drupal.org/node/3167034)

Для изображений выводимых Drupal и которых заданы `width` и `height` аттрибуты включена ленивая загрузка. Требования наличия ширины и высоты обусловлено тем, что без данных аттрибутов ленивая загрузка приводит к проблемам [CLS](https://web.dev/cls/).

## Добавлен новый компонент — FrontMatter

- [#3064854](https://www.drupal.org/project/drupal/issues/3064854)

В ядро добавлен новый компонент `Drupal\Component\FrontMatter\FrontMatter`.

Данный компонент позволяет вам парсить [Front Matter](https://jekyllrb.com/docs/front-matter/) разметку из различных файлов.

Front Matter используется для того, чтобы добавить в исходный файл дополнительную статическую информацию.

Front Matter разметка должны быть самой первой в исходном файле, также она должна быть валидным YAML. Содержимое Front Matter задаётся между открывающей и закрывающей конструкцией, которая состоит из трёх тире подряд `---`.

### Пример

**source.md**

```markdown
---
important: true
---
My content
```

**example.php**

```php
use Drupal\Component\FrontMatter\FrontMatter;

$frontMatter = FrontMatter::create(file_get_contents('source.md'));
$data = $frontMatter->getData(); // ['important' => TRUE]
$content = $frontMatter->getContent(); // 'My content'
$line => $frontMatter->getLine(); // 4, line where content actually starts.
```

### Twig

[Сервис](../../../../9/services/index.md) `twig` был расширен для поддержки данной возможности в Twig шаблонах.

**Пример:**

```php
$metadata = \Drupal::service('twig')->getTemplateMetadata('/path/to/template.html.twig');
```

## PHPUnit обновлён до версии 9.

- [#3127141](https://www.drupal.org/project/drupal/issues/3127141)

Друпал ядро обновлено для использования PHPUnit 9. Установки на PHP 7.3 продолжат использовать PHPUnit 8.4 в целях [обратной совместимости](../../../../../backward-compatibility/index.md).

Большинство тестов не должно задеть данное изменение, тем не менее, разработчикам модулей следует обратить внимание на следующее:

- `::assertContains()` теперь производит строгое сравнение (`===`) что может привести к ошибкам. PHPUnit предоставляет более мягкий вариант данной проверки `::assertContainsEquals()`, благодаря которому, тесты продолжат работать на PHP 7.3.
- В ядро добавлен новый трейт `Drupal\Tests\PhpUnitCompatibilityTrait` для всех базовых классов. Тесты contrib модулей всегда должны расширять базовые классы тестов из ядра вместо прямого подключения трейта.

## Новая тема оформления Drupal — Olivero — добавлена в ядро

- [#3111409](https://www.drupal.org/project/drupal/issues/3111409)

Текущая тема оформления — Bartik, была частью релиза Drupal 7, который состоялся в январе 2011 года. Спустя 9 лет, данная тема успела попасть в релизы [Drupal 8](../../../../8/index.md) и [Drupal 9](../../../../9/index.md). Веб двигается вперёд, это было отлично для 2011 года, но не годится сейчас.

Начиная с данной версии, в ядро добавлена новая тема — [Olivero](../../../../olivero/index.md). На данный момент она находится в экспериментальном статусе и будет выключена по умолчанию, но со временем, она заменит Bartik.

Вы можете посмотреть как выглядит данная тема в живую на демо сайте: <http://lb.cm/olivero>

![Olivero demo](https://i.imgur.com/ehypMHV.png)

## Composer шаблоны Drupal больше не задают значение minimum-stability равным dev

- [#3135247](https://www.drupal.org/project/drupal/issues/3135247)

Начиная с [Drupal 8.8](../../../8/8.8.x/8.8.0/index.md) и до [Drupal 9.0](../../9.0.x/9.0.0/index.md), включительно, Composer шаблоны для Drupal ([drupal/recommended-project
](../../../../../composer/drupal/recommended-project/index.md) и [drupal/legacy-project](../../../../../composer/drupal/legacy-project/index.md)) указывали минимальную стабильную версию следующим образом:

```json
    "minimum-stability": "dev",
    "prefer-stable": true,
```

Это позволяет устанавливать пакеты в разработке вместе с шаблоном. Иногда данное поведение приводит к непредсказуемым результатам, например, обновление на новый нестабильный релиз, вместо того чтобы обновляться в пределах текущего стабильного релиза. Настройка Composer `prefer stable` должна рассматриваться как рекомендация, и не гарантирует, что будет выбран стабильный релиз.

Начиная с Drupal 9.1.0-alpha1, Composer шаблоны с Drupal будут указывать соответствующие уровни стабильности:

- Drupal 9.1.0-alpha1 будет указывать минимальную стабильность как "alpha"
- Drupal 9.1.0-beta1 будет указывать минимальную стабильность как "beta"
- Drupal 9.1.0-rc1 будет указывать минимальную стабильность как "RC"
- Drupal 9.1.0 будет указывать минимальную стабильность как "stable"

Данные изменения не окажут влияния на уже созданные ранее проекты управляемые Composer. Пользователи сайтов должны сами скорректировать это значение в `composer.json` файле, если это необходимо.

Используя минимальную стабильность в значении `stable` вы по-прежнему сможете загружать и устанавливать нестабильные зависимости, для этого достаточно запрашивать необходимые версии с «[флагом стабильности](https://getcomposer.org/doc/04-schema.md#package-links)», например:

```
composer require drupal/contrib:^1.0@beta
```

Данная команда позволит проекту `contrib` быть загруженным в бета-версии и выше. Тем не менее, если другой модуль имеет зависимость на него, то приведенный пример выше приведёт к ошибке. Данная ситуация также может быть решена похожим способом, запросив оба проекта с нужными метками:

```
composer require drupal/contrib:^1.0-beta1 outside/library:@alpha
```

## Добавлен новый класс StatementWrapper, Drupal\Core\Database\Connection::$statementClass помечен устаревшим

- [#3174662](https://www.drupal.org/project/drupal/issues/3174662)

В ядро добавлен новый класс `StatementWrapper`, который является обёрткой для встроенного в PHP класса `\PDOStatement`. `\PDOStatement` изменён в PHP 8, в связи с чем `\Drupal\Core\Database\StatementInterface` становится несовместимым с ним. Данная обертка позволит использовать `\Drupal\Core\Database\Statement` с PHP 8.

Драйвера БД из ядра для MySQL и Postgres теперь используют `\Drupal\Core\Database\StatementWrapper`.

Если вы не используете сторонние драйвера баз данных, никаких действий не требуется.

Сторонние и собственные драйвера баз данных должны установить значение `\Drupal\Core\Database\StatementWrapper` для свойства `\Drupal\Core\Database\Connection::$statementWrapperClass`, а также установить значение `NULL` для `\Drupal\Core\Database\Connection::$statementClass`.

Пример:

```php
  /**
   * {@inheritdoc}
   */
  protected $statementClass = NULL;

  /**
   * {@inheritdoc}
   */
  protected $statementWrapperClass = StatementWrapper::class;
```

Свойство `\Drupal\Core\Database\Connection::$statementClass` помечено устаревшим.

## Добавлен новый компонент — Reflection

- [#3156542](https://www.drupal.org/project/drupal/issues/3156542)

В PHP 8 был удалён `\ReflectionParameter::getClass()`, для того чтобы сохранить обратную совместимость добавлен новый утилитарный компонент `Reflection` и соответствующий метод `::getParameterClassName()`.

Для того чтобы ваш код был совместимо с PHP 8, обновите код с использованием нового компонента.

**Ранее:**

```php
function foo(Exception $a) { }

$functionReflection = new ReflectionFunction('foo');
$parameters = $functionReflection->getParameters();
$aParameter = $parameters[0];

echo $aParameter->getClass()->name;
```

**Теперь:**

```php
function foo(Exception $a) { }

$functionReflection = new ReflectionFunction('foo');
$parameters = $functionReflection->getParameters();
$aParameter = $parameters[0];

$param_class = \Drupal\Component\Utility\Reflection::getParameterClassName($aParameter);
echo $param_class;

// If you need the reflection class object.
$reflection = new \ReflectionClass($param_class);
echo $reflection->name;
```

## Drupal ядро теперь использует composer/semver 3 версии

- [#3128631](https://www.drupal.org/project/drupal/issues/3128631)

Drupal ядро теперь использует `composer/semver` 3 версии и все тесты используют Composer 2 для совместимости с PHP 8. Это не значит что Drupal теперь требует Composer 2 для управления зависимостями, релизы 8.8, 8.9, 9.0 и 9.1 могут по прежнему управляться Composer 1.

Если вы обновляетесь на Drupal 9.1+ при помощи Composer и видите следующее сообщение:

```
  Problem 1
    - Root composer.json requires my_project/my_package * -> satisfiable by my_project/my_package[...].
    - my_project/my_package dev-master requires composer/composer ^1 -> satisfiable by composer/composer[1.0.0-alpha1, ..., 1.10.x-dev].
```

Это означает что проблема с `my_project/my_package`, а не с Drupal ядром. Для решения проблемы, попробуйте обновить данные зависимости до актуальных версий. Если это не помогает, рекомендуется сообщить о проблеме разработчикам модуля (а не ядра).

## Для проверки наличия обновлений теперь требуется OpenSSL или дополнительная конфигурация

- [#1538118](https://www.drupal.org/project/drupal/issues/1538118)

Начиная с данного релиза, проверка обновлений для Drupal ядра и модулей будет производиться HTTPS протоколу. Для корректной проверки сертификата теперь потребуется [OpenSSL расширение для PHP](https://www.php.net/manual/en/openssl.installation.php). Если данное расширение отсутствует, то проверка обновления будет недоступна, а на странице статуса системы будет выводиться соответствующая ошибка и как её решить.

Для проверки обновлений по HTTP добавлена новая [настройка](../../../../9/settings-php/index.md) `update_fetch_with_http_fallback`. Это позволит работать в прежнем режиме и получать информацию об обновлениях. Включение данной настройки может привести к возможности для MITM атак на сайт.

В связи с введением новой настройки, классы `\Drupal\update\UpdateFetcher` и `\Drupal\update\Controller\UpdateController` ожидают в качестве аргумента конструктора сервис `settings`. Данный аргумент является опциональным для Drupal 9, но станет обязательным в [Drupal 10](../../../../10/index.md).

## Плагин источника данных миграций «variable» получил новую настройку

- [#3182891](https://www.drupal.org/project/drupal/issues/3182891)

Плагин источника данных Variable (`variable`) получил новую настройку `variables_no_row_if_missing`. Данная настройка позволяет перечислить все обязательные значения для данного плагина. Если хотя бы одного из значений отсутствует, то плагин не вернёт никаких значений.

### Примеры

При данной конфигурации, источник вернёт одну строку, даже если переменной `filter_fallback_format` не существует:

```yaml
source:
  plugin: variable
  variables:
    - filter_fallback_format
```

При данной конфигурации, источник данных вернёт одну строку если доступна переменная `file_public_path` или ноль значений, если переменная отсутствует:

```yaml
source:
  plugin: variable
  variables:
    - file_private_path
    - file_temporary_path
  variables_no_row_if_missing:
    - file_public_path
```

Значения настройки `variables_no_row_if_missing` всегда объединяется с `variables`. Все следующие примеры делают одно и тоже и валидны:

```yaml
source:
  plugin: variable
  variables:
    - book_child_type
    - book_block_mode
    - book_allowed_types
  variables_no_row_if_missing:
    - book_child_type
    - book_block_mode
    - book_allowed_types
```

```yaml
source:
  plugin: variable
  variables:
    - book_child_type
    - book_block_mode
  variables_no_row_if_missing:
    - book_allowed_types
```

```yaml
source:
  plugin: variable
  variables_no_row_if_missing:
    - book_child_type
    - book_block_mode
    - book_allowed_types
```

## Action

- [#3174573](https://www.drupal.org/project/drupal/issues/3174573) Исправлена грамматическая ошибка в документации `ActionUninstallTest`.

## Asset Library System

- [#3163500](https://www.drupal.org/project/drupal/issues/3163500) Сообщение об устаревшей библиотеке теперь также выводится при переопределении или расширении данной библиотеки темой.
- [#3095113](https://www.drupal.org/project/drupal/issues/3095113) Свойства `IE` и `!IE` для библиотек помечены устаревшими, так как его поддержка в IE была удалена в 2016 году для версий IE10+.

## Block

- [#3105976](https://www.drupal.org/project/drupal/issues/3105976) В `BlockViewBuilder::buildPreRenderableBlock()` для аргумента `$entity` добавлен тайпхинт `\Drupal\block\BlockInterface`.
- [#2151001](https://www.drupal.org/project/drupal/issues/2151001) Для административной страницы «Схема блоков» добавлен Tour.
- [#2890758](https://www.drupal.org/project/drupal/issues/2890758) Видимость блока по типу ноды теперь работает на маршрутах с предварительным просмотром и ревизии.
- [#2918149](https://www.drupal.org/project/drupal/issues/2918149) Теперь сообщение «This block is broken or missing…» будет отображаться только пользователям, у которых есть права на действия связанные с блоком.
- [#2404105](https://www.drupal.org/project/drupal/issues/2404105) Теперь при установке [дистрибутивов](../../../../9/distributions/index.md) блоки устанавливается только для конкретной темы, а не для всех активных.

## Book

- [#26552](https://www.drupal.org/project/drupal/issues/26552) Теперь редакторы могут редактировать или создавать неопубликованные страницы и подшивки.

## CKeditor

- [#3099662](https://www.drupal.org/project/drupal/issues/3099662) `ckeditor_stylesheets` теперь могут указывать путь относительно корня Drupal.
- [#2911527](https://www.drupal.org/project/drupal/issues/2911527) Добавлена возможность использовать `/` при добавлении собственных вариантов стилей.
- [#3171952](https://www.drupal.org/project/drupal/issues/3171952) CKEditor обновлён до 4.15.0.

## Claro

- [#3060697](https://www.drupal.org/project/drupal/issues/3060697) Claro теперь использует `#dropbutton_type` для вариантов `dropbutton` элемента, вместо классов.
- [#3154425](https://www.drupal.org/project/drupal/issues/3154425) Удалён комментарий «@todo Remove this after 8.6.x is out of support.» и код для него.
- [#3105575](https://www.drupal.org/project/drupal/issues/3105575) HTML классы перенесены из `claro_preprocess_textarea()` в шаблон.
- [#3164871](https://www.drupal.org/project/drupal/issues/3164871) Исправлены отступы у радио кнопок.
- [#3057772](https://www.drupal.org/project/drupal/issues/3057772) Улучшены иконки для элемента `details`.
- [#3171727](https://www.drupal.org/project/drupal/issues/3171727) Разделитель для хлебных крошек теперь более контрастный.
- [#3066006](https://www.drupal.org/project/drupal/issues/3066006) Оформление Views UI приведено в соответствие дизайну.
- [#3085212](https://www.drupal.org/project/drupal/issues/3085212) Новое оформление страницы «Сайт находится в режиме обслуживания».
- [#3072772](https://www.drupal.org/project/drupal/issues/3072772) Новое оформление страницы расширений.
- [#3166068](https://www.drupal.org/project/drupal/issues/3166068) Исправлен AJAX индикатор загрузки значений для автодополнения в инлайн формах.
- [#3158854](https://www.drupal.org/project/drupal/issues/3158854) Для форм создания и редактирования нод, основные элементы теперь центруются если ширина превышает 1200px.
- [#3070493](https://www.drupal.org/project/drupal/issues/3070493) Предоставлен механизм и начальная реализация оформления тулбара под стиль Claro.
- [#3061042](https://www.drupal.org/project/drupal/issues/3061042) Улучшено оформление страницы состояния системы.
- [#3062751](https://www.drupal.org/project/drupal/issues/3062751) Улучшено оформления для Media и Media Library элементов.
- [#3179768](https://www.drupal.org/project/drupal/issues/3179768) Улучшено оформление каретки для `<details>` элемента.
- [#3183425](https://www.drupal.org/project/drupal/issues/3183425) Добавлено решение для бага Google Chrome 87 при котором fieldset отрисовывался с артефактами.

## Comment

- [#2984243](https://www.drupal.org/project/drupal/issues/2984243) Кнопка фильтрации в представлении для вывода комментариев теперь содержит значение «Filter» вместо «Apply».
- [#3163685](https://www.drupal.org/project/drupal/issues/3163685) Удалена неиспользуемая переменная `$block` в `CommentBlockTest`.
- [#3163686](https://www.drupal.org/project/drupal/issues/3163686) Удалена неиспользуемая переменная `$comment` в `CommentLinksAlterTest`.
- [#3163425](https://www.drupal.org/project/drupal/issues/3163425) Удалена неиспользуемая переменная `$fields` в `CommentViewsData`.
- [#3059719](https://www.drupal.org/project/drupal/issues/3059719) Использование `#markup` заменено на `#context`.

## Composer

- [#3156558](https://www.drupal.org/project/drupal/issues/3156558) Обновлены зависимости.
- [#3133903](https://www.drupal.org/project/drupal/issues/3133903) Добавлены проверка, что все пакеты из `composer.lock` файла ядра имеются и имеют конкретные версии.
- [#3121847](https://www.drupal.org/project/drupal/issues/3121847) В шаблоны проектов [drupal/recommended-project](drupal-recommended-project.md) и [drupal/legacy-project](drupal-legacy-project.md) теперь добавляется новый путь установки `drupal-custom-profile` (`profiles/custom/{$name}/`).
- [#3164349](https://www.drupal.org/project/drupal/issues/3164349) `symfony/var-dumper` теперь указан как dev зависимость в корневом composer.json Drupal.
- [#3157296](https://www.drupal.org/project/drupal/issues/3157296) Обновлены зависимости ядра.
- [#3168514](https://www.drupal.org/project/drupal/issues/3168514) Удалены неиспользуемые полифилы.
- [#3176504](https://www.drupal.org/project/drupal/issues/3176504) Обновлены зависимости ядра.
- [#3178046](https://www.drupal.org/project/drupal/issues/3178046) Обновлены зависимости ядра.
- [#3179284](https://www.drupal.org/project/drupal/issues/3179284) Обновлены зависимости ядра.
- [#3180092](https://www.drupal.org/project/drupal/issues/3180092) Зависимость `fabpot/goutte` обновлена до версии 3.3.1.
- [#3151118](https://www.drupal.org/project/drupal/issues/3151118) Файл `bootstrap.inc` добавлен в автозагрузчик Composer. Это позволит использовать функции,  например `t()`, в тестах без проблем.
- [#3181240](https://www.drupal.org/project/drupal/issues/3181240) Зависимость `typo3/phar-stream-wrapper` обновлена до версии 3.1.6.
- [#3182959](https://www.drupal.org/project/drupal/issues/3182959) Drupal пакеты теперь явно зависят от текущей минорной версии ядра, вместо мажорной. Например `^9.1` вместо `^9`. Это позволяет исправить неполадку, когда предрелизные версии при обновлении даунгрейдились до предыдущей минорной версии: 9.1.0-alpha1 > 9.0.7.
- [#3183825](https://www.drupal.org/project/drupal/issues/3183825) При генерации фикстуры с вендорами теперь используется абсолютный путь, вместо относительного.
- [#3098281](https://www.drupal.org/project/drupal/issues/3098281) Теперь в `composer.json` для ядра явно указана версия PHP в разделе `platform`.
- [#3180207](https://www.drupal.org/project/drupal/issues/3180207) Для совместимости с PHP 8 обновлены следующие зависимости: `laminas/laminas-diactoros`, `laminas/laminas-feed`, `laminas/laminas-escaper`.

## Contact

- [#3150227](https://www.drupal.org/project/drupal/issues/3150227) Удалены неиспользуемые переменные `$contact_form` и `$recipients_str`.

## Content Moderation

- [#3044292](https://www.drupal.org/project/drupal/issues/3044292) (откачено) Добавлен новый метод `::isModeratedEntity` для хендлеров moderation сущностей.
- [#3164498](https://www.drupal.org/project/drupal/issues/3164498) Удалена неиспользуемая переменная `$entity_type_ids` в `content_moderation.module`.
- [#3155022](https://www.drupal.org/project/drupal/issues/3155022) Изменена сигнатура `EntityModerationForm::__construct()`. Параметр `$time` теперь имеет более слабые требования и вместо `Time` объекта ожидает экземпляр `TimeInterface`.
- [#3167811](https://www.drupal.org/project/drupal/issues/3167811) Улучшена документация для конструктора `EntityModerationForm`.

## Content Translation

- [#2972308](https://www.drupal.org/project/drupal/issues/2972308) Добавлено новое разрешение `translate editable entities` позволяющее переводить сущности, которые пользователь может редактировать.
- [#2796399](https://www.drupal.org/project/drupal/issues/2796399) Улучшены hreflang метатеги для сущности что используется в качестве главной страницы.
- [#3178338](https://www.drupal.org/project/drupal/issues/3178338) Исправлены стандарты кодирования для `d7_menu_links_localized.yml`.

## CSS

- [#3170864](https://www.drupal.org/project/drupal/issues/3170864) `postcss-custom-properties` заменён `postcss-preset-env`.

## Database System

- [#2278971](https://www.drupal.org/node/2278971) `Connection::supportsTransactions` помечен устаревшим. Таким образом [настройка](../../../../9/settings-php/index.md) подключения к БД `transactions` также становится устаревшей.
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
- [#3120892](https://www.drupal.org/project/drupal/issues/3120892) Для драйвера SQL Lite добавлена поддержка функции `LEAST()`.
- [#3174848](https://www.drupal.org/project/drupal/issues/3174848) Исправлена опечатка в сообщении о [депрекации](../../../../../deprecation/index.md) метода `Connection::prepare`.
- [#2736777](https://www.drupal.org/project/drupal/issues/2736777) Теперь при работе с транзакциями на PHP 8 не будет вызываться исключение.
- [#3089902](https://www.drupal.org/project/drupal/issues/3089902) При создании БД Drupal больше не задаёт `NO_AUTO_CREATE_USER`.

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
- [#2918290](https://www.drupal.org/project/drupal/issues/2918290) Исправлен некорректно указанный возвращаемый типа для `FieldStorageConfig::loadByName`.
- [#3177545](https://www.drupal.org/project/drupal/issues/3177545) Внесены улучшения в `\Drupal\field\Entity\FieldStorageConfig::getCardinality()` для совместимости с PHP 8.
- [#3158651](https://www.drupal.org/project/drupal/issues/3158651) Настройка сортировки теперь видна только если выбрано хотя бы одно поле для сортировки.

## File

- [#3070902](https://www.drupal.org/project/drupal/issues/3070902) Для исключения вызываемого в `prepareDestination()` улучшено описание лога.
- [#2991219](https://www.drupal.org/project/drupal/issues/2991219) `template_preprocess_file_link()` больше не добавляет `length` параметр после MIME в `type` аттрибуте.
- [#3157370](https://www.drupal.org/project/drupal/issues/3157370) Исправлен комментарий ссылающийся на `file_directory_temp()`.

## Filter

- [#3151101](https://www.drupal.org/project/drupal/issues/3151101) Употребление слов «whitelist» и «blacklist» в Filter модуле заменено на более подходящие.

## Forum

- [#3067622](https://www.drupal.org/project/drupal/issues/3067622) Справка из `hook_help()` конвертирована в Help Topics.
- [#2828680](https://www.drupal.org/project/drupal/issues/2828680) Добавлена отсутствующая обёртка `<ul>` для действий на форуме. 

## JSON:API

- [#3165794](https://www.drupal.org/project/drupal/issues/3165794) Удалена неиспользуемая переменная `$account_bundle` в `ResourceTestBase`.
- [#3093757](https://www.drupal.org/project/drupal/issues/3093757) Убраны вызовы `testRelated()` из тестов так как проблемы (issues) решены.
- [#3112229](https://www.drupal.org/project/drupal/issues/3112229) JSON:API больше не упрощает результаты для типов полей, которые возвращают `NULL` или имя основного свойства не соответствует тому, что возвращает значение.
- [#2996114](https://www.drupal.org/project/drupal/issues/2996114) Улучшено сообщение об ошибке если ресурс был изменён.
- [#3175884](https://www.drupal.org/project/drupal/issues/3175884) Исправлена теоретическая ошибка которая могла привести к одинаковым ссылкам в ответе при разных метаданных для этих самых ссылок.

## Help Topic

- [#3087879](https://www.drupal.org/project/drupal/issues/3087879) Для поиска по Help Topic теперь используется административная [тема оформления](../../../../9/themes/index.md).
- [#3095740](https://www.drupal.org/project/drupal/issues/3095740) Справка для `menu_link_content` и `menu_ui` модулей конвертирована в Help Topic.
- [#3164965](https://www.drupal.org/project/drupal/issues/3164965) Удалена неиспользуемая переменная `$source` в `HelpTopicTwigLoaderTest`.
- [#3166763](https://www.drupal.org/project/drupal/issues/3166763) Исправлены двойные пробелы в `help.help_topic_search.html.twig`.
- [#3047703](https://www.drupal.org/project/drupal/issues/3047703) Справки модулей `basic_auth`, `hal`, `jsonapi`, `rdf`, `rest` и `serialization` конвертированы в Help Topic.
- [#3085972](https://www.drupal.org/project/drupal/issues/3085972) `Drupal\help_topics\FrontMatter` заменён на `Drupal\Component\Utility\FrontMatter`.

## Image

- [#3153009](https://www.drupal.org/project/drupal/issues/3153009) Добавлен новый [стиль изображения](../../../../9/image/image-styles/index.md) устанавливаемый с модулем — «Wide (1090)». Он будет использоваться как Hero стиль в будущей теме Olivero.
- [#3097797](https://www.drupal.org/project/drupal/issues/3097797) Улучшена документация для функции `image_filter_keyword()`.
- [#3165350](https://www.drupal.org/project/drupal/issues/3165350) Удалена неиспользуемая переменная `$key` в `MigrateImageCacheTest`.
- [#2630230](https://www.drupal.org/project/drupal/issues/2630230) Исправлена неполадка из-за которой мог не генерироваться стиль изображения из корня публичной файловой директории при конвертации типов.
- [#3174913](https://www.drupal.org/project/drupal/issues/3174913) Исправлена ошибка в конструкторе `ImageStyleDownloadController` для соответствия `StreamWrapperManagerInterface`.

## Install System

- [#3157895](https://www.drupal.org/project/drupal/issues/3157895) Обновление состояния `install_time` перенесено в `installed_finished()`.
- [#3086307](https://www.drupal.org/project/drupal/issues/3086307) Производительность установки увеличена примерно на ~20%, путем сброса кеша маршрутов после установки всех модулей, а не после каждого.

## JavaScript

- [#3145930](https://www.drupal.org/project/drupal/issues/3145930) Размер «липкого» заголовка теперь пересчитывается после сворачивания и разворачивания тулбара.
- [#3096516](https://www.drupal.org/project/drupal/issues/3096516) В `domready` внесены улучшения, которые решают проблему с [race condition](https://ru.wikipedia.org/wiki/%D0%A1%D0%BE%D1%81%D1%82%D0%BE%D1%8F%D0%BD%D0%B8%D0%B5_%D0%B3%D0%BE%D0%BD%D0%BA%D0%B8).
- [#3152473](https://www.drupal.org/project/drupal/issues/3152473) Улучшена работа обратного вызова domready.
- [#3078501](https://www.drupal.org/project/drupal/issues/3078501) Функция `Drupal.AjaxCommands.prototype.alert` теперь вызывает `window.alert` с одним параметром, так как второй ни на что не влияет.
- [#1936708](https://www.drupal.org/project/drupal/issues/1936708) Улучшено отображение вертикальных вкладок. Теперь они корректно отображают и обновляют сводку по выбранным значениям.
- [#3143465](https://www.drupal.org/project/drupal/issues/3143465) 😑 Добавлен полифил `NodeList.forEach` для совместимости с IE11. 
- [#3174884](https://www.drupal.org/project/drupal/issues/3174884) Обновлены зависимости Yarn.
- [#3181057](https://www.drupal.org/project/drupal/issues/3181057) Underscore.js обновлён до 1.11.0.
- [#3181059](https://www.drupal.org/project/drupal/issues/3181059) Popper.js обновлён до 2.5.4.

## Install system

- [#2691389](https://www.drupal.org/project/drupal/issues/2691389) Строка «Save and continue» в `InstallerTestBase::setUpLanguage` больше не является переводимой так как на данном этапе только английский язык.

## Layout Builder

- [#3053887](https://www.drupal.org/project/drupal/issues/3053887) В код добавлена документация почему блоки требуют создание новой ревизии при изменении.
- [#3069578](https://www.drupal.org/project/drupal/issues/3069578) Исправлена неполадка из-за которой [псевдо-поля](../../../../9/hooks/extra-fields/index.md) не рендерились.

## Locale

- [#2925318](https://www.drupal.org/project/drupal/issues/2925318) Для таблицы `locales_location` удалён индекс `sid` так как он покрыт в `string_type`.
- [#3167600](https://www.drupal.org/project/drupal/issues/3167600) Удалена неиспользуемая переменная `$config` в `locale.bulk.inc`.
- [#3167599](https://www.drupal.org/project/drupal/issues/3167599) Удалена неиспользуемая переменная `$frequency` в `locale.module`.
- [#3168261](https://www.drupal.org/project/drupal/issues/3168261) Удалена неиспользуемая переменная `$language_list` в `locale.module`.
- [#3179258](https://www.drupal.org/project/drupal/issues/3179258) Теперь, при поиске строки для перевода в интерфейсе, удаляются начальные и конечные пробелы в поисковой фразе.

## Media

- [#3142818](https://www.drupal.org/project/drupal/issues/3142818) Из ссылок удалён аттрибут `target=_blank`.
- [#3159793](https://www.drupal.org/project/drupal/issues/3159793) Исправлена опечатка в форме настройки Media Library.
- [#3146492](https://www.drupal.org/project/drupal/issues/3146492) Удалены неиспользуемые переменные в модуле.
- [#3169866](https://www.drupal.org/project/drupal/issues/3169866) Удалены неиспользуемые переменные `$target` и `$button` в `CKEditorIntegrationTest`.

## Mail System

- [#3094783](https://www.drupal.org/project/drupal/issues/3094783) Для AJAX запросов отключение рефокусировки перенесено из `data-disable-refocus` кнопки отправки, непосредственно в `#ajax` опцию `disable-refocus`.

## Menu UI

- [#3158562](https://www.drupal.org/project/drupal/issues/3158562) Теперь в интерфейсе всегда ссылка меню упоминается как «menu link», вместо «menu item».
- [#3153394](https://www.drupal.org/project/drupal/issues/3153394) Добавлена документация что меню поддерживает маршрут типа `route:<button>`.

## Migration System

- [#3024682](https://www.drupal.org/node/3024682) На странице со списком миграций теперь показываются человеко-понятные названия, вместо машинных.
- [#3143719](https://www.drupal.org/project/drupal/issues/3143719) В `MigrateUpgradeTestBase` добавлен новый метод `getCredentials()`.
- [#2993367](https://www.drupal.org/project/drupal/issues/2993367) Добавлена миграция из Drupal 7 Picture (contrib модуль) в Responsive Image.
- [#3133139](https://www.drupal.org/project/drupal/issues/3133139) Удалена `is_array` проверка в `getProcessPlugins`.
- [#3134300](https://www.drupal.org/project/drupal/issues/3134300) Упрощена разметка и код в `ReviewForm::buildForm()`.
- [#3154398](https://www.drupal.org/project/drupal/issues/3154398) Миграции теперь могут указывать требуемые плагины для своей работы: при помощи нового метода `getRequirements()`.
- [#3110669](https://www.drupal.org/project/drupal/issues/3110669) Добавлена поддержка миграции мультиязычных меню из Drupal 7.
- [#2845485](https://www.drupal.org/project/drupal/issues/2845485) Улучшена документация для плагина `MenuLinkParent`.
- [#3160323](https://www.drupal.org/project/drupal/issues/3160323) Название переменных в исключениях `Row` теперь обёрнуты в одинарные кавычки.
- [#3143717](https://www.drupal.org/project/drupal/issues/3143717) Добавлены новые хелперы `MigrateUpgradeTestBase::assertIdConflictForm()` и `MigrateUpgradeTestBase::assertReviewForm()`.
- [#3164120](https://www.drupal.org/project/drupal/issues/3164120) Исправлен пример кода в документации плагина `MenuLinkParent`.
- [#2447727](https://www.drupal.org/project/drupal/issues/2447727) Добавлен абстрактный `ReferenceBase` для миграции связующих полей.
- [#3112249](https://www.drupal.org/project/drupal/issues/3112249) Добавлена новая миграция `d7_menu_translation` для миграции переводов меню из Drupal 7.
- [#3164652](https://www.drupal.org/project/drupal/issues/3164652) Для плагина обработчика `Substr` включено исключение cspell для игнорирования `skÅ‚odowska`.
- [#3158277](https://www.drupal.org/project/drupal/issues/3158277) Удалена неиспользуемая переменная в `EntityLinkTest`.
- [#3172592](https://www.drupal.org/project/drupal/issues/3172592) Удалена неиспользуемая переменная `$field_type` в `EntityContentBase`.
- [#3172332](https://www.drupal.org/project/drupal/issues/3172332) Удалена неиспользуемая переменная `$process_plugin_manager` в `MigrationLookupTest`.
- [#3170972](https://www.drupal.org/project/drupal/issues/3170972) Удалена неиспользуемая переменная `$iterator` в `MigrateExecutableTest`.
- [#3010951](https://www.drupal.org/project/drupal/issues/3010951) Исправлена неполадка из-за которой метод `::createInstancesByTag` менеджера миграций создавал экземпляры для всех найденных плагинов если нет плагинов с метками.
- [#2960170](https://www.drupal.org/project/drupal/issues/2960170) Для плагина обработчика `Flatter` добавлена валидация входных данных.
- [#3152789](https://www.drupal.org/project/drupal/issues/3152789) Для плагина источника `variable` добавлена новая настройка `variables_required`.
- [#3171755](https://www.drupal.org/project/drupal/issues/3171755) Удалена неиспользуемая переменная `$row` в `RowTest`.
- [#3143676](https://www.drupal.org/project/drupal/issues/3143676) Исправлена неполадка в миграции `d7_term_localized_translation` из-за недостаточного количества проверок.
- [#3143720](https://www.drupal.org/project/drupal/issues/3143720) Добавлен новый тест `CredentialFormTest`.
- [#3008028](https://www.drupal.org/project/drupal/issues/3008028) Добавлены миграции ссылок меню i18n из Drupal 7.
- [#3172116](https://www.drupal.org/project/drupal/issues/3172116) Удалена неиспользуемая переменная `$parent_link_path`.

## Node System

- [#2830504](https://www.drupal.org/project/drupal/issues/2830504) Исправлена неполадка из-за которой `Drupal\node\Plugin\Action\AssignOwnerNode` позволяла выбрать гостя в качестве владельца ноды.
- [#3165950](https://www.drupal.org/project/drupal/issues/3165950) Из `NodeTypeForm` удалено упоминание что нижние подчёркивания будут конвертированы в дефисы, так как для путей форм сущностей уже используются нижние подчёркивания.
- [#2586013](https://www.drupal.org/project/drupal/issues/2586013) Функция `node_views_analyze()` перенесена из файла `node.views.inc` в `node.views_execution.inc`.

## QuickEdit

- [#3174574](https://www.drupal.org/project/drupal/issues/3174574) Исправлена опечатка в документации к `QuickEditLoadingTest`. 

## Help Topics

- [#3047723](https://www.drupal.org/project/drupal/issues/3047723) Документация модулей views, views_ui конвертирована в Help Topics.
- [#3067614](https://www.drupal.org/project/drupal/issues/3067614) Документация модулей filter, ckeditor, editor конвертирована в Help Topics.

## Olivero

- [#3177345](https://www.drupal.org/project/drupal/issues/3177345) Улучшен контраст текста для блока «Powered by Drupal».
- [#3161010](https://www.drupal.org/project/drupal/issues/3161010) Исправлена синтаксическая ошибка в `layout-builder-twocol-section.css`.
- [#3173905](https://www.drupal.org/project/drupal/issues/3173905) Исправлены ошибки в JavaScript связанные с некорректным использованием `classList.remove()`.
- [#3157308](https://www.drupal.org/project/drupal/issues/3157308) Стили неопубликованного материала теперь объявлены непосредственно в теме и больше не подключается библиотека `classy/node`.
- [#3176908](https://www.drupal.org/project/drupal/issues/3176908) Улучшена документация для `fieldset.html.twig`.
- [#3179157](https://www.drupal.org/project/drupal/issues/3179157) Исправлено отображение уменьшенных элементов форм в RTL формате.
- [#3178806](https://www.drupal.org/project/drupal/issues/3178806) Класс `text-content` удалён из `node--teaser.html.twig`.
- [#3173018](https://www.drupal.org/project/drupal/issues/3173018) Классы для форм элементов скорректированы для соответствия БЭМ.
- [#3177318](https://www.drupal.org/project/drupal/issues/3177318) В `MAINTAINERS.txt` добавлена информация о мейнтейнерах Olivero.
- [#3176910](https://www.drupal.org/project/drupal/issues/3176910) Для `preload.twig` добавлена документация.
- [#3176919](https://www.drupal.org/project/drupal/issues/3176919) Добавлена документация для шаблона `node--article--full.html.twig`.
- [#3179150](https://www.drupal.org/project/drupal/issues/3179150) Теперь используется шрифт Metropolis `h1` внутри контента.
- [#3176913](https://www.drupal.org/project/drupal/issues/3176913) Добавлена документация для шаблона `block--system-powered-by-block.html.twig`.
- [#3177231](https://www.drupal.org/project/drupal/issues/3177231) Улучшена проверка на наличие заголовка страницы.
- [#3174088](https://www.drupal.org/project/drupal/issues/3174088) Удалён комментарий с `@todo` для scripts.es6.js.
- [#3176912](https://www.drupal.org/project/drupal/issues/3176912) Добавлена недостающая документация в шаблоне `block--secondary-menu--plugin-id--search-form-block.html.twig`.
- [#3176914](https://www.drupal.org/project/drupal/issues/3176914) Добавлен класс для «неопубликованных» мультимедиа-сущностей в `media.html.twig`.
- [#3180726](https://www.drupal.org/project/drupal/issues/3180726) Добавлен отсутствующий знак точки в документации к `html.html.twig`.
- [#3176906](https://www.drupal.org/project/drupal/issues/3176906) Исправлена документация во множестве различных шаблонов.
- [#3176911](https://www.drupal.org/project/drupal/issues/3176911) Добавлена документация для переменной `noscript_styles` в `html.html.twig`.
- [#3180280](https://www.drupal.org/project/drupal/issues/3180280) Уменьшен трекинг для навигационного меню.
- [#3180756](https://www.drupal.org/project/drupal/issues/3180756) Исправлена высота для встроенных аудио мультимедиа.

## Plugin System

- [#2273381](https://www.drupal.org/project/drupal/issues/2273381) `ContextAwarePluginBase` заменён трейтом `ContextAwarePluginTrait` и помечен устаревшим.

## PostgreSQL драйвер

- [#3129560](https://www.drupal.org/project/drupal/issues/3129560) Удалена реализация `Upsert`.
- [#3154669](https://www.drupal.org/project/drupal/issues/3154669) Исправлены ошибки и опечатки для комментариев.

## RDF

- [#3110972](https://www.drupal.org/project/drupal/issues/3110972) Библиотека `easyrdf/easyrdf` обновлена до версии 1.0.0.

## Render System

- [#3172410](https://www.drupal.org/project/drupal/issues/3172410) Класс `HtmlResponse` подкорректирован для совместимости с Symfony 5.

## REST

- [#3152848](https://www.drupal.org/project/drupal/issues/3152848) Код связанный с `bc_entity_resource_permissions` настройкой удалён, так как она больше не используется.
- [#3169578](https://www.drupal.org/project/drupal/issues/3169578) Удалён неиспользуемый код.
- [#3173076](https://www.drupal.org/project/drupal/issues/3173076) Удалена неиспользуемая переменная `$parseable_valid_request_body_2` в `EntityResourceTestBase`.
- [#3172846](https://www.drupal.org/project/drupal/issues/3172846) Удалена неиспользуемая переменная `$supported_formats` в `ResourceRoutes`.

## Routing System

- [#3158708](https://www.drupal.org/project/drupal/issues/3158708) Возвращено поведение, что `RouteProvider::getAllRoutes()` возвращает `iterable` результат, которое было изменено в [#2917331](https://www.drupal.org/project/drupal/issues/2917331).
- [#3173958](https://www.drupal.org/project/drupal/issues/3173958) В `EntityResolverManager::getContro#llerClass` добавлена проверка что `$controller` не `NULL`.
- [#3074201](https://www.drupal.org/project/drupal/issues/3074201) Методы `RouteCompiler::getDefaults()`, `RouteCompiler::getRequirements()` и `RouteCompiler::getRequirements()` признаны устаревшими.

## Search

- [#3086794](https://www.drupal.org/project/drupal/issues/3086794) Плагины результатов поиска теперь могут указывать, какую тему использовать для отрисовки страниц.
- [#3086795](https://www.drupal.org/project/drupal/issues/3086795) «Search help» на странице поиска заменён на «About searching» для избежания двусмысленности.
- [#3155221](https://www.drupal.org/project/drupal/issues/3155221) Удален устаревший «@todo».
- [#3075703](https://www.drupal.org/project/drupal/issues/3075703) Функции для обработки поискового запроса `search_index_split()`, `search_simplify()` и `search_expand_cjk()` перенесены в [сервис](../../../../9/services/index.md) `search.text_processor`.
- [#3173595](https://www.drupal.org/project/drupal/issues/3173595) Удалена неиспользуемая переменная `$email` в `SearchBlockTest`.
- [#3177377](https://www.drupal.org/project/drupal/issues/3177377) Удалены неиспользуемые переменные `$charcodes` и `$node2`.

## Serialization

- [#3135304](https://www.drupal.org/project/drupal/issues/3135304) Удалён слой обратной совместимости с Symfony 3 из `JsonEncoder`.

## Seven

- [#3054196](https://www.drupal.org/node/3054196) Исправлена проблема с белым фоном у кнопки в таблице.

## Simpletest

- [#3112432](https://www.drupal.org/project/drupal/issues/3112432) Добавлена реализация `hook_requirements()` которая будет постоянно блокировать включение данного модуля на новых сайтах.
- [#3178037](https://www.drupal.org/project/drupal/issues/3178037) Исправлена опечатка «is has» в `SimpleTest`.

## System

- [#3077938](https://www.drupal.org/project/drupal/issues/3077938) Добавлена функция `tableDragHandle` для `Drupal.theme`. Теперь темы могут менять разметку управления сортировкой таблицы.
- [#3174378](https://www.drupal.org/project/drupal/issues/3174378) Удалена неиспользуемая переменная `$filesystem_config` в `system.install` и `UpdateScriptTest`.
- [#3156887](https://www.drupal.org/project/drupal/issues/3156887) Для совместимости с PHP 8 `\Drupal\system\Plugin\ImageToolkit\GDToolkit` теперь поддерживает объекты `\GdImage`.

## Taxonomy

- [#3122511](https://www.drupal.org/node/3122511) На странице редактирования добавлен пункт удаления во вкладки.
- [#3151953](https://www.drupal.org/project/drupal/issues/3151953) В тесте `TermTranslationUITest` использование прямого запроса заменено на Entity Query.

## Update

- [#2303323](https://www.drupal.org/project/drupal/issues/2303323) `update_delete_file_if_stale()` теперь возвращает логическое значение.

## User

- [#3082006](https://www.drupal.org/node/3082006) Поле пароля больше нельзя использовать в Views для вывода. Ранее он не показывал ничего, сейчас отключена возможность выбора данного значения.
- [#3150070](https://www.drupal.org/project/drupal/issues/3150070) Видимость свойств в новых тестах изменена с `public` на `protected`.
- [#2847808](https://www.drupal.org/project/drupal/issues/2847808) Метка для прав доступа `administer permissions` изменена на «Administer roles and permissions».
- [#2193803](https://www.drupal.org/project/drupal/issues/2193803) Переход по невалидной ссылке выхода из аккаунта больше не выдаёт 403, а редиректит на главную страницу сайта.

## Views

- [#3139353](https://www.drupal.org/project/drupal/issues/3139353) Добавлен новый публичный метод `Drupal\views\Plugin\views\query\Sql::getConnection()`.
- [#3150490](https://www.drupal.org/project/drupal/issues/3150490) Улучшено именование переменных в `Drupal\views\ViewExecutableFactory::get`.
- [#2838555](https://www.drupal.org/project/drupal/issues/2838555) Views больше не позволит добавлять связи на данные у которых нет базовой таблицы для join (например, конфигурационные сущности).
- [#2780869](https://www.drupal.org/project/drupal/issues/2780869) Исправлена неполадка, при которой невозможно было сохранить представление, если в значении опции для фильтра была точка.
- [#2625136](https://www.drupal.org/project/drupal/issues/2625136) Раскрытые фильтры для `numeric` и `date` полей теперь имеют обертку, для того чтобы поля были на одном уровне.
- [#2846485](https://www.drupal.org/project/drupal/issues/2846485) Улучшена производительность при рендере множественного поля, где каждый элемент поля создаёт свою строку с выводом.
- [#3013216](https://www.drupal.org/project/drupal/issues/3013216) Упрощены селекторы в `views-admin.es6.js`.
- [#2336569](https://www.drupal.org/project/drupal/issues/2336569) Улучшено добавление `<span>` в `#field_prefix` и `#field_suffix`.
- [#3175081](https://www.drupal.org/project/drupal/issues/3175081) Удалена неиспользуемая переменная `$exposed` в `Equality`.
- [#3175564](https://www.drupal.org/project/drupal/issues/3175564) Удалена неиспользуемая переменная `$renderer` в `AreaOrderTest`.
- [#3175571](https://www.drupal.org/project/drupal/issues/3175571) Удалена неиспользуемая переменная `$nodes` в `SortTranslationTest`.
- [#3175665](https://www.drupal.org/project/drupal/issues/3175665) Удалена неиспользуемая переменная `$view` в `FilterTest`.
- [#3177546](https://www.drupal.org/project/drupal/issues/3177546) Исправлено неправильное использование `in_array()` в `\Drupal\views\Plugin\views\PluginBase::listLanguages()` что приводило к выводу дополнительных языков.
- [#3177590](https://www.drupal.org/project/drupal/issues/3177590) Внесены улучшения для `ViewsFormBase::getForm()` для совместимости с PHP 8.
- [#3175666](https://www.drupal.org/project/drupal/issues/3175666) Удалена неиспользуемая переменная `$admin_user` в `FieldEntityOperationsTest`.

## Workspaces

- [#3174418](https://www.drupal.org/project/drupal/issues/3174418) Удалены неиспользуемые переменные `$revision_metadata_keys` и `$active_workspace_id`.

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
- [#3082859](https://www.drupal.org/project/drupal/issues/3082859) `AssertMailTrait::assertMailPattern()` теперь преобразует значение `$regex_found` в логический тип.
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
- [#3139442](https://www.drupal.org/project/drupal/issues/3139442) Использование устаревшего `AssertLegacyTrait::constructFieldXpath()` заменено на `$this->getSession()->getPage()->findField()`.
- [#3101247](https://www.drupal.org/project/drupal/issues/3101247) Использование устаревшего `AssertHelperTrait::castSafeStrings()` заменено на `$this->assertEquals()`.
- [#3129002](https://www.drupal.org/project/drupal/issues/3129002) Использование устаревшего `AssertLegacyTrait::assert()` заменено на `$this->assertTrue()`.
- [#3168107](https://www.drupal.org/project/drupal/issues/3168107) Зависимость `symfony/phpunit-bridge` обновлена до версии `^5.1.4`.
- [#3165588](https://www.drupal.org/project/drupal/issues/3165588) Добавлена проверка что свойство `$module` является `protected.
- [#3139405](https://www.drupal.org/project/drupal/issues/3139405) Использование устаревших `AssertLegacyTrait::assertUniqueText()` и `AssertLegacyTrait::assertNoUniqueText()` заменено на `$this->getSession()->getPage()->getText()`.
- [#3139419](https://www.drupal.org/project/drupal/issues/3139419) Использование устаревшего `AssertLegacyTrait::assertUrl()` заменено на `$this->assertSession()->addressEquals()`.
- [#3139418](https://www.drupal.org/project/drupal/issues/3139418) Использование устаревших `AssertLegacyTrait::assertLinkByHref` и `AssertLegacyTrait::assertNoLinkByHref` заменено на `$this->assertSession()->linkByHrefExists()`.
- [#3159230](https://www.drupal.org/project/drupal/issues/3159230) Исправлены оставшиеся вызовы с передачей `$message` в `AssertLegacyTrait::assertRaw` и `AssertLegacyTrait::assertNoRaw`.
- [#3168946](https://www.drupal.org/project/drupal/issues/3168946) Использование устаревшего `AssertLegacyTrait::assertTextHelper` заменено на `$this->assertSession()->pageTextContains()` и `$this->assertSession()->pageTextNotContains()`.
- [#3139407](https://www.drupal.org/project/drupal/issues/3139407) Использование устаревших `AssertLegacyTrait::assertFieldById` и `AssertLegacyTrait::assertNoFieldById` заменено на `$this->assertSession()->fieldExists()`, `$this->assertSession()->buttonExists()` и `$this->assertSession()->fieldValueEquals()`.
- [#3139406](https://www.drupal.org/project/drupal/issues/3139406) Использование устаревших `AssertLegacyTrait::assertFieldByName` и `AssertLegacyTrait::assertNoFieldByName`.
- [#3166543](https://www.drupal.org/project/drupal/issues/3166543) `UiHelperTrait::drupalPostForm` теперь помечен устаревшим по всем стандартам.
- [#3168788](https://www.drupal.org/project/drupal/issues/3168788) Использование xpath заменено на WebAssert.
- [#2802401](https://www.drupal.org/project/drupal/issues/2802401) Передача `NULL` в качестве параметра для `$edit` в `::drupalPostForm` помечена устаревшей.
- [#3171920](https://www.drupal.org/project/drupal/issues/3171920) В `AssertLegacyTrait` поправлено сообщение об устаревшем коде.
- [#3162403](https://www.drupal.org/project/drupal/issues/3162403) `symfony/phpunit-bridge` обновлён до 5.1.6, чтобы решить проблему с некорректным сообщением об устаревшем коде.
- [#3142267](https://www.drupal.org/project/drupal/issues/3142267) Использование трейта `PHPUnit8Warnings` заменено `PhpUnitWarnings`.
- [#3173888](https://www.drupal.org/project/drupal/issues/3173888) У функции `_drupal_error_handler_real()` удалён параметр `$context` и код обновлён в соответствии с данным изменением.
- [#3135027](https://www.drupal.org/project/drupal/issues/3135027) Использование `UnitTestCase::assertArrayEquals` заменено на `$this->assertEquals()`.
- [#3174038](https://www.drupal.org/project/drupal/issues/3174038) `DrupalSelenium2Driver` теперь открывает архив с флагом `\ZipArchive::CREATE` вместо `\ZipArchive::OVERWRITE`.
- [#3162008](https://www.drupal.org/project/drupal/issues/3162008) `SectionComponentTest::testToRenderArray` теперь возвращает объект события, чтобы соответствовать возвращаемому типу `EventDispatcherInterface::dispatch` из Symfony 5.
- [#3174158](https://www.drupal.org/project/drupal/issues/3174158) Тест предупреждений обновлён для соответствия PHP 8, так как используемый вариант «деления на ноль» теперь не предупреждение, а фатальная ошибка.
- [#3172438](https://www.drupal.org/project/drupal/issues/3172438) Использование аннотации `@expectedDeprecation` заменено на `ExpectDeprecationTrait::expectDeprecation()`.
- [#2858646](https://www.drupal.org/project/drupal/issues/2858646) Исправлены вызовы метода `::setUp()` с некорректным регистром.
- [#3174928](https://www.drupal.org/project/drupal/issues/3174928) Внесены улучшения в JS тесты ядра.
- [#3166450](https://www.drupal.org/project/drupal/issues/3166450) Использование условий с `&&` в аргументе для `::assertTrue()` разбито на несколько вызовов.
- [#3139429](https://www.drupal.org/project/drupal/issues/3139429) Использование `AssertLegacyTrait::assertFieldByXPath` и `AssertLegacyTrait::assertNoFieldByXPath` заменено на `$this->xpath()`.
- [#3145418](https://www.drupal.org/project/drupal/issues/3145418) Удалены вызовы `t()` в аргументах для `::assertText()`.
- [#3145005](https://www.drupal.org/project/drupal/issues/3145005) Удалены вызовы `t()` в вызовах `::drupalPostForm()`.
- [#3183420](https://www.drupal.org/project/drupal/issues/3183420) Для класса `DrupalSelenium2Driver` добавлен новый метод `::attachFile()`.
- [#3168375](https://www.drupal.org/project/drupal/issues/3168375) Некоторые вызовы `::drupalPostForm()` заменены на `::submitForm()`.
- [#3176200](https://www.drupal.org/project/drupal/issues/3176200) Удалены вызовы `t()` в аргументах для `::assertNoText()`.
- [#3184324](https://www.drupal.org/project/drupal/issues/3184324) Вызовы `::drupalPostForm()`, которые записывают результат в переменную, заменены на `::submitForm()`.
- [#3128815](https://www.drupal.org/project/drupal/issues/3128815) Вызовы `::assert*()` методов, которые содержали операторы сравнения больше/меньше, заменены на `::assert(Greater|Less)Than(OrEqual)()`.

## Прочие изменения

- [#3123472](https://www.drupal.org/node/3123472) Последовательный вызов методов `StorageComparer` больше не используется в условиях.
- [#2488350](https://www.drupal.org/node/2488350) При установке Drupal теперь используется кеш-бэкенд в памяти. Это позволяет ускорить установку.
- [#3127255](https://www.drupal.org/node/3127255) Из проверки системных требований удалены проверки `mbstring.http_input` и `mbstring.http_output`. Данные параметры, начиная с PHP 5.6 являются устаревшими и ничего не возвращают.
- [#2778917](https://www.drupal.org/node/2778917) Вместо тернарного оператора при вызове `\Drupal::state()->get()` теперь используется второй параметр.
- [#3021788](https://www.drupal.org/node/3021788) Функции `template_preprocess_menu_local_task()` и `template_preprocess_menu_local_action()` перенесены в `core/includes/theme.inc`.
- [#3112328](https://www.drupal.org/node/3112328) Классы расширяющие `FormatterBase` больше не реализуют `ContainerFactoryPluginInterface`, так как это объявлено в `FormatterBase`.
- [#3033734](https://www.drupal.org/node/3033734) На странице списка модулей исправлена горизонтальная прокрутка при больших описаниях.
- [#3112790](https://www.drupal.org/project/drupal/issues/3112790) Исправлена неполадка, из-за которой «установка» модулей User и System происходила дважды.
- [#3143605](https://www.drupal.org/project/drupal/issues/3143605) Удалена функция `update_replace_permissions()`.
- [#2972224](https://www.drupal.org/project/drupal/issues/2972224) В ядро добавлен `.cspell.json` для автоматической проверки правописания в ядре Drupal.
- [#2256367](https://www.drupal.org/project/drupal/issues/2256367) Использование «web site» в документации и UI заменено на «website».
- [#3143724](https://www.drupal.org/project/drupal/issues/3143724) «dont» заменён на «do_not» в якоре ссылки на документацию.
- [#3153790](https://www.drupal.org/project/drupal/issues/3153790) Исправлена опечатка в сервисе `user.flood_subscriber`.
- [#3055189](https://www.drupal.org/project/drupal/issues/3055189) Маппинг ключей в несколько строк помечен устаревшим в Symfony 4.3, соответствующие изменения внесены в ядро.
- [#3143713](https://www.drupal.org/project/drupal/issues/3143713) Функция `drupal_get_schema_versions()` теперь всегда возвращает целые числа.
- [#3154594](https://www.drupal.org/project/drupal/issues/3154594) `composer.json` и `composer.lock` будут пропускаться CSpell.
- [#3154665](https://www.drupal.org/project/drupal/issues/3154665) Из словаря CSpell удалены названия модулей и плагинов.
- [#2807743](https://www.drupal.org/project/drupal/issues/2807743) Триггеры ошибок для `FormattableMarkup::placeholderFormat()` приведены к единому стилю.
- [#2619482](https://www.drupal.org/project/drupal/issues/2619482) Использование `get_called_class()` и `get_class($this)` заменены на `static::class`.
- [#2928960](https://www.drupal.org/project/drupal/issues/2928960) Длина слогана сайта увеличена со 128 символов до 255.
- [#3155770](https://www.drupal.org/project/drupal/issues/3155770) Удалены избыточные указания реализации `ContainerFactoryPluginInterface` когда класс расширял уже класс реализующий интерфейс.
- [#3154914](https://www.drupal.org/project/drupal/issues/3154914) Исправлены грамматически ошибки при употреблении множественного и единственного числа.
- [#3157546](https://www.drupal.org/project/drupal/issues/3157546) В `MAINTAINERS.txt` добавлен mondrake в качестве мейнтейнера тест фреймворка.
- [#3157954](https://www.drupal.org/project/drupal/issues/3157954) Из тестов удалены избыточные ребилды маршрутов.
- [#2989262](https://www.drupal.org/project/drupal/issues/2989262) В генератор `.htaccess` файла добавлены экранирования для точек и запятых.
- [#3116858](https://www.drupal.org/project/drupal/issues/3116858) `ExtensionDiscovery` теперь кеширует не объект расширения целиком, а только информацию о нём.
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
- [#3083044](https://www.drupal.org/project/drupal/issues/3083044) (откачено) Первая колонка для таблиц с сортировкой теперь имеет стиль `display: flex;`.
- [#3168074](https://www.drupal.org/project/drupal/issues/3168074) Исправлены комментарии для `FeedStorage` и `ItemStorage`.
- [#3154909](https://www.drupal.org/project/drupal/issues/3154909) Употребление «not existing» заменено на «non-existent».
- [#3162972](https://www.drupal.org/project/drupal/issues/3162972) Исправлены опечатки в 32 словах для XSS тестов.
- [#3169306](https://www.drupal.org/project/drupal/issues/3169306) Исправлены дубли «the» в документации.
- [#3169286](https://www.drupal.org/project/drupal/issues/3169286) Исправлены дубли «more» в документации.
- [#3169543](https://www.drupal.org/project/drupal/issues/3169543) Из cspell словаря удалены исправленные опечатки.
- [#3170675](https://www.drupal.org/project/drupal/issues/3170675) Адрес `http://cgit.drupalcode.org` заменён на `https://git.drupalcode.org/project/drupal`.
- [#3055193](https://www.drupal.org/project/drupal/issues/3055193) Использование `Symfony\Component\HttpFoundation\File\MimeType\MimeTypeGuesser` заменено на `Symfony\Component\Mime\MimeTypes`.
- [#3156944](https://www.drupal.org/project/drupal/issues/3156944) Добавлена поддержка `nullable` тайпхинта в `ProxyBuilder`.
- [#3170626](https://www.drupal.org/project/drupal/issues/3170626) Удалено дублирование «on» в `SystemTestController`.
- [#3170627](https://www.drupal.org/project/drupal/issues/3170627) Удалено дублирование «list» в `InstallHelper`.
- [#3170629](https://www.drupal.org/project/drupal/issues/3170629) Удалено дублирование «from» в комментариях к коду.
- [#3171872](https://www.drupal.org/project/drupal/issues/3171872) Удалено дублирование «for» в комментариях к коду.
- [#3156880](https://www.drupal.org/project/drupal/issues/3156880) `CsrfTokenGenerator::validate` теперь проверяет, является ли `$token` строкой перед вызовом `hash_equals()`.
- [#3173991](https://www.drupal.org/project/drupal/issues/3173991) Передача аргументов в анонимные функции при использовании `array_*` функций теперь производится без ссылки.
- [#3173440](https://www.drupal.org/project/drupal/issues/3173440) Удалено дублирование «will» в комментариях к коду.
- [#3172537](https://www.drupal.org/project/drupal/issues/3172537) Создание экземпляра `Symfony\Component\Process\Process` теперь происходит через метод `::fromShellCommandline`.
- [#3174022](https://www.drupal.org/project/drupal/issues/3174022) Теперь при вызове `call_user_func_array()`, там где это возможно, значения аргументов передаются используя `array_values()`.
- [#3176990](https://www.drupal.org/project/drupal/issues/3176990) cspell теперь также проверяет файлы начинающиеся с точки.
- [#3172582](https://www.drupal.org/project/drupal/issues/3172582) Ссылка на форматы дат в PHP обновлена на новую.
- [#3171267](https://www.drupal.org/project/drupal/issues/3171267) bnjmnm добавлен в список мейнтейнеров в качестве временного мейнтейнера a11y.
- [#2607116](https://www.drupal.org/project/drupal/issues/2607116) Удалена неиспользуемая переменная `$language` в `hook_tokens_alter()`.
- [#3177557](https://www.drupal.org/project/drupal/issues/3177557) Внесены улучшения в `\Drupal\error_test\Controller\ErrorTestController::generateWarnings()` для совместимости с PHP 8.
- [#3179013](https://www.drupal.org/project/drupal/issues/3179013) Из теста `EntityRouteEnhancerTest` удалено ненужное сравнение, которое также приводило к ошибке на PHP 8.
- [#3178998](https://www.drupal.org/project/drupal/issues/3178998) Внесены улучшения в типы ошибок и сообщений об ошибках для поддержки PHP 8.
- [#3177541](https://www.drupal.org/project/drupal/issues/3177541) Внесены улучшения в `LocalStream` для совместимости с PHP 8.
- [#3156260](https://www.drupal.org/project/drupal/issues/3156260) Добавлена документация о `title` переменной во всех шаблонах `feed-icon.html.twig`.
- [#3138746](https://www.drupal.org/project/drupal/issues/3138746) Множественные улучшения в коде где должен быть CamelCase.
- [#2987980](https://www.drupal.org/project/drupal/issues/2987980) Произведён рефакторинг `UncaughtExceptionTest`.
- [#3178581](https://www.drupal.org/project/drupal/issues/3178581) Из словаря проекта удалены слова с ошибками.
- [#2851394](https://www.drupal.org/project/drupal/issues/2851394) Некорректные употребления «a» заменены на «an».
- [#3183712](https://www.drupal.org/project/drupal/issues/3183712) Добавлена документация параметра `$operation` для `hook_entity_access()`.
- [#3165065](https://www.drupal.org/project/drupal/issues/3165065) Улучшена документация для методов `PluginWithFormsTrait`.
- [#3053897](https://www.drupal.org/project/drupal/issues/3053897) Модулям `jsonapi` и `migrate_drupal_multilingual` добавлен параметр `version` в *.info.yml.
- [#3183673](https://www.drupal.org/project/drupal/issues/3183673) Применены автофиксы для стандарта `Drupal.Commenting.DocComment.ShortFullStop`.

## Ссылки

- [Drupal 9.1.0](https://www.drupal.org/project/drupal/releases/9.1.0) (англ.), drupal.org, 2 декабря 2020
