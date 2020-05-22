---
id: release-9.0.0
title: 'Drupal 9.0.0'
path: /9/releases/9.0.0
core: 9
metatags:
  title: 'Drupal 9.0.0: Список изменений'
  description: 'Список изменений Drupal 9.0.0.'
---

**Дата релиза**: 3 июня 2020 г.

> [!WARNING]
> Drupal 9.0.0 находится в разработке.

Данный релиз является переходным с [Drupal 8](../../8/drupal-8.md) на [Drupal 9](../drupal-9.md).

**Drupal 9.0.0** API совместим с [**Drupal 8.9.0**](../../8/releases/release-8.9.0.md). Главное отличие между версиями — Drupal 9.0.0 не содержит код, который помечен `@deprecated` в Drupal 8.9.0.

Изменения в данной версии минимальны, по причине того, что он разрабатывается одновременно с Drupal 9 и все силы направлены на чистку устаревшего кода Drupal 8.

Новый функционал и возможности будут добавляться начиная с Drupal 9.1.0.

## Drupal 8.8.0 — минимальная версия для обновления на Drupal 9 

- [#3106666](https://www.drupal.org/node/3106666)

Для обновления с Drupal 8 до Drupal 9 необходимо иметь как минимум версию Drupal 8.8.0.

Изменение связано с тем, что все апдейт хуки до 8.8.0-rc1 были удалены из Drupal 9. Это означает, что Drupal 9 не содержит обновлений для базы данных для более ранних версий и вы не сможете корректно произвести обновление.

Перед обновлением до Drupal 9, вам будет необходимо обновить сайт как минимум до [Drupal 8.8.0](../../8/releases/release-8.8.0.md), а затем обновляться до Drupal 9.

## Исходный код jQuery UI был добавлен в ядро Drupal

- [#3089526](https://www.drupal.org/node/3089526)

Библиотеки что не были удалены в [Drupal 8.8.0](../../8/releases/release-8.8.0.md) форкнуты в ядро Drupal 9. Данный код будет поддерживаться сообществом на случай обнаружения различных уязвимостей до конца поддержки Drupal 9. Цель: заменить использование всех библиотек и пометить их устаревшими до релиза Drupal 10.

Ранее Drupal ядро содержало минифицированные версии данных JavaScript библиотек, теперь будет доступен и их исходный код соответствующий [jQuery UI 1.12.1](https://github.com/jquery/jquery-ui/tree/1.12.1).

## Представлена новая «стабильная» тема оформления специально для Drupal 9 — Stable 9

- [#3050374](https://www.drupal.org/node/3050374)

**Stable 9** — новая «стабильная» тема оформления поставляемая с Drupal 9. Основной задачей данной темы является сохранения слоя обратной совместимости с ядром на уровне разметки, CSS и JavaScript.

### Отличия от Stable темы Drupal 8

- Все шаблоны и CSS файлы были обновлены для соответствия актуальным версиям в модулях.
- `normalize.css` обновлён с 3.0.3 до 8.0.1.
- Тема Stable 9 хранит в себе копии всех изображений из `/modules` и `/misc`. Копии хранятся только для тех изображений, который были изменены.
- Неиспользуемые шаблоны и CSS файлы были удалены. В данный список вошли:

    - `file.admin.css`
    - `filter.admin.css`
    - `simpleteset-result-summary.html.twig`
    
### Моя тема расширяет Stable от Drupal 8, что мне нужно сделать?

Stable тема от Drupal 8 будет поддерживаться и поставляться с ядром Drupal 9, но данная тема будет удалена в Drupal 10 и перенесена в контриб проект. Таким образом, необходимо будет отрефакторить свою тему на новую за время жизни Drupal 9.

### Моя тема расширяет Classy, что мне нужно сделать?

Classy продолжит расширять Stable от Drupal 8. Будущее Classy после Drupal 9 на данный момент обсуждается. Тем не менее, тема будет поддерживаться либо ядром, либо как контриб проект.

## info.yml файлы больше не принимают параметр «core»

- [#3119415](https://www.drupal.org/node/3119415)

Поддержка параметра `core` в `info.yml` файлах была удалена. Вместо него необходимо использовать новый параметр `core_version_requirment`, который доступен начиная с [Drupal 8.7.7](../../8/releases/release-8.7.7.md).

## Оформление и темизация

- [#3103178](https://www.drupal.org/node/3103178) Темы оформления из ядра (Bartik, Claro, Seven и Umami) больше не наследуются от Classy. Тема Classy будет переведена в контриб до релиза Drupal 10.
- [#3115102](https://www.drupal.org/node/3115102) Добавлен полифил `es6-promise` ([stefanpenner/es6-promise](https://github.com/stefanpenner/es6-promise)) для поддержки `Promise`.
- [#3113447](https://www.drupal.org/node/3113447) Добавлен полифил `drupal.object.assign` для поддержки `Object.assign()`.
- [#3113446](https://www.drupal.org/node/3113446) Добавлен полифил `drupal.array.find` для поддержки `Array.find()`.
- [#3112670](https://www.drupal.org/node/3112670) Popper.js обновлён до версии 2.0.6.
- [#3116334](https://www.drupal.org/node/3116334) Обновлён шаблон `update-version.html.twig` для поддержки `core_compatibility_details`.
- [#3117217](https://www.drupal.org/node/3117217) В темах ядра произведена чистка зависимостей от `stable` темы оформления.
- [#2821525](https://www.drupal.org/node/2821525) `normalize.css` обновлён до актуальной версии 8.0.1.
- [#3117330](https://www.drupal.org/node/3117330) Использование функций темизации теперь будет вызывать явную ошибку, так как они являются устаревшими начиная с Drupal 8.8.x и будут удалены в Drupal 10.
- [#3115223](https://www.drupal.org/node/3115223) Stable больше не используется как базовая тема в ядре.

## Aggregator

- [#3046957](https://www.drupal.org/node/3046957) Справка из `hook_help()` конвертирована в Help Topics. 

## Claro

- [#3109351](https://www.drupal.org/node/3109351) Удалена библиотека `image-widget` стили которой нигде не использовались.

## Composer

- [#3133442](https://www.drupal.org/node/3133442) Обновлены зависимости ядра.
- [#3032686](https://www.drupal.org/node/3032686) Неиспользуемые в Drupal 9 зависимости удалены: `jcalderonzumba/gastonjs`, `jcalderonzumba/mink-phantomjs-driver`, `paragonie/random_compat`, `phpunit/phpunit-mock-objects`, `brumann/polyfill-unserialize`, `doctrine/cache`, `doctrine/collections`, `doctrine/common`, `doctrine/inflector`, `paragonie/random_compat`, `phpunit/phpunit-mock-objects`.
- [#3134648](https://www.drupal.org/node/3134648) `drupal/core-recommended` больше не запрашивает `composer/installers`.

## Contact

- [#3067617](https://www.drupal.org/node/3067617) Справка из `hook_help()` конвертирована в Help Topics.

## Database API

- [#2986452](https://www.drupal.org/node/2986452) Название колонок и синонимов должны оборачиваться квадратными скобками (`[]`) при использовании `Connection::query`, `db_query()`, `ConditionInterface::where`.
- [#2986894](https://www.drupal.org/node/3126658) Удалён метод `\Drupal\Core\Database\Connection::identifierQuote` и добавлено свойство `\Drupal\Core\Database\Connection::$identifierQuotes`. 
- [#3123376](https://www.drupal.org/node/3123376) Функция `update_fix_incompatibility()` была удалена так как она не нужна в Drupal 9, а также, теоретически, может привести к повреждению данных.
- [#3112476](https://www.drupal.org/node/3112476) `Database::getDatabaseDriverNamespace` помечен устаревшим, вместо него используйте `$info['namespace']`.
- [#3119170](https://www.drupal.org/node/3119170) Из `pgsql/Schema::findPrimaryKeyColumns` удалено решение для реализации `array_position` для PosgreSQL 9.5 и ниже. Начиная с PostgreSQL 9.6 данная функция появилась в БД, а Drupal 9 требует PosgtgreSQL 10+.
- [#3124354](https://www.drupal.org/node/3124354) Собственные или контриб драйвера БД теперь опционально объявлять свои классы для запросов БД. Ядро теперь предоставляет реализации по умолчанию `Drupal\Core\Database\Query`.
- [#2983452](https://www.drupal.org/node/2983452) SQLite теперь может использоваться в памяти при помощи URL: `sqlite://localhost/:memory:`.

## Entity API

- [#3118998](https://www.drupal.org/node/3118998) Параметр `$memory_cache` в конструкторе `ContentEntityStorageBase` больше не принимает значение `NULL` по умолчанию. Таким образом, параметр становится обязательным.

## Help Topics

- [#3055317](https://www.drupal.org/node/3055317) Справки для history, statistics и tracker модулей конвертированы из `hook_help()` в Help Topics.

## JavaScript

- [#3107918](https://www.drupal.org/node/3107918) Прекращена поддержка Node.js 8, Drupal теперь запрашивает Node.js 12.

## Migrate

- [#3134459](https://www.drupal.org/node/3134459) Из кода удалены метки `@group legacy`.

## System

- [#3137414](https://www.drupal.org/node/3137414) Логотип на странице статуса сайта заменён на новый для Drupal 9.
- [#3138671](https://www.drupal.org/node/3138671) Исправлены опечатки в слове «incompatitable».

## Twig

- [#3117250](https://www.drupal.org/node/3117250) `StringLoader` и `TestLoader` обновлены для поддержки Twig 3.

## Workspaces

- [#3108416](https://www.drupal.org/node/3108416) Удалено обновление `workspace_update_8803()`. Добавлено предупреждение, что прежде чем обновляться до Drupal 9, сначала необходимо обновиться до 8.8.2 как минимум.

## Тестирование

- [#3098322](https://www.drupal.org/node/3098322) Дампы базы данных для тестирования обновлений обновлены до Drupal 8.8.x.
- [#3112907](https://www.drupal.org/node/3112907) Модуль Simpletest удалён из ядра и перенесён в контриб.
- [#3055197](https://www.drupal.org/node/3055197) Убрана проверка на исключение `Goutte\Client`, так как в Symfony 5 отсутствует `Symfony\Component\BrowserKit\Client`.
- [#3116856](https://www.drupal.org/node/3116856) Теперь тесты будут фейлится при наличии предупреждений. Также исправлены предупреждения PHPUnit8 которые несовместимы с PHPUnit 9.
- [#3119027](https://www.drupal.org/node/3119027) Для теста `RestSettingsDeletionUpdateTest` теперь используется `drupal-8.8.0.filled.standard.php.gz`, так как там уже включён модуль Rest.
- [#3113211](https://www.drupal.org/node/3113211) Добавлен тест, проверяющий что темы из ядра не расширяют Stable и не используют её ассетов.
- [#3114041](https://www.drupal.org/node/3114041) `::setUp` доработан для корректной работы с PHPUnit 8.
- [#3119910](https://www.drupal.org/node/3119910) В `EntityQueryTest` использование кавычек заменено на квадратные скобки.
- [#3122961](https://www.drupal.org/node/3122961) PHPUnit 8 ругается на расширение конструктора, поэтому тесты теперь не расширяют конструктор.
- [#3126792](https://www.drupal.org/node/3126792) Использование `::assertAttributeSame` прекращено, так как он устарел. Вместо него используется `::assertSame`.
- [#3126569](https://www.drupal.org/node/3126569) Использование `::assertAttributeEmpty` прекращено, так как он устарел. Вместо него используется `::assertNull`.
- [#3126848](https://www.drupal.org/node/3126848) Использование параметра `$ignoreCase` для `::assertContains` прекращено.
- [#3107732](https://www.drupal.org/node/3107732) Для методов `::setUp`, `::tearDown`, `::setUpBeforeClass` и `:assertPostConditions` добавлены тайпхинты.
- [#2822382](https://www.drupal.org/node/2822382) Свойство `$modules` теперь `protected` для всех классов расширяющих `BrowserTestBase` и `KernelTestBase`.
- [#3127455](https://www.drupal.org/node/3127455) Для `BlockCreationTrait::placeBlock` исправлено описание параметра `id`.
- [#3126564](https://www.drupal.org/node/3126564) Использование `::assertArraySubset` прекращено, так как он устарел. Вместо него используется `::assertEquals` в цикле.
- [#3117863](https://www.drupal.org/node/3117863) Ограничение `Length` теперь (Symfony 5) требует обязательно указывать `allowEmptyString` вместе с `min` параметром.
- [#3119710](https://www.drupal.org/node/3119710) `NodeMigrateTypeTestTrait::getTableName` больше не приводит название таблицы к нижнему регистру, так как это больше не требуется.
- [#3122547](https://www.drupal.org/node/3122547) Исправлены тесты учитывающие исправление [#3121362](https://www.drupal.org/node/3121362) ([Drupal 8.8.6](../../8/releases/release-8.8.6.md).
- [#3015855](https://www.drupal.org/node/3015855) Обращения к `block_content` таблице теперь производятся через Entity API.
- [#3126969](https://www.drupal.org/node/3126969) Использование `::assertAttributeInstanceOf` прекращено, так как он устарел. Вместо него используется `::assertInstanceOf`.
- [#3082415](https://www.drupal.org/node/3082415) Использование `::assertIdentical` для булевых значений заменено на соответствующие `::assertTrue` и `::assertFalse`.
- [#3126563](https://www.drupal.org/node/3126563) Использование `::assertAttributeEqual` прекращено, так как он устарел. Вместо него используется `::assertEquals`.
- [#3126970](https://www.drupal.org/node/3126970) Использование `::readAttribute` прекращено, так как он устарел. Вместо него используется `::assertEquals`.
- [#3126971](https://www.drupal.org/node/3126971) Использование `::getObjectAttribute` прекращено, так как он устарел.
- [#3128905](https://www.drupal.org/node/3128905) Использование `assert*` на экземпляры заменено на `::assertInstanceOf()`, `::assertNotInstanceOf()`.
- [#3128746](https://www.drupal.org/node/3128746) Сравнения с использованием `strpos()` заменены на более точные строковые сравнения.
- [#3131402](https://www.drupal.org/node/3131402) Удалены тайпхинты возвращаемых данных для абстрактных классов.
- [#3112641](https://www.drupal.org/node/3112641) Удалена старая отсылка на `datetime_range_view_presave()` в `EntityTypeWithoutViewsDataTest::testEntityTypeWithoutViewsData()`.
- [#3117421](https://www.drupal.org/node/3117421) Drupal CI больше не будет запускать SimpleTest тесты.
- [#3131817](https://www.drupal.org/node/3131817) Использование `is_numberic()` заменено на нативные методы `::assertIsNumeric()`, `::assertIsNotNumeric()`.
- [#3135308](https://www.drupal.org/node/3135308) Исправлено использование устаревшего `Symfony\Component\BrowserKit\Response::getStatus()` на `getStatusCode()`.
- [#3135302](https://www.drupal.org/node/3135302) Удалёны старые тесты для Symfony 3: `ReverseProxyMiddlewareTest::testReverseProxyEnabledLegacy()`, `ReverseProxyMiddlewareTest::reverseProxyEnabledProviderLegacy()`.

## Прочие изменения

- [#3089166](https://www.drupal.org/node/3089166) Минимальная версия PHP 7.2.3, рекомендуемая — 7.3.
- [#3088712](https://www.drupal.org/node/3088712) Drupal ядро теперь запрашивает Symfony 4.4.
- [#3089508](https://www.drupal.org/node/3089508) Полифилы (html5shiv, matchMEdia, domready, classList) — удалены.
- [#3095199](https://www.drupal.org/node/3095199) Twig обновлён до 2.х версии.
- [#2955931](https://www.drupal.org/node/2955931) Зависимость `easyrdf/easyrdf` перенесена в `require-dev`.
- [#3096454](https://www.drupal.org/node/3096454) Функция `twig_without()` — удалена.
- [#3111612](https://www.drupal.org/node/3111612) Параметр `Connection` для Select query builder теперь находится на первой позиции. Было: `public function __construct($table, $alias, Connection $connection, $options = []) {}`, стало: `public function __construct(Connection $connection, $table, $alias = NULL, $options = []) {}`.
- [#3113653](https://www.drupal.org/node/3113653) PHPUnit обновлён до 8 версии. Изменения в тестах пока не требуются, но в планах стоит обновление до PHPUnit 9 которое повлечёт за собой изменения.
- [#3116384](https://www.drupal.org/node/3116384) В Drupal 8 можно было добавлять дополнительные загрузчики, для того чтобы включить поддержку APCu. Эта возможность была удалена в пользу загрузчика Composer.
- [#3116297](https://www.drupal.org/node/3116297) Symfony 4 больше не поставляет WinCache, таким образом в Drupal 9 он также будет отсутствовать. Рекомендуется использовать APCu как замену.
- [#3104265](https://www.drupal.org/project/drupal/issues/3104265) Doctrine разделил на части пакет `doctrine/common`. Drupal не использует все данные пакеты, таким образом зависимости обновлены и были удалены из зависимостей: `doctrine/cache`, `doctrine/collections`, `doctrine/common`, `doctrine/inflector`.
- [#3084472](https://www.drupal.org/project/drupal/issues/3084472) Трейт `DeprecatedModulesTestTrait` удалён.
- [#3109534](https://www.drupal.org/node/3109534) Минимальная версия MySQL увеличена до 5.7.8
- [#3120124](https://www.drupal.org/node/3120124) Минимальная версия MariaDB увеличена до 10.2.7.
- [#3107155](https://www.drupal.org/node/3107155) Минимальная версия SQLite увеличена до 3.26.
- [#3074585](https://www.drupal.org/node/3074585) Сервисы `app.root` и `site.path` заменены на параметры.
- [#3118079](https://www.drupal.org/node/3118079) `postcss-header` обновлён до 2.0.0 и убрано временное решение для корректной работы.
- [#3114079](https://www.drupal.org/node/3114079) Минимальная версия Apache увеличена до 2.4.7.
- [#2846994](https://www.drupal.org/node/2846994) Минимальная версия PostgreSQL увеличена до 10, также теперь требуется расширение `pg_trgm`.
- [#3087130](https://www.drupal.org/node/3087130) Конфигурации установочных профилей теперь могут зависеть от опциональных конфигураций модулей.
- [#3117558](https://www.drupal.org/node/3117558) Git hash для ядра 9.0.x в `composer.lock` исправлен на актуальный.
- [#3043471](https://www.drupal.org/node/3043471) Drupal теперь использует PSR-17 фабрики для создания PSR-7 `Request` и `Response`.
- [#3119017](https://www.drupal.org/node/3119017) Улучшена проверка версии драйвера баз данных для поддержки различных суфиксов в названии (докер билды).
- [#3117342](https://www.drupal.org/node/3117342) Все зависимости были обновлены.
- [#3121024](https://www.drupal.org/node/3121024) Удалён `@todo` для `ThemeInstaller::uninstall`.
- [#3123558](https://www.drupal.org/node/3123558) Компоненты Symfony обновлены до 4.4.7.
- [#3113876](https://www.drupal.org/node/3113876) Использование `GetResponseForExceptionEvent` заменено на `ExceptionEvent`.
- [#3123326](https://www.drupal.org/node/3123326) Удалены проверки и пометка о конфлитке с pathauto. Для Drupal 9 нету версий модуля которые несовместимы с ним.
- [#3032605](https://www.drupal.org/node/3032605) `Drupal\Component\DependencyInjection\Container` теперь реализует `ResetInterface`.
- [#3127641](https://www.drupal.org/node/3127641) Исправлена опечатка в вызове исключения `BadRequestHttpException` в `ResourceIdentifierNormalizer`.
- [#3117858](https://www.drupal.org/node/3117858) Прекращено использование `Symfony\Component\Process\Process::inheritEnvironmentVariables`, так как метод устарел, а начиная с Symfony 4.4 значения наследуются по умолчанию.
- [#3127674](https://www.drupal.org/node/3127674) Обновлены Composer зависимости.
- [#3117869](https://www.drupal.org/node/3117869) Передача аргумента в `Symfony\Component\HttpFoundation\Request::isMethodSafe` помечена устаревшей с Symfony 4.4. Внесены соответствующие изменения в ядро.
- [#3125234](https://www.drupal.org/node/3125234) `ProxyBuilder` обновлён для соответствия Symfony 5.
- [#3117857](https://www.drupal.org/node/3117857) Начиная с Symfony 4.4 `Symfony\Component\Debug\BufferingLogger` является устаревшим и вместо него теперь используется `Symfony\Component\ErrorHandler\BufferingLogger`.
- [#3129508](https://www.drupal.org/node/3129508) Множественные последовательные вызовы `dir()` заменены на число в качестве 2 аргумента.
- [#3094454](https://www.drupal.org/node/3094454) Включена проверка стандартов `@deprecated` которые должны содержать ссылку на изменения.
- [#3093173](https://www.drupal.org/node/3093173) Для jQuery UI форка добавлен README.
- [#3094398](https://www.drupal.org/node/3094398) События для Symfony запросов и ответов обновлены в соответствии с изменениями.
- [#3130341](https://www.drupal.org/node/3130341) Удалён метод `Drupal\Core\Update\UpdateKernel::fixSerializedExtensionObjects()`.
- [#3123537](https://www.drupal.org/node/3123537) Страница о статусе сайта не будет показывать WSOD если оно нашло расширение без `core_version_requirement`, вместо этого будет показано сообщение о проблеме.
- [#2821499](https://www.drupal.org/node/2821499) Включена проверка `DrupalPractice.InfoFiles.NamespacedDependency`. Расширения должны указывать свои зависимости в формате `{project}:{module}`.
- [#2937513](https://www.drupal.org/node/2937513) Исправлены несоответствия стандарту кодирования `Drupal.Commenting.DocComment.TagGroupSpacing` в ядре.