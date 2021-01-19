---
id: release-9.2.0
title: 'Drupal 9.2.0'
path: /9/releases/9.2.0
core: 9
metatags:
  title: 'Drupal 9.2.0: Список изменений'
  description: 'Список изменений Drupal 9.2.0.'
---

**Дата релиза**: 2 июня 2021\
**Активная поддержка до**: 01 декабря 2021\
**Поддержка безопасности до**: 2022

> [!WARNING]
> Drupal 9.2.0 находится в разработке.

## Объект исключения теперь передаётся в контексте в логер

- [#2949419](https://www.drupal.org/project/drupal/issues/2949419)

При логировании исключения теперь можно получить объект самого исключения из контекста `$context['exception']`.

Данное изменение соответствует [PSR-3](https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-3-logger-interface.md#13-context) а также позволит проще обрабатывать ошибки сторонними решениями, например [Raven](https://www.drupal.org/project/raven).

## Передача StatementInterface объекта в Connection::query() помечена устаревшим

- [#3154439](https://www.drupal.org/node/3154439)

Передача объекта реализующего `Drupal\Core\Database\StatementInterface` в `Drupal\Core\Database\Connection::query()` помечена устаревшей и будет удалена в [Drupal 10](../../10/drupal-10.md). Сейчас метод позволяет передавать только строковые значения для параметра `$query`.

Было:

```php
  $db = \Drupal\Core\Database\Database::getConnection();
  $stmt = $db->prepareStatement('SELECT * FROM {test}', []);
  $result = $db->query($stmt);
```

Стало:

```php
  $db = \Drupal\Core\Database\Database::getConnection();
  $result = $db->query('SELECT * FROM {test}');
```

## Библиотеки jQuery UI помечены устаревшими

- [#3113400](https://www.drupal.org/project/drupal/issues/3113400)

Следующие библиотеки ядра помечены устаревшими:

- `jquery.ui`
- `jquery.ui.autocomplete`
- `jquery.ui.dialog`
- `jquery.ui.draggable`
- `jquery.ui.menu`
- `jquery.ui.mouse`
- `jquery.ui.position`
- `jquery.ui.resizable`
- `jquery.ui.widget`

Если ваши модули используют их, вам необходимо обновить зависимости и использовать для этого контрибные модули. Полный и актуальный список контрибных модулей для конкретных библиотек вы найдёте на странице проекта [jQuery UI](https://www.drupal.org/project/jquery_ui).

Библиотеки `drupal.autocomplete`, `drupal.dialog` и `drupal.tabbingmanager` продолжать иметь зависимости на jQuery UI JS и CSS файлы, но они явно прописаны в данных библиотеках, без использования устаревших.

Модули и темы переопределяющие или заменяющие данные библиотеки теперь должны производить нужные изменения непосредственно в библиотеках `drupal.autocomplete`, `drupal.dialog` и `drupal.tabbingmanager`.

## Добавлено новое свойство 'bundle' для маршрутов entity:*

- [#3155568](https://www.drupal.org/project/drupal/issues/3155568)

Настройки для маршрутов сущностей теперь принимают новое свойство `bundle` в качестве массива. При указании данного свойства, конвертация параметра для сущности будет ограничена указанными бандлами.

```yaml
example.route:
  path: foo/{example}
  options:
    parameters:
      example:
        type: entity:node
        bundle:
          - article
          - news
```

В примере выше, параметр `{example}` будет конвертирован в ноду только если это нода типа «article» или «news».

В связи с этим, свойство маршрута `_entity_bundles` помечено устаревшим.

Благодаря данному изменения, при несоответствии типов, маршрут теперь будет отвечать HTTP 404 вместо HTTP 403.

## Расширения больше не могут объявлять мажорную версию

- [#3105353](https://www.drupal.org/project/drupal/issues/3105353)

Расширения ([модули](../modules/modules.md), [темы оформления](../themes/themes.md)) больше не могут указывать свойство `major` в ***.info.yml** файлах.

## Изменены сигнатуры конструкторов EntityFieldManager и EntityLastInstalledSchemaRepository

- [#3131585](https://www.drupal.org/project/drupal/issues/3131585)

Конструктор класса `Drupal\Core\Entity\EntityFieldManager` теперь принимает новый параметр типа `\Drupal\Core\Entity\EntityLastInstalledSchemaRepositoryInterface`. Для сохранения [обратной совместимости](../../../backward-compatibility.md) параметр является опциональным, тем не менее, он станет обязательным в [Drupal 10](../../10/drupal-10.md).

Конструктор класса `Drupal\Core\Entity\EntityLastInstalledSchemaRepository` теперь принимает новый параметр типа `\Drupal\Core\Cache\CacheBackendInterface`. Для сохранения [обратной совместимости](../../../backward-compatibility.md) параметр является опциональным, тем не менее, он станет обязательным в [Drupal 10](../../10/drupal-10.md).

Данное изменение также немного улучшает производительность сущностей, снижая количество запросов к БД.

## Drupal теперь использует возможности PHP для генерации ID сессии

- [#2238561](https://www.drupal.org/project/drupal/issues/2238561)

Drupal теперь использует встроенный в PHP механизм генерации ID сессии. Это означает что `session_id()` и `\Drupal::service('session')->getId()` не гарантируют что вернут одни и те же результаты, даже если сессия в Drupal инициализирована. Это вызвано тем что Drupal использует "ленивые сессии" - сессия будет инициализирована только в случае попытки записать туда значение.

Это также означает что Drupal теперь поддерживает следующие настройки PHP:

- [session.sid_length](http://php.net/manual/en/session.configuration.php#ini.session.sid-length)
- [session.sid_bits_per_character](http://php.net/manual/en/session.configuration.php#ini.session.sid-bits-per-character)

### Идентификаторы сессии для анонимных пользователей

Drupal старается избегать создания сессии для анонимных пользователей на столько, на сколько это возможно. Некоторые модули, например Flag, для своей работы требуют открытия сессии для всех пользователей.

Подобные модули использовали ID сессии как идентификатор анонимного пользователя, что сейчас является устаревшим способом. Вместо этого, модули должны хранить уникальные идентификаторы в самой сессии, например:

```php
 if (!$session->has('core.tempstore.private.owner')) {
    // This generates a unique identifier for the user
    $session->set('core.tempstore.private.owner', Crypt::randomBytesBase64());
 }
```

### SharedTempStore и SharedTempStoreFactory получили новые зависимости

В `SharedTempStore` теперь [инжектится](../services/dependency-injection.md) [сервис](../services/services.md) `current_user`. 

В `SharedTempStoreFactory` теперь инжектятся сервисы `current_user` и `session`.

`ShareTempStoreFactory` больше не использует ID сессии в качестве идентификатора анонимного пользователя. Он генерирует свой уникальный ID когда это требуется. Данное значение хранится в сессии под ключом `core.tempstore.shared.owner`.

### Drupal\Core\Session\SessionManager::migrateStoredSession() удалён

Для того чтобы использовать встроенный в PHP генератор ID сессии был удалён метод `Drupal\Core\Session\SessionManager::migrateStoredSession()`. Замена не предоставляется, так как данный метод не был частью публичного API.

Собственные реализации `SessionManagerInterface` которые расширяют `SessionManager` должны быть обновлены чтобы не вызывать данный метод. Генерация сессии теперь производится при помощи вызова `session_regenerate_id()` который производится в `\Symfony\Component\HttpFoundation\Session\Storage\NativeSessionStorage::regenerate()`.

Если имеются собственные реализации которые переопределяют метод `::migrateStoredSession()`, то данный код может быть удалён после того, как минимальное требование модуля будет Drupal 9.2.0.

Код, использующий данный метод будет удалён, поэтому это не вызовет проблем. Если вам требуется мигрировать старую сессию в новую, вы по прежнему можете использовать `\Drupal::service('session')->migrate();`.

## Все идентификаторы в запросах теперь должны быть обёрнуты в кавычки

- [#3189880](https://www.drupal.org/project/drupal/issues/3189880)

Имена столбцов и синонимы таблиц должны быть обёрнуты квадратными кавычками (`[]`) при использовании `Connection::query()`, `db_query()`, `ConditionInterface::where()` или `SelectInterface::addExpression()`. Это позволит слою баз данных корректно оборачивать данные идентификаторы.

Например:

```php
$connection->query('SELECT [nid] FROM {node} WHERE [nid] = :id', [':id' => '1']);
```

Данное изменение позволит нам использовать различные типы баз данных без необходимости заводить список слов, которые не могут быть использованы в качестве названия таблиц или полей.

Запросы построенные при помощи `Connection::select()`, `Connection::insert()`, `Connection::delete()`, `Connection::update()`, `Connection::upsert()`, `Connection::merge()` не требуют добавлять квадратные скобки при вызовах `::fields()` или `::condition()`. Новый способ записи должен быть использовать при добавлении "сырого" SQL значения в объект при помощи `::addExpression()`, `::where()` или `::having()`.

Например:

```php
// Обратите внимание, что это плохой пример, так как использование ::condition() было бы лучше.
$connection->select('node')->where('[nid] = :id', [':id' => '1']);
```

В крайне редких случаях запросам требуются квадратные скобки, например, при создании SQL функции. В таких ситуациях задайте параметр `$option['allow_square_brackets']` равным `TRUE`.

Например:

```php
$connection->query('CREATE OR REPLACE FUNCTION "substring_index"(text, text, integer) RETURNS text AS
  \'SELECT array_to_string((string_to_array($1, $2)) [1:$3], $2);\'
  LANGUAGE \'sql\'',
  [],
  ['allow_delimiter_in_query' => TRUE, 'allow_square_brackets' => TRUE]
);
```

### Изменения в драйверах БД

Если вы являетесь автором или имеет свой собственный драйвер БД, вам необходимо задать значение свойству `\Drupal\Core\Database\Connection::$identifierQuotes`. Значение должно быть массивом из двух строк. Первое значение массива отвечает за символ начала цитаты, второе, его закрытие.

В [Drupal 10](../../10/drupal-10.md) данное свойство станет обязательным. В [Drupal 9](../drupal-9.md), когда значение не задано, будут использованы пустые строки.

Проверьте все переопределения `\Drupal\Core\Database\Connection::escapeDatabase()`, `\Drupal\Core\Database\Connection::escapeTable()`, `\Drupal\Core\Database\Connection::escapeField()` и `\Drupal\Core\Database\Connection::escapeAlias()`. Скорее всего, они больше не потребуются.

Если драйвер использует `\Drupal\Core\Database\Connection::$escapedNames`, код должен быть обновлён с использованием либо `\Drupal\Core\Database\Connection::$escapedTables`, либо `\Drupal\Core\Database\Connection::$escapedFields`.

## Изменения связанные с восстановлением пароля

- [#1521996](https://www.drupal.org/project/drupal/issues/1521996)

В процесс восстановления пароля внесены некоторые изменения.

### Восстановление пароля больше не раскрывает имя пользователя или что указанный email используется на сайте

Форма восстановления пароля больше не отображения сообщение, указывающее, что введенное имя пользователя или email не существуют.

#### Ранее

Если указанное имя пользователя или email не использовалось на сайте, выводилось следующее сообщение:

> unknown@example.com is not recognized as a username or an email address.

Данное сообщение раскрывает информацию о том, что пользователь с указанными данными не зарегистрирован.

Когда пользователь зарегистрирован и предоставлены корректные данные, выводилось следующее сообщение:

> Further instructions have been sent to your email address.

Данное сообщение можно использовать как подтверждение, что указанные данные используются на сайте.

#### После изменений

Сообщения были изменены таким образом, чтобы из них было невозможно получить не подтверждающую, не опровергающую информацию.

Если предоставлены некорректные имя пользователя (валидация на длину, допустимые символы и т.д.) или email, будет показано следующее сообщение:

> The username or email address is invalid.

Если валидация на имя пользователя или email прошла успешно, будет показано следующее сообщение:

> If [username or email] is a valid account, an email will be sent with instructions to reset your password.

### Изменена сигнатура UserPasswordForm

Класс `\Drupal\user\Form\UserPasswordForm` теперь принимает два новых аргумента в конструкторе:

```php
   * @param \Drupal\Core\TypedData\TypedDataManagerInterface $typed_data_manager
   *   The typed data manager.
   * @param \Drupal\Component\Utility\EmailValidatorInterface $email_validator
   *   The email validator service.
```

Если ваш код инициализирует данный класс самостоятельно без использования метода `::create()`, обновите свой код.

Данные параметры опциональны в [Drupal 9](../drupal-9.md) но станут обязательными в [Drupal 10](../../10/drupal-10.md).

## Добавлены хуки для изменения форм виджетов полей

- [#2872162](https://www.drupal.org/project/drupal/issues/2872162)

Представлено два новых [хука](../hooks/hooks.md) для редактирования форм используемые для настроек виджетов полей: `hook_field_widget_complete_form_alter()`, `hook_field_widget_complete_WIDGET_TYPE_form_alter()`, `hook_field_widget_single_element_form_alter()` и `hook_field_widget_single_element_WIDGET_TYPE_form_alter()`.

Из-за введения новых хуков, следующие хуки помечены устаревшими: `hook_field_widget_multiple_form_alter()`, `hook_field_widget_multiple_WIDGET_TYPE_form_alter()`, `hook_field_widget_form_alter()` и `hook_field_widget_WIDGET_TYPE_form_alter()`.

## AJAX

- [#3179939](https://www.drupal.org/project/drupal/issues/3179939) Удалён неиспользуемый `AjaxTestBase`.

## Book

- [#2575827](https://www.drupal.org/project/drupal/issues/2575827) `BookNavigationBlock` и `BookNavigationCacheContext` теперь получают информацию из `route_match` [сервиса](../services/services.md), вместо получения из аттрибутов запроса.
- [#2470896](https://www.drupal.org/project/drupal/issues/2470896) Подшивки книг теперь переводимы. В связи с чем классы `Drupal\book\BookExport`, `Drupal\book\BookBreadcrumbBuilder` и `Drupal\book\BookManager` имеют новые зависимости.
- [#3188519](https://www.drupal.org/project/drupal/issues/3188519) В конструкторе `BookBreadcrumbBuilder` исправлена ошибка в названии сервиса.

## CKEditor

- [#3150364](https://www.drupal.org/project/drupal/issues/3150364) Улучшена документация (`/admin/help/ckeditor`) для CKEditor.

## Composer

- [#3096781](https://www.drupal.org/project/drupal/issues/3096781) Зависимости `symfony/mime`, `symfony/var-dumper` и `symfony/phpunit-bridge` обновлены до версии 5.2. Добавлена новая зависимость `symfony/deprecation-contracts`.
- [#3187025](https://www.drupal.org/project/drupal/issues/3187025) Зависимости ядра обновлены на 8.12.2020.

## Contact

- [#3173756](https://www.drupal.org/project/drupal/issues/3173756) Добавлена административная вкладка "Просмотр" для контактных форм.

## Cron System

- [#2863464](https://www.drupal.org/project/drupal/issues/2863464) Логи [крона](../hooks/cron.md) теперь используют тип `info` вместо `notice`, для большего соответствия [RFC 5424](https://tools.ietf.org/html/rfc5424).

## Database System

- [#3129563](https://www.drupal.org/project/drupal/issues/3129563) Сторонние драйвера БД теперь могут переопределять стандартные реализации расширителей запросов: `Drupal\Core\Database\Query\PagerSelectExtender`, `Drupal\Core\Database\Query\TableSortExtender` и `Drupal\search\SearchQuery`.
- [#3129534](https://www.drupal.org/project/drupal/issues/3129534) Добавлены новые методы `Drupal\Core\Database\Connection::getProvider()` и `Connection::enableModuleProvidingDatabaseDriver()`.

## Editor

- [#3181290](https://www.drupal.org/project/drupal/issues/3181290) Удалён неиспользуемый код в `editor.admin.es6.js`.

## Entity System

- [#3138631](https://www.drupal.org/project/drupal/issues/3138631) Если поле содержит некорректную связь, сообщение об ошибке теперь будет более точно сообщать с чем связана проблема.

## Form API

- [#2702233](https://www.drupal.org/project/drupal/issues/2702233) Добавлены JavaScript тесты для Form API `#states` состояний: `required`, `visible`, `invisible`, `expanded`, `checked`, `unchecked`.

## Help Topics

- [#3090257](https://www.drupal.org/project/drupal/issues/3090257) Добавлено больше тестов проверки синтаксиса.

## JavaScript

- [#3191497](https://www.drupal.org/project/drupal/issues/3191497) `cpre/jquery.ui.dialog` добавлена зависимость `core/jquery`.

## Layout Builder

- [#3180674](https://www.drupal.org/project/drupal/issues/3180674) Удалён неиспользуемый модуль `layout_builder_overrides_test`.

## MySQL DB Driver

- [#3185231](https://www.drupal.org/project/drupal/issues/3185231) Режим SQL при инициализации теперь задаётся через `ANSI,TRADITIONAL` вместо перечисления всех возможных значений.

## Migration System

- [#2939328](https://www.drupal.org/project/drupal/issues/2939328) Внесены улучшения в подсказки для Drupal Migrate UI.
- [#3176394](https://www.drupal.org/project/drupal/issues/3176394) Типы комментариев больше не мигрируют если в источнике отключен модуль `comment`.
- [#3151363](https://www.drupal.org/project/drupal/issues/3151363) Исправлена подготовка пути до источника. Теперь она будет без двойных `//` слэшей.
- [#3184545](https://www.drupal.org/project/drupal/issues/3184545) Для `Migration::getMigrationPluginManager()` исправлена документация о возвращаемом типе.
- [#2920168](https://www.drupal.org/project/drupal/issues/2920168) Удалён метод `SqlBaseTest::getHighWaterStorage()`.
- [#3047328](https://www.drupal.org/project/drupal/issues/3047328) Обработчик `Log` теперь может логгировать различные типы данных, а не только строки.
- [#3148959](https://www.drupal.org/project/drupal/issues/3148959) Обработчик `Extract` теперь отображает при обработке какого значение произошла ошибка.
- [#3187477](https://www.drupal.org/project/drupal/issues/3187477) Их источника `TermTranslation` удалено подключение трейта I18nQueryTrait.
- [#2937989](https://www.drupal.org/project/drupal/issues/2937989) Источник данных `Node` для Drupal 6 и Drupal 7 теперь может принимать сразу несколько типов содержимого в качестве источника.
- [#3189878](https://www.drupal.org/project/drupal/issues/3189878) Из плагина источника данных Drupal 7 File удалено свойство `temporaryPath`.
- [#3189064](https://www.drupal.org/project/drupal/issues/3189064) Для плагинов источников данных расширяющих `SqlBase` зависимость `database` больше не сериализуется.
- [#3187263](https://www.drupal.org/project/drupal/issues/3187263) Миграции для конфигурационных блоков `d6_block_translation` и `d7_block_translation` теперь запрашивают `config_translation` вместо `content_translation`.

## Node System

- [#3187435](https://www.drupal.org/project/drupal/issues/3187435) Для Views плагина `Vid`, аргумент `$database` помечен устаревшим.

## Olivero

- [#3187884](https://www.drupal.org/project/drupal/issues/3187884) Удален лишний условный оператор из `block--system-branding-block.html.twig`.
- [#3180281](https://www.drupal.org/project/drupal/issues/3180281) Максимальная длина описания для элементов форм задана в 60 символов.

## PostgreSQL DB Driver

- [#3185399](https://www.drupal.org/project/drupal/issues/3185399) Для определения предыдущего ID теперь используется `RETURNING`.

## System

- [#2409413](https://www.drupal.org/project/drupal/issues/2409413) Удалены неиспользуемые RSS настройки и описания.
- [#3002983](https://www.drupal.org/project/drupal/issues/3002983) Протокол в ссылках заменён на HTTPS.

## Taxonomy

- [#3170185](https://www.drupal.org/project/drupal/issues/3170185) `Drupal\taxonomy\Form\OverviewTerms` теперь использует `pager.manager` для получения текущей страницы вместо `Request`.

## Toolbar

- [#3174422](https://www.drupal.org/project/drupal/issues/3174422) Для тулбара добавлен класс `clearfix`.

## Umami Demo

- [#3051465](https://www.drupal.org/project/drupal/issues/3051465) Поле тегов для материалов теперь не переводимое и не создаёт автоматически несуществующие теги.
- [#3066570](https://www.drupal.org/project/drupal/issues/3066570) Роль редактора теперь имеет больше прав, в связи с чем может: управлять переводами, изучать страницы "помощи", просматривать таксономию, управлять ярлыками.
- [#3061267](https://www.drupal.org/project/drupal/issues/3061267) Машинные имена блоков теперь имеют префикс равный машинному имени темы оформления.
- [#3108503](https://www.drupal.org/project/drupal/issues/3108503) Теперь Layout Builder по умолчанию включен для всех типов содержимого.

## Update

- [#2577407](https://www.drupal.org/project/drupal/issues/2577407) Установка нового модуля через интерфейс теперь имеет постоянный лейбл «Add».

## User

- [#3186752](https://www.drupal.org/project/drupal/issues/3186752) Аргумент `$langcode` для функции `_user_mail_notify()` помечен устаревшим.

## Views

- [#2628130](https://www.drupal.org/project/drupal/issues/2628130) Параметр `$database` для `\Drupal\node\Plugin\views\argument\Vid` помечен устаревшим.
- [#2925612](https://www.drupal.org/project/drupal/issues/2925612) Метод `StylePluginBase::wizardForm()` помечен устаревшим, так как нигде не используется.
- [#3186582](https://www.drupal.org/project/drupal/issues/3186582) Отображение по умолчанию во Views теперь именуется как «Default».

## Тестирование

- [#3176655](https://www.drupal.org/project/drupal/issues/3176655) `GoutteDriver` помечен устаревшим, вместо него ядро теперь использует `BrowserKitDriver`.
- [#3132887](https://www.drupal.org/project/drupal/issues/3132887) `BrowserTestBase::drupalGetHeader()` помечен устаревшим. Используйте `$this->getSession()->getResponseHeader()`.
- [#3181329](https://www.drupal.org/project/drupal/issues/3181329) Удалён метод `BrowserTestBase::getResponseLogHandler()` так как он дублирует `BrowserHtmlDebugTrait::getResponseLogHandler()`.
- [#3184632](https://www.drupal.org/project/drupal/issues/3184632) Сравнения для submit инпутов с использованием xpath заменены на WebAssert.
- [#3139431](https://www.drupal.org/project/drupal/issues/3139431) Вызовы устаревшего `AssertLegacyTrait::assertFieldsByValue()` заменены на современные аналоги.
- [#3132919](https://www.drupal.org/project/drupal/issues/3132919) Вызовы `::assert*()` со сравнением в качестве ожидаемого значения заменены на `::assertNot*()`.
- [#3191986](https://www.drupal.org/project/drupal/issues/3191986) Вызовы устаревшего `AssertLegacyTrait::assertNotIdentical()` заменены на современные аналоги.
- [#3169171](https://www.drupal.org/project/drupal/issues/3169171) Исправлены сообщения о [депрекации](../../../deprecation.md).
- [#3192221](https://www.drupal.org/project/drupal/issues/3192221) Изменён порядок передачи аргументов в вызовах `::assertIdentical()` для последующей замены на `::assertSame()`.

## Symfony 5

- [#3188056](https://www.drupal.org/project/drupal/issues/3188056) Обновлён код, который вызывал сериалайзер Symfony с `NULL` в качестве формата.

## Прочие изменения

- [#3173636](https://www.drupal.org/project/drupal/issues/3173636) В `PagerManager` добавлена реализация `PagerManagerInterface::findPage()`.
- [#3181084](https://www.drupal.org/project/drupal/issues/3181084) Из `web.config` удалены комментарии для правила `httpoxy`.
- [#3180998](https://www.drupal.org/project/drupal/issues/3180998) Удалён код для поддержки старых версий PHP ниже 7.3.
- [#3164676](https://www.drupal.org/project/drupal/issues/3164676) Из словаря cspell удалены слова с ошибками, фикстуры для тестов добавлены в список игнорируемых файлов.
- [#3187489](https://www.drupal.org/project/drupal/issues/3187489) Sally Young (justafish) и Théodore Biadala (nod_) добавлены в список мейнтенеров ядра.
- [#3178135](https://www.drupal.org/project/drupal/issues/3178135) `favicon.ico` из `/core/misc` обновлён для соответствия новому логотипу Drupal.
- [#3185652](https://www.drupal.org/project/drupal/issues/3185652) Исправлены множественные ошибки для соответствия стандарту `Drupal.Commenting.DocComment.ShortFullStop`.
- [#3183656](https://www.drupal.org/project/drupal/issues/3183656) Исправлены множественные ошибки для соответствия стандарту `Drupal.Commenting.DocComment.TagGroupSpacing`.
- [#3185614](https://www.drupal.org/project/drupal/issues/3185614) Исправлена опечатка в «The users's account name».
- [#3170260](https://www.drupal.org/project/ideas/issues/3170260) В `MAINTAINERS.txt` добавлен раздел для инициативы Decoupled Menus.
- [#3188920](https://www.drupal.org/project/drupal/issues/3188920) Guzzle исключения обновления для совместимости с Guzzle 7.
- [#3147135](https://www.drupal.org/project/drupal/issues/3147135) Исправлены множественные ошибки для соответствия стандарту `Drupal.Classes.UseGlobalClass`.
- [#3189466](https://www.drupal.org/project/drupal/issues/3189466) Francesco Placella (plach) снял с себя полномочия мейнтейнера ядра.
- [#3162827](https://www.drupal.org/project/drupal/issues/3162827) В модулях ядра, где использование хранилища сущности является опциональным, данные хранилища запрашиваются только в данной ситуации. Следовательно, в классах теперь хранится `entityStorage`, вместо хранилища конкретной сущности.
- [#3162822](https://www.drupal.org/project/drupal/issues/3162822) Исправлены проблемы со словами включающие "reference" и CSPell.