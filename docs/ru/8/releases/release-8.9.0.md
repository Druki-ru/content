---
id: release-8.9.0
title: 'Drupal 8.9.0'
path: /8/releases/8.9.0
core: 8
metatags:
  title: 'Drupal 8.9.0: Список изменений'
  description: 'Список изменений Drupal 8.9.0.'
---

**Дата релиза**: 3 июня 2020 г.

Данный релиз является переходным с [Drupal 8](../drupal-8.md) на [Drupal 9](../../9/drupal-9.md).

**Drupal 8.9.0** API совместим с [**Drupal 9.0.0**](../../9/releases/release-9.0.0.md). Главное отличие между версиями — Drupal 9.0.0 не содержит код, который помечен `@deprecated` в Drupal 8.9.0.

Изменения в данной версии минимальны, по причине того, что он разрабатывается одновременно с Drupal 9 и все силы направлены на чистку устаревшего кода Drupal 8.

## hook_install, hook_uninstall, hook_modules_installed и hook_modules_uninstalled теперь получают параметр $is_syncing

- [#3098920](https://www.drupal.org/node/3098920)

Теперь в хуки `hook_install()`, `hook_uninstall()`, `hook_modules_installed()` и `hook_modules_uninstalled()` передаётся новый параметр `$is_syncing`.

Параметр `$is_syncing` будет `TRUE` когда модуль устанавливается или удаляется как часть процесса обновления конфигураций. Это позволит корректировать поведение модуля с учётом данного значения. Например, если происходит синхронизация конфигураций, вы не должны производить никаких изменений в конфигурационных объектах и сущностях.

**Раньше**

```php
/**
 * Implements hook_install().
 */
function demo_umami_content_install() {
  if (!\Drupal::service('config.installer')->isSyncing()) {
    \Drupal::classResolver(InstallHelper::class)->importContent();
  }
}
```

**Теперь**

```php
/**
 * Implements hook_install().
 */
function demo_umami_content_install($is_syncing) {
  if (!$is_syncing) {
    \Drupal::classResolver(InstallHelper::class)->importContent();
  }
}
```

## Twig фильтр `without()` теперь может принимать массив

- [#3102853](https://www.drupal.org/node/3102853)

Иногда требуется перенести большое количество полей из `{{ content }}` массива и отрендерить их в ручном режиме.

Вы не можете не рендерить `{{ content }}` и просто вызывать поля напрямую, так как это может нарушить целостность кэша.

До текущего изменения `without()` принимал лишь аргументы в качестве строк чтобы вы могли контролировать поведение, теперь он может принимать массив из строк.

**Ранее**

```twig
<article>
  <div class="fancy">
    {{ content.field_fancy_1 }}
    {{ content.field_fancy_2 }}
    {{ content.field_fancy_3 }}
    {{ content.field_fancy_4 }}
  </div>
  <div class="important">
    {{ content.field_important_1 }}
    {{ content.field_important_2 }}
    {{ content.field_important_3 }}
    {{ content.field_important_4 }}
  </div>
  <div class="content">
    {{ content|without('field_fancy_1', 'field_fancy_2', 'field_fancy_3', 'field_fancy_4', 'field_important_1', 'field_important_2', 'field_important_3', 'field_important_4', 'field_special') }}
  </div>
  <div class="special">
    {{ content.field_special }}
  </div>
</article>
```

**Теперь**

```twig
{%
  set fancy_fields = [
    'field_fancy_1',
    'field_fancy_2',
    'field_fancy_3',
    'field_fancy_4',
  ]
%}
{%
  set important_fields = [
    'field_important_1',
    'field_important_2',
    'field_important_3',
    'field_important_4',
  ]
%}
<article>
  <div class="important">
    {% for field in important_fields %}
      {{ content[field] }}
    {% endfor %}
  </div>
  <div class="fancy">
    {% for field in fancy_fields %}
      {{ content[field] }}
    {% endfor %}
  </div>
  <div class="content">
    {{ content|without(fancy_fields, important_fields, 'field_special') }}
  </div>
  <div class="special">
      {{ content.field_special }}
  </div>
</article>
```

## Теперь к листингам добавляется новый кэш тег [entity_type]_list:[bundle]

- [#3107058](https://www.drupal.org/node/3107058)

Теперь для листингов возвращается дополнительный кэш тег `[entity_type]_list:[bundle]`, если у сущности имеется поддержка бандлов.

Ранее, сущность возвращала кэш тег для списков `[entity_type]_list` (`node_list`), который автоматически устанавливался на выводы со списком конкретной сущности. Например это делает Views, если вы выводите сущности.

Это приводило к ситуациям, когда обновление одной сущности инвалидировало множество других, не имеющих отношения к текущей. Например, у вас есть типы материалов «Новость» и «Отзыв», а для них созданы страницы с листингом `/news` и `/reviews`, соответственно. На оба листинга ранее устанавливался кэш тег `node_list`, и когда вы правили новость, он инвалидировался и очищал соответствующие кэш данные, но это также задевало листинг отзывов, что совершенно ненужно. Теперь, изменяя новость, будут инвалидированы только новости, а другие типы содержимого останутся в кэше.

Данное изменение позволит снизить частоту обращения и инвалидации кэша без необходимости.

## Добавлена поддержка поиска библиотек в папке сайта и установочных профилях

- [#3099614](https://www.drupal.org/node/3099614)

Некоторые модули используют модуль Libraries, для того чтобы хранить сторонние библиотеки в отличных от `/libraries` директориях. Данное поведение и возможность перенесена в ядро.

Все [библиотеки](../libraries/libraries.md), путь которых начинается с `/libraries` теперь ищутся в нескольких директориях в соответствующем порядке:

1. `/PATH/TO/SITE/libraries` (например `/sites/default/libraries`)
1. `/libraries`
1. `/PATH/TO/INSTALL_PROFILE` (например `/core/profiles/demo_umami/libraries`)

Первый файл что будет обнаружен и будет использован для библиотеки.

Дополнительно в ядро добавлен новый [сервис](../services/services.md) `library.libraries_directory_file_finder`, метод `find()` которого полная альтернатива для `libraries_get_path()`.

## Разработчики теперь могут создавать собственные Session Bag

- [#3109877](https://www.drupal.org/node/3109877)

Теперь разработчики могут создавать свои собственные «сессионные мешки» для хранения информации конкретного модуля внутри сессии.

Сессионный мешок должен реализовывать `\Symfony\Component\HttpFoundation\Session\SessionBagInterface` и быть объявлен как [сервис с меткой](../services/services.md). Получить сессионный мешок можно через сессию из запроса.

**Пример объявления сервиса:**

```yaml
example.session_bag:
  class: Drupal\example\Session\ExampleSessionBag
  tags:
    - { name: session_bag }
```

**Пример получения сессионного мешка:**

```php
$bag = $request->getSession()->getBag(ExampleSessionBag::BAG_NAME);
$bag->setFlag();
```

## Темы могут объявлять зависимости на модули

- [#474684](https://www.drupal.org/node/474684)

[Темы оформления](../themes/themes.md) теперь могут указывать зависимости на модули, которые необходимы для работы. Принцип указания зависимостей такойже как и у [модулей](../modules/modules.md) — при помощи свойства `dependencies`.

```yaml
name: test theme depending on nonexisting module
type: theme
core: 8.x
dependencies:
  - test_module_non_existing
```

## Оформление и темизация

- [#3040758](https://www.drupal.org/node/3040758) В теме оформления Classy на поля с меткой «в строку» теперь добавляется класс `clearfix`. Если вызовет проблемы в вашей теме, вы можете переопределить данное поведение перезаписав шаблон `field.html.twig` в своей теме.
- [#3115005](https://www.drupal.org/node/3115005) Из тем оформления удалён фикс для fieldset в Firefox, так как ошибку в браузере исправили.
- [#2550717](https://www.drupal.org/node/2550717) [js-cookie](https://github.com/js-cookie/js-cookie) (`core/js-cookie`) добавлен в ядро на замену `core/jquery.cookie`, которая теперь помечена устаревшей.

## Ban

- [#3126784](https://www.drupal.org/node/3126784) Класс `\Drupal\ban\Plugin\migrate\destination\BlockedIP` приведён в соответствии с его названием файла `BlockedIp`.

## Book

- [#3010378](https://www.drupal.org/node/3010378) `BookManager::buildItems` теперь не загружает сущности для генерации ссылки, а генерирует роут на основе ID. Это значительно повышает производительность на материалах с большим количество «подшивок».

## Composer

- [#3127013](https://www.drupal.org/node/3127013) `wikimedia/composer-merge-plugin` удалён из зависимостей ядра.
- [#3121885](https://www.drupal.org/node/3121885) `drupal/coder` обновлён до 8.3.8.

## Claro

- [#3078524](https://www.drupal.org/node/3078524) Стили фокусировки WYSIWYG элементов приведены к общему стилю.
- [#3101040](https://www.drupal.org/node/3101040) Исправлена проблема, когда стиль фокусировки обрезался.

## Database API

- [#3113403](https://www.drupal.org/node/3113403) (временно откачено) Все драйверы баз данных теперь должны переопределять `Drupal\Core\Database\Query\Condition`.
- [#3118832](https://www.drupal.org/node/3118832) Сторонние драйвера баз данные теперь могут иметь те же самые имена что и в ядре. Также, в тестах, терминале и установщике будет использоваться драйвер указанный в `settings.php` в приоритете над стандартным.
- [#3120096](https://www.drupal.org/node/3120096) Драйвера баз данных теперь могут располагаться в `src/Driver/Database/[DriverName]` под соответствующим неймспейсом `Drupal\[module_name]\Driver\[DriverName]`. Drupal теперь будет автоматически находить данные драйвера без необходимости их копировать в `/core/lib` или `/drivers/lib`.
- [#3126940](https://www.drupal.org/node/3126940) Теперь в URI до БД нет необходимости указывать опциональный параметр `module`.
- [#2956556](https://www.drupal.org/node/2956556) `StatementPrefetch` теперь передает также `$class_name` в `fetchOptions`.
- [#3125391](https://www.drupal.org/node/3125391) В возвращаемые типы данных методом `Connection::query()` добавлено строковое значение, а также описание, когда оно может таким стать.

## Extension API

- [#3066801](https://www.drupal.org/node/3066801) Добавлен новый хук `hook_removed_post_updates()` который позволяет указать какие `hook_post_update_N()` реализации были удалены и в какой версии.
- [#3039296](https://www.drupal.org/node/3039296) Примеры для `hook_install()` и `hook_uninstall()` обновлены и упрощены.

## File

- [#3016814](https://www.drupal.org/node/3016814) Теперь хук `hook_file_download()` не будет вызываться при отсутствии аргументов для файла.

## Field UI

- [#3126617](https://www.drupal.org/node/3126617) Исправлена опечатка в теге для Help Topics. `<me>` заменён на `<em>`.

## Views

- [#3092185](https://www.drupal.org/node/3092185) Добавлен `rel="nofollow"` для заголовков сортировки таблиц.

## Composer

- [#3090416](https://www.drupal.org/node/3090416) Представлен Composer плагин Core Project Message.
- [#3103090](https://www.drupal.org/node/3103090) [drupal/core-composer-scaffold](../../composer/drupal-core-composer-scaffold.md) теперь не заменяет файлы которые не изменились, а также не выводит сообщение о файлах которые не были изменены. Это сократит выводимое сообщение данным пакетом в композере.
- [#3126566](https://www.drupal.org/node/3126566) Плагины, предоставляемые Drupal, теперь совместимы с Composer 2.

## Color

- [#3082742](https://www.drupal.org/node/3082742) Тип данных возвращаемый функцией `color_valid_hexadecimal_string()` был изменён. До изменения возвращаемый тип был числом (0 или 1), теперь возвращается булево значение (`TRUE`, `FALSE`).

## Config Translation

- [#3131388](https://www.drupal.org/node/3131388) Добавлен тест на страницу с листингом конфигураций. Исправлен DI приводящий к WSOD.

## JSON:API

- [#3115772](https://www.drupal.org/node/3115772) Вызовы `User::load` заменены на [Dependency Injection](../services/dependency-injection.md).

## Language

- [#3130438](https://www.drupal.org/node/3130438) Сервис `page_cache_kill_switch` в `LanguageNegotiationBrowser` теперь добавляется через [Dependency Injection](../services/dependency-injection.md).

## Locale

- [#3119278](https://www.drupal.org/node/3119278) Теперь сообщения при обновлении и проверки локазаций будут более информативными. Дополнительно будут выводиться и языковые коды, для которых производится проверка.

## Migrate

- [#2746541](https://www.drupal.org/node/2746541) Добавлена новая миграция `d*_node_complete.yml`, которая именуется как "complete node migration". Данная миграция переносить все ноды, их ревизии, переводы и переводы ревизий.
- [#2966856](https://www.drupal.org/node/2966856) Модуль `migrate_drupal_multilingual` удалён из зависимостей миграций и скрыт из интерфейса. Мультиязычные миграции теперь стабильны и данный модуль больше не требуется и будет удалён в Drupal 10.
- [#3128069](https://www.drupal.org/node/3128069) Название модуля «Book» теперь пишется с заглавной буквы в `OverviewForm`.

## Media Library

- [#3127838](https://www.drupal.org/node/3127838) Кнопка "Сохранить и вставить" теперь не имеет дополнительного вложения в массиве.

## Plugin API

- [#3115987](https://www.drupal.org/node/3115987) Свойство `$instanceIDs` переименовано в `$instanceIds`.

## Seven

- [#2980304](https://www.drupal.org/node/2980304) Внесены исправления в `<details>` и `<summary>` чтобы фокусировка работала во всех браузерах.

## Help Topics

- [#3041926](https://www.drupal.org/node/3041926) Инструкции `hook_help()` для модулей `automated_cron`, `dblog`, `syslog`, `system`, `update` и `user` конвертированы в Help Topics.
- [#3087562](https://www.drupal.org/node/3087562) Инструкции `hook_help()` для модулей `datetime`, `datetime_range`, `field`, `field_ui`, `link`, `options`, `telehpone`, `text` конвертированы в Help Topics.

## Update

- [#3098475](https://www.drupal.org/node/3098475) Теперь страница обновления БД будет показывать ожидающие обновления, зависимости и требования которых до сих пор не удовлетворены и они не могут быть применены.

## Views

- [#3113556](https://www.drupal.org/node/3113556) Views UI теперь использует библиотеку `core/drupal.dialog.ajax` вместо `core/jquery.ui.dialog`.
- [#3123653](https://www.drupal.org/node/3123653) Исправлена опечатка в документации `BulkForm::clickSortable`.
- [#2997748](https://www.drupal.org/node/2997748) Джоины теперь могут использовать `left_formula` вместо `left_field` для того чтобы применять SQL выражения для левой части джоина.
- [#3103010](https://www.drupal.org/node/3103010) В интерфейсе представлений улучшены пояснения для машинного имени.
- [#3119279](https://www.drupal.org/node/3119279) В шаблоне `views-view-table.html.twig` сводка в `<details>` теперь подготавливается в препроцессе при помощи рендер массива. Это также подключит полифил для браузеров, в которых нет нативной поддержки данного элемента.

## Тестирование

- [#3105980](https://www.drupal.org/node/3105980) Метод `\Drupal\migrate_drupal\Tests\StubTestTrait::createStub` был переименован в `createEntityStub`.
- [#3118477](https://www.drupal.org/node/3118477) Реестр темы теперь создаётся через мок, для того чтобы избежать конфликта когда он создавался в двух разных местах `RegistryTest` и `RegistryLegacyTest`.
- [#3078671](https://www.drupal.org/node/3078671) Зависимость `behat/mink` обновлена до 1.8.0, а `behat/mink-selenium2-driver` до 1.4.0.
- [#3121827](https://www.drupal.org/node/3121827) Исправлена докуменация в тестах `update_test_last_removed`.
- [#3126694](https://www.drupal.org/node/3126694) В тесте `ThemeTest` заменено использование `tpl.twig` на `html.twig.`
- [#3119922](https://www.drupal.org/node/3119922) `DeleteTruncateTest` больше не опирается на стандартную сортировку, которая может быть разной у каждой БД. Теперь сортировка явно указана.
- [#3124769](https://www.drupal.org/node/3124769) `UiHelperTrait::drupalLogout` теперь получает путь для редиректа напрямую из маршрута `user.page` вместо захардкоженого `/user`.
- [#3119924](https://www.drupal.org/node/3119924) `SelectPagerDefaultTest` больше не опирается на стандартную сортировку, которая может быть разной у каждой БД. Теперь сортировка явно указана.
- [#3128575](https://www.drupal.org/node/3128575) Использование `is_null()` в сравнениях заменено на нативные методы `::assertNotNull()`, `::assertNull()`.
- [#3130396](https://www.drupal.org/node/3130396) Использование `is_file()` в сравнениях заменено на нативные методы `::assertFileExists()`, `::assertFileNotExists()`.
- [#3131090](https://www.drupal.org/node/3131090) Использование `is_dir()` в сравнениях заменено на нативные методы `::assertDirectoryExists()`, `::assertDirectoryNotExists()`.
- [#3130811](https://www.drupal.org/node/3130811) Удалена передача аргумента `$message` для методов `::assertInstanceOf()` и `::assertNotInstanceOf()`.
- [#3131088](https://www.drupal.org/node/3131088) Использование `file_exists()` в сравнениях заменено на нативные методы `::assertFileExists()`, `::assertFileNotExists()`.
- [#3128814](https://www.drupal.org/node/3128814) Использование `:assert*` методов для сравнения количества заменено на штатный `::assertCount`.
- [#3131223](https://www.drupal.org/node/3131223) Использование `array_key_exists()` в сравнениях заменено на нативные методы `::assertArrayHasKey()`, `::assertArrayNotHasKey()`.
- [#3129074](https://www.drupal.org/node/3129074) Произведен рефактор сравнений, результаты которых используются в условиях.
- [#3126787](https://www.drupal.org/node/3126787) (только Drupal 8) Добавлены методы проверки типов для обратной с PHPUnit 6 и 7 через `::assertInternalType()`.
- [#3131258](https://www.drupal.org/node/3131258) Удалена передача аргумента `$message` для методов `::assertFileExists()`, `::assertFileNotExists()`, `::assertDirectoryExists()`, `::assertDirectoryNotExists()`.
- [#3131821](https://www.drupal.org/node/3131821) Использование `is_callable()` в сравнениях заменено на нативные методы `::assertIsCallable()`, `::assertIsNotCallable()`.
- [#3131821](https://www.drupal.org/node/3131821) Использование `is_numeric()` в сравнениях заменено на нативные методы `::assertIsNumeric()`, `::assertIsNotNumeric()`.
- [#3131816](https://www.drupal.org/node/3131816) Использование `is_array()` в сравнениях заменено на нативные методы `::assertIsArray()`, `::assertIsNotArray()`.
- [#3131818](https://www.drupal.org/node/3131818) Использование `is_object()` в сравнениях заменено на нативные методы `::assertIsObject()`, `::assertIsObject()`.
- [#3131819](https://www.drupal.org/node/3131819) Использование `is_resource()` в сравнениях заменено на нативный метод `::assertIsResource()`.
- [#3108006](https://www.drupal.org/node/3108006) Использование `::assertInternalType` заменено на соответствующие методы: `::assertIsArray`, `::assertIsBool` и т.д.

## Производительность

- [#3092180](https://www.drupal.org/node/3092180) Добавлен новый кэш контекст `protocol_version`, который позволяет иметь различные результаты для разных протоколов (HTTP/1.0, HTTP/1.1 или HTTP/2).

## Прочие изменения

- [#3083486](https://www.drupal.org/node/3083486) Конфигурация `action.settings.recursion_limit` и её схема была удалена. Drupal ядро не использовало данную конфигурацию.
- [#3111613](https://www.drupal.org/node/3111613) Удалены два `protected` метода у `Drupal\Core\Entity\Sql\SqlContentEntityStorageSchema`. Они не использовались ядром, а так как являются защищёнными, не относятся к публичному API.
- [#3113062](https://www.drupal.org/node/3113062) Функция `system_user_timezone()` помечена как `@internal` и будет удалена в [Drupal 9](../../9/releases/release-9.0.0.md).
- [#3054049](https://www.drupal.org/node/3054049) Добавлена новая [проверка доступа маршрута](../routing/route-access-control.md) `_entity_bundles`, при помощи которой можно создать [маршрут](../routing/routing.md) принимающий только конкретные бандлы сущности из аргумента. Например: `_entity_bundles: 'node:article|page'`.
- [#3114909](https://www.drupal.org/node/3114909) Документация для `EntityTypeInterface::hasHandlerClass` и `::getHandlerClass` приведены к одному виду.
- [#2999549](https://www.drupal.org/node/2999549) Добавлен новый служебный роут `<button>` для генерации ссылки в виде кнопки `route:<button>`.
- [#3120954](https://www.drupal.org/node/3120954) Сообщение об устаревшей функции приведено в соответствии с требования для `Registry` класса.
- [#3121229](https://www.drupal.org/node/3121229) Удалён эксперементальный модуль `config_environment`, который находился в альфа версии.
- [#3104015](https://www.drupal.org/node/3104015) Zend Framework стал Laminas, поэтому все зависимости и код был заменён на новые.
- [#3107243](https://www.drupal.org/node/3107243) Исправлены метки для `link.schema.yml` файла. Теперь они утверждения, вместо вопросов.
- [#3126003](https://www.drupal.org/node/3126003) Исправлена ошибка приводящая к увеличенному потреблению памяти в момент установки Drupal.
- [#3126923](https://www.drupal.org/node/3126923) Исправлена неполадка, вызванная изменением #3120096, приводящая к ошибкам на PHP 7.0.
- [#3127960](https://www.drupal.org/node/3127960) Удалено некорректное нижнее подчёркивание в миграции Drupal 7 `Shortcut`.
- [#2455465](https://www.drupal.org/node/2455465) Удалена поддержка PHP 5 для `.htaccess` и добавлена поддержка PHP 7.
- [#3084078](https://www.drupal.org/node/3084078) `AdminNegotiator::determineActiveTheme()` теперь соответствует интерфейсу и явно возвращает `NULL`.
- [#3118726](https://www.drupal.org/node/3118726) js.cookie обновлён до 3.0.
- [#3107472](https://www.drupal.org/node/3107472) `DbDumpCommand` теперь использует `Drupal::VERSION` вместо захардкоженного значения.

## Смотрите также

- [Drupal 9](../../9/drupal-9.md)