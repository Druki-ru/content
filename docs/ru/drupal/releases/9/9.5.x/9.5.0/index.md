---
title: 'Drupal 9.5.0'
slug: wiki/drupal/releases/9.5.0
core: 9
metatags:
  title: 'Drupal 9.5.0: Список изменений'
  description: 'Список изменений Drupal 9.5.0.'
authors:
  - Niklan
  - chesn0k
category:
  area: 'Drupal 9.5.x'
  title: Drupal 9.5.0
  order: 0
---

**Дата релиза**: не позднее декабря 2022

<Aside type="warning">

Данная версия находится в разработке. Список изменений может меняться и дополняться, даты могут смещаться.

</Aside>

## Метод `Connection::lastInsertId()` теперь является частью публичного Database API

- [#3260007](https://www.drupal.org/node/3260007)

Метод `Drupal\Core\Database\Connection::lastInsertId()` больше не является `@internel` для того чтобы стать публичным API. Данный метод возвращает ID последней вставленной строки или последовательного значения.

## Drupal теперь предупреждает если MySQL база данных не использует уровень изоляции `READ COMMITED`

- [#2733675](https://www.drupal.org/node/2733675)

Стандартный уровень изоляции транзакций для MySQL и MariaDB равен `REPEATABLE READ`. Эта настройка в Drupal может приводить к мёртвым блокам в таблицах, которые могут замедлить работу БД или вовсе повредить её.

Рекомендуемый уровень изоляции транзакций для Drupal `READ COMMITED`. Другие два варианта `READ UNCOMMITED` и `SERIALIZABLE` не поддерживаются ядром, используйте их на свой страх и риск.

Drupal будут генерировать предупреждение на странице `status/report` если значение будет задано неверно.

Для того чтобы активировать `READ COMMITED` для Drupal (если он не включен), необходимо обновить настройки подключения к БД в [settings.php](../../../../9/settings-php/index.md):

```php
  'init_commands' => [
    'isolation_level' => 'SET SESSION TRANSACTION ISOLATION LEVEL READ COMMITTED',
  ],
```

Полный пример:

```php
$databases['default']['default'] = [
  'database' => 'databasename',
  'username' => 'sqlusername',
  'password' => 'sqlpassword',
  'host' => 'localhost',
  'driver' => 'mysql',
  'prefix' => '',
  'port' => '3306',
  'init_commands' => [
    'isolation_level' => 'SET SESSION TRANSACTION ISOLATION LEVEL READ COMMITTED',
  ],
];
```

## Рекомендуемая версия PHP — 8.1.6

- [#3294938](https://www.drupal.org/node/3294938)

Рекомендуемая версия PHP увеличена до 8.1.6.

## Добавлена поддержка `_defaults` ключа в `*.services.yml` файлах

- [#3021898](https://www.drupal.org/node/3021898)

Drupal теперь предоставляет функционал для установки значений по умолчанию 
для всех сервисов файла `*.services.yml`. На данный момент можно задать 
значения для трёк ключей: `autowire`, `public`, `tags`.

Эти значения будут применяться ко всем сервисам файла ([подробнее на сайте 
Symfony](https://symfony.com/doc/current/service_container.html):

```yaml
services:
  _defaults:
    autowire: true
    public: true
    tags:
      - 'foo.tag'
      - {name: bar.tag, value: 123}
  your_service:
    class: Drupal\your_module\YourClass
```

## Big Pipe

- [#3294720](https://www.drupal.org/node/3294720) `Drupal.attachBehaviors()` 
  теперь вызывается на элементы до окончания загрузки.

## Block

- [#3281429](https://www.drupal.org/node/3281429) Тесты, не использующие миграции, обновлены чтобы не использовать темы Bartik и Seven.
- 

## Claro

- [#3291100](https://www.drupal.org/node/3291100) Улучшено отображение `details` элемента.
- [#3085219](https://www.drupal.org/node/3085219) Улучшено оформление процесса установки сайта.

## CKEditor 4

- [#3271057](https://www.drupal.org/node/3271057) Интеграция Media Library с 
  CKEditor 4 перенесена в модуль CKEditor 4.
- [#3271094](https://www.drupal.org/node/3271094) Интеграция с Media 
  перенесена из одноимённого модуля в CKEditor.

## CKEditor 5

- [#3292626](https://www.drupal.org/node/3292626) Удалён файл стилей 
  `core/modules/ckeditor5/css/quickedit.css`.

## Comment

- [#2958241](https://www.drupal.org/node/2958241) Внесены улучшения в 
  `CommentSelection` для корректной работы комментариев с различными типами 
  сущностей.

## Composer

- [#3280399](https://www.drupal.org/node/3280399) Пакет `drupal/core-bridge:9.5.x` объявлен заброшенным.
- [#3280589](https://www.drupal.org/node/3280589) Скрипт для чистки vendor 
  директории объявлен устаревшим.
- [#3298343](https://www.drupal.org/node/3298343) Зависимость `egulias/email-validator` обновлена до версии 3.2.1.

## Entity API

- [#3266589](https://www.drupal.org/node/3266589) Удалён заголовок `Link` из ответа страницы сущности.

## File

- [#2588013](https://www.drupal.org/node/2588013) Из `managed_file` элемента 
  удалён суффикс `<span class="ajax-new-content"></span>`.

## JavaScript

- [#3086931](https://www.drupal.org/node/3086931) Удалён файл `postcss.
  config.js`.

## Help Topics

- [#3086075](https://www.drupal.org/node/3086075) Twig синтаксис, не 
  являющийся частью самой разметки (пример для документации), внутри Help 
  Topics теперь обрабатывается корректно.
- [#3281439](https://www.drupal.org/node/3281439) Тесты модуля больше не используют темы оформления Bartik и Seven.

## Media

- [#3255887](https://www.drupal.org/node/3255887) Исправлена ошибка в 
  конструкторе `MediaThumbnailFormatter`.

## MySQL

- [#3296108](https://www.drupal.org/node/3296108) `mysql_requirements()` 
  теперь производит проверки, только если подключение использует MySQL.

## Node

- [#1198120](https://www.drupal.org/node/1198120) Добавлены описания для 
  прав доступа «Edit own *».
- [#3264987](https://www.drupal.org/node/3264987) Исправлено двойное 
  присвоение переменной в `NodeViewsData`.

## Render System

- [#3117291](https://www.drupal.org/node/3117291) Внесены улучшения в метод 
  `Element::isEmpty()`.

## Shortcut

- [#3281440](https://www.drupal.org/node/3281440) Тесты модуля больше не 
  используют темы оформления Bartik и Seven.

## Theme System

- [#3285131](https://www.drupal.org/node/3285131) Инициализация `Drupal\Core\Theme\Registry::__construct()` без передачи параметра `$runtime_cache` объявлена устаревшей.

## Tracker

- [#3267314](https://www.drupal.org/node/3267314) Добавлены тесты для миграций покрывающие удаления модуля Tracker.

## Quick Edit

- [#3267258](https://www.drupal.org/node/3267258) Функционал интеграции модуля Quick Edit с модулем Editor, перенесён непосредственно в Quick Edit.
- [#3291047](https://www.drupal.org/node/3291047) Функционал интеграции модуля CKEditor 4/5 с модулем Editor, перенесён непосредственно в Quick Edit.
- [#3291018](https://www.drupal.org/node/3291018) `CKEditor5QuickEditLibraryTest` перенесён в Quick Edit.

## Update

- [#3279279](https://www.drupal.org/node/3279279) Удалена ссылка «Скачать» со страницы доступных обновлений.

## User

- [#2563995](https://www.drupal.org/node/2563995) Оптимизирован код в методе 
  `RoleStorage::isPermissionInRoles()`.

## Views

- [#3295813](https://www.drupal.org/node/3295813) Добавлено отсутствующее объявление свойства `ViewsEntitySchemaSubscriber::$viewsToSave`.

## Views UI

- [#2473877](https://www.drupal.org/node/2473877) Исправлена неполадка, 
  из-за которой индикатор прогресса оформлялся как пагинация и находился в 
  неположенном месте.

## Стандартный установочный профиль

- [#3243121](https://www.drupal.org/node/3243121) Профиль больше не устанавливает модуль RDF.

## Тестирование

- [#3281535](https://www.drupal.org/node/3281535) Внесены исправления для «Access to an undefined property».

## Прочие изменения

- [#3279192](https://www.drupal.org/node/3279192) Внесены улучшения в `DrupalKernel::handle()` для того чтобы Drupal мог работать со Swoole.
- [#3227431](https://www.drupal.org/node/3227431) Иконка для Tabledrag API теперь адаптируется по режим `forced-colors`.
- [#3276108](https://www.drupal.org/node/3276108) Добавлена возможность указывать заголовок для блока RSS.
- [#3257485](https://www.drupal.org/node/3257485) Удалён лишний код в `TestDiscoveryTest`.
- [#3153852](https://www.drupal.org/node/3153852) `ContextAwarePluginBase` 
  объявлен устаревшим.
- [#3291283](https://www.drupal.org/node/3291283) Восстановлен файл 
  `backbone-min.js.map`.
- [#3293215](https://www.drupal.org/node/3293215) Удалены остатки от Simpletest.
- [#3112290](https://www.drupal.org/node/3112290) Заменено использование 
  `REQUEST_TIME` в процедурном коде.
- [#3294076](https://www.drupal.org/node/3294076) Удалён неиспользуемый 
  сервис `exception.test_site`.
- [#3275585](https://www.drupal.org/node/3275585) Исправлены ошибки в 
  реализации `::getInstance()` для классов `FormatterPluginManager` и 
  `WidgetPluginManager`.
- [#3295157](https://www.drupal.org/node/3295157) Исправлены ошибки PHPStan 
  «Access to an undefined property».
- [#3281452](https://www.drupal.org/node/3281452) Тесты ядра больше не 
  используют темы оформления Bartik и Seven.
- [#3252587](https://www.drupal.org/node/3252587) `Xss` теперь не будет 
  проверять CSS классы с префиксом `:`.
- [#3267900](https://www.drupal.org/node/3267900) Внесены улучшения в 
  `ComposerProjectTemplatesTest`.
- [#3281454](https://www.drupal.org/node/3281454) Из тестов различных 
  модулей ядра удалено использование темы оформления Bartik и Seven.
- [#3295650](https://www.drupal.org/node/3295650) Из `example.settings.local.
  php` убрана строка с `\Drupal\Component\Assertion\Handle::register()`.
- [#3278124](https://www.drupal.org/node/3278124) Из тестов различных
  модулей ядра удалено использование темы оформления Bartik и Seven.
- [#3295709](https://www.drupal.org/node/3295709) Удалены стили для 
  неиспользуемого селектора `.views-progress-indicator`.
- [#3283602](https://www.drupal.org/node/3283602) В комментариях удалены 
  повторы слов.
- [#3292908](https://www.drupal.org/node/3292908) Быстрые 404 ответы теперь 
  быстрее регулярных.
- [#2852361](https://www.drupal.org/node/2852361) Улучшена обработка дублирующихся
  слешей для входящих запросов.
- [#3281449](https://www.drupal.org/node/3281449) Unit тесты ядра больше не
  используют тему оформления Bartik и Seven.
- [#3119840](https://www.drupal.org/node/3119840) Улучшены настройки `.gitattributes` для того чтобы нестандартные PHP файлы имели подсветку синтаксиса.