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

## Добавлена новая AJAX команда `add_js`

- [#1988968](https://www.drupal.org/node/1988968)

Добавлена новая AJAX команда `add_js` которая позоволяет дождаться загрузки всех необходимых JavaScript файлов прежде чем выполнять следующую команду \ код.

Это изменение меняет порядок вызовов AJAX команд. Ранее, добавляя JavaScript файлы через AJAX, команды идущие за командой добавления JavaScript не ожидали загрузки этих скриптов и сразу выполнялись. Это приводило к проблемам когда код вызывается прежде чем его зависимости доступны для использования.

## Тема оформления Bartik объявлена устаревшей

- [#3249109](https://www.drupal.org/node/3249109)

Тема оформления Bartik объявлена устаревшей и не будет включена в поставку Drupal 10. Основная тема по умолчанию в 
Drupal 10 — Olivero. Если вы используете Bartik, рекомендуется перейти на Olivero.

Если вам по прежнему необходим Bartik, запросите сторонний проект где будет продолжена поддержка и развитие данный темы
оформления — `composer require drupal/bartik`.

### Изменения API

AJAX команды теперь могут возвращать Promise, который дожидается выполнения предыдущей команды прежде чем перейти к следующей на очереди.

```javascript
// This command will wait for one second. The command execution queue will be 
// paused until the promise is resolved to continue.
Drupal.AjaxCommands.prototype.delayedCommand = function (ajax, response) {
  return new Promise((resolve, reject) => {
    setTimeout(resolve, 1000);
  });
}
```

`Drupal.Ajax.prototype.success` теперь возвращает Promise — если ваш код перезаписывает данный метод, убедитесь что он тоже возвращает Promise. В общем случае это решается следующим образом:

```javascript
const oldSuccess = Drupal.Ajax.prototype.success;
Drupal.Ajax.prototype.success = function (response, status) {
  // Custom processing here

  // Make sure that this method returns a Promise.
  return oldSuccess.call(this, response, status);
};
```

## Модуль Quick Edit объявлен устаревшим

- [#3270434](https://www.drupal.org/node/3270434)

Модуль Quick Edit объявлен устаревшим и будет удалён в Drupal 10. Модуль 
можно безопасно удалить без какого-либо влияния на сайт.

Многие пользователи Drupal не используют Quick Edit, поскольку он имеет 
целый ряд проблем, связанных с удобством использования, поэтому подумайте, 
возможно удаление данного модуля — лучшее решение.

Если вы хотите продолжать использовать Quick Edit, вы можете добавить 
сторонний модуль [Quick Edit](https://www.drupal.org/project/quickedit), 
чтобы продолжить использование на Drupal 10.x.

## Библиотека `farbtastic` объявлена устаревшей

- [#3306208](https://www.drupal.org/node/3306208)

Библиотека `core/jquery.farbtastic` объявлена устаревшей и будет удалена в 
Drupal 10.

## Функция Twig dump() теперь использует Symfony VarDumper (если доступен)

- [#3306864](https://www.drupal.org/node/3306864)

Встроенная в Twig функция `dump()` теперь будет отдавать предпочтение Symfony 
VarDumper для форматирования вывода, если данный пакет доступен.

Вы можете запросить данную зависимость для разработки: `composer require --dev symfony/var-dumper`.

## Модуль RDF объявлен устаревшим

- [#3267703](https://www.drupal.org/node/3267703)

Модуль RDF объявлен устаревшим и будет удалён в Drupal 10.

Модуль RDF устанавливался по умолчанию при использовании стандартного 
установочного профиля, но многие сайты не использовали функционал 
предоставляемый данным модулем. Если вы не знаете что это за модуль, скорее 
всего, вы можете отключить без последствий.

Если вам нужен данный модуль в Drupal 10, рекомендуется использовать проект 
`drupal/rdf` где будет продолжена поддержка данного модуля.

## Библиотека PopperJS объявлена устаревшей

- [#3307471](https://www.drupal.org/node/3307471)

Библиотека `core/popperjs` объявлена устаревшей и будет удалена в Drupal 10.
Данная библиотека использовалась в Drupal только модулем QuickEdit, который также
объявлен устаревшим.

Никакой замены в ядре не предоставляется. Если вы пользовались данной библиотекой,
рекомендуется перейти на [Floating UI](https://floating-ui.com/).

## CKEditor 4 объявлен устаревшим, CKEditor 5 — новый стабильный редактор

- [#3307186](https://www.drupal.org/node/3307186), [#3304326](https://www.drupal.org/node/3304326)

CKEditor 4 объявлен устаревшим и будет удалён в Drupal 10. Если вы желаете
продолжать использовать данный модуль на Drupal 10, запросите его как сторонний
модуль `composer require drupal/ckeditor`.

CKEditor 5 на 100% совместим с функционалом предоставляемым модулем CKEditor 4.
Все данные будут корректно смигрированы при переходе на новую версию 
автоматически. Если вы использовали какие-либо сторонние плагины, вы можете
потерять их функционал (но это не повлияет на сами данные).

За более подробной информацией обратитесь к [руководству по обновлению с CKEditor 4 на CKEditor 5](https://www.drupal.org/docs/core-modules-and-themes/core-modules/ckeditor-5-module/upgrading-modules-extending-ckeditor-4-to-support-ckeditor-5).

## Тема оформления Classy заменена генератором Starterkit темы

- [#3307241](https://www.drupal.org/node/3307241)

В Drupal 10, тема оформления Classy будет полностью заменена новым генератором
Starterkit темы. Данный генератор рекомендуется для создания новых тем 
оформления, вместо того чтобы использовать Classy в качестве базовой темы.

Если ваша тема расширяет Classy и вы не можете на данный момент отказаться от
использования её в качестве базовой, вы можете запросить стороннюю тему 
`composer require drupal/classy`, которая будет поддерживаться в качестве
независимой.

Если вы хотите отвязать свою тему от использования Classy в качестве базовой,
обратитесь к [инструкции на данной странице](https://www.drupal.org/node/3305674).

## Библиотека Shepherd.js объявлена устаревшей

- [#3308786](https://www.drupal.org/node/3308786)

Библиотека Shepherd.js объявлена устаревшей и помечена для внутреннего 
использования. Замена данной библиотеке не предоставляется.

## Тема оформления Stable объявлена устаревшей

- [#3308890](https://www.drupal.org/node/3308890)

Тема оформления Stable объявлена устаревшей и будет удалена в Drupal 10. В
качестве замены предоставлен функционал генерации стартовой темы.

Если ваша тема зависит от Stable, замените зависимость на Stable9. Для более
подробной информации обратитесь к [инструкции на данной странице](https://www.drupal.org/node/3309392).

## Добавлена возможность отладки кеш-метаданных при помощи HTML комментариев

- [#2381797](https://www.drupal.org/node/2381797)

В настройки рендера добавлена новая опция `debug` которая позволяет включить
отладку кеш-метаданных в HTML комментариях.

Для активации данной опции добавьте `debug` в `render.config`:

```yaml
  renderer.config:
    required_cache_contexts: ['languages:language_interface', 'theme', 'user.permissions']
    auto_placeholder_conditions:
      max-age: 0
      contexts: ['session', 'user']
      tags: []
    debug: true
```

Пример результата дебага:

```html
<!-- START RENDERER -->
<!-- CACHE-HIT: No -->
<!-- CACHE TAGS:
   * node:1
   * node_view
   * user:1
   * user_view
-->
<!-- CACHE CONTEXTS:
   * languages:language_interface
   * theme
   * timezone
   * url.site
   * user.permissions
   * user.roles
   * user.roles:anonymous
   * user.roles:authenticated
-->
<!-- CACHE KEYS:
   * entity_view
   * node
   * 1
   * full
-->

<article data-history-node-id="1" data-quickedit-entity-id="node/1" role="article" class="contextual-region node node--type-page node--
...
</article>

<!-- END RENDER -->
```

## Добавлен новый хук `hook_requirements_alter()`

- [#309040](https://www.drupal.org/node/309040), [#3316871](https://www.drupal.org/node/3316871)

Добавлен новый хук `hook_requirements_alter()` при помощи которого вы можете
править проверки и требования для сторонних модулей. Например, его можно
использовать для:

- Изменений заголовков, значений, описаний или даже уровня важности которые были
  объявлены в оригинальном `hook_requirements()`.
- Удалять записи объявленные в `hook_requirements()`.

Вы также можете добавлять новые требования при помощи данного хука, но данное
применение не рекомендуется. Для объявления новых требований используйте `hook_requirements()`.

Пример использования нового хука:

```php
function mymodule_requirements_alter(array &$requirements): void {
  // Change the title from 'PHP' to 'PHP version'.
  $requirements['php']['title'] = t('PHP version');

  // Decrease the 'update status' requirement severity from warning to warning.
  $requirements['update status']['severity'] = REQUIREMENT_INFO;

  // Remove a requirements entry.
  unset($requirements['foo']);
}
```

## Big Pipe

- [#3294720](https://www.drupal.org/node/3294720) `Drupal.attachBehaviors()` 
  теперь вызывается на элементы до окончания загрузки.

## Block

- [#3281429](https://www.drupal.org/node/3281429) Тесты, не использующие миграции, обновлены чтобы не использовать темы Bartik и Seven.
- [#3105880](https://www.drupal.org/node/3105880) Из теста `BlockRepositoryTest` удалён неиспользуемый код.
- [#3061148](https://www.drupal.org/node/3061148) Исправлена неполадка, из-за которой заголовки отключеных блоков проходили через двойное экранирование.
- [#3301663](https://www.drupal.org/node/3301663) Из метода `\Drupal\block\BlockForm::buildVisibilityInterface()` 
  удалена проверка устаревшего условия `node_type`.
- [#2779321](https://www.drupal.org/node/2779321) Исправлена неполадка из-за которой сохранение пустой структуры блоков могло привести к поломке всех блок сущностей.

## Block Content

- [#3053881](https://www.drupal.org/node/3053881) Исправлена неполадка, из-за которой возврат старой ревизии с 
  использованием собственных блоков могло приводить к вызову `EntityChangedConstraint`.
- [#3307182](https://www.drupal.org/node/3307182) smustgrave добавлен в качестве мейнтейнера `block_content`.
- [#1937266](https://www.drupal.org/node/1937266) В сущности `BlockContent` метод `::delete()` заменён на `::preDelete()`.

## Book

- [#3303033](https://www.drupal.org/node/3303033) Тесты модуля больше не используют тему оформления Classy.

## Claro

- [#3291100](https://www.drupal.org/node/3291100) Улучшено отображение `details` элемента.
- [#3085219](https://www.drupal.org/node/3085219) Улучшено оформление процесса установки сайта.
- [#3300941](https://www.drupal.org/node/3300941) Исправлена неполадка в выравнивании лейаута Views UI.

## CKEditor 4

- [#3271057](https://www.drupal.org/node/3271057) Интеграция Media Library с 
  CKEditor 4 перенесена в модуль CKEditor 4.
- [#3271094](https://www.drupal.org/node/3271094) Интеграция с Media 
  перенесена из одноимённого модуля в CKEditor.
- [#3285049](https://www.drupal.org/node/3285049) Внесены улучшения в стили для редактора.

## CKEditor 5

- [#3292626](https://www.drupal.org/node/3292626) Удалён файл стилей 
  `core/modules/ckeditor5/css/quickedit.css`.
- [#3294908](https://www.drupal.org/node/3294908) Внесены улучшения в проверку на доступные классы 
  `StyleSensibleElementConstraintValidator`.
- [#3231336](https://www.drupal.org/node/3231336) Внесены улучшения в `HtmlRestrictions.`
- [#3314770](https://www.drupal.org/node/3314770) (отменено) Добавлена сортировка настроек в файлах конфигураций.
- [#3317332](https://www.drupal.org/node/3317332) Удалён неактуальный `README.md`.
- [#3315884](https://www.drupal.org/node/3315884) Исправлены опечатки в коде модуля.
- [#3317330](https://www.drupal.org/node/3317330) Исправлены случайные провалы теста `ImageTest::testAltTextRequired()`.
- [#3273532](https://www.drupal.org/node/3273532) В случае возникновении ошибок в `ckeditor5.js` теперь в консоль выводится сообщение для разработчиков, как провести отладку проблемы.

## Colors

- [#3281430](https://www.drupal.org/node/3281430) Тесты модуля, не связанные с миграциями, больше не используют Bartik.

## Comment

- [#2958241](https://www.drupal.org/node/2958241) Внесены улучшения в 
  `CommentSelection` для корректной работы комментариев с различными типами 
  сущностей.
- [#3308912](https://www.drupal.org/node/3308912) Улучшена производительность теста `CommentEditTest`.
- [#2254175](https://www.drupal.org/node/2254175) Улучшена производительность теста `CommentPreviewTest`.

## Composer

- [#3280399](https://www.drupal.org/node/3280399) Пакет `drupal/core-bridge:9.5.x` объявлен заброшенным.
- [#3280589](https://www.drupal.org/node/3280589) Скрипт для чистки vendor 
  директории объявлен устаревшим.
- [#3298343](https://www.drupal.org/node/3298343) Зависимость `egulias/email-validator` обновлена до версии 3.2.1.
- [#3299213](https://www.drupal.org/node/3299213) Зависимость `mikey179/vfsstream` теперь запрашивается версии ^1.6.11.
- [#3295520](https://www.drupal.org/node/3295520) Зависимости ядра обновлены на 08.08.2022.
- [#3272110](https://www.drupal.org/node/3272110) `composer.json` файлы компонентов `Drupal\Component` приведены в 
  порядок и актуализированы.
- [#3306946](https://www.drupal.org/node/3306946) Зависимости ядра обновлены на 01.09.2022.
- [#3317873](https://www.drupal.org/node/3317873) Зависимости ядра обновлены на 15.11.2022.
- [#3269457](https://www.drupal.org/node/3269457) Зависимость `laminas/escaper` обновлена до версии 2.10.0.
- [#3324544](https://www.drupal.org/node/3324544) Зависимости ядра обновлены на 30.11.2022.

## Configuration System

- [#3295735](https://www.drupal.org/node/3295735) Улучшена работа теста `ConfigImportUITest` с темой Olivero.
- [#2232051](https://www.drupal.org/node/2232051) Запись конфигурации в файл больше не вызывает `chmod()`.

## Database Log

- [#2888872](https://www.drupal.org/node/2888872) Форма фильтрации больше не отображается если нет логов.

## Database System

- [#3312439](https://www.drupal.org/node/3312439) Исправлены тайпхинты в `@return` для `StatementInterface`.
- [#3231950](https://www.drupal.org/node/3231950) Тесты связанные с БД были разделены на тесты «ядра» (API) и драйверов баз данных.
- [#3320240](https://www.drupal.org/node/3320240) «count» запросы теперь возвращают `int` вместо `string`.

## Dependency Injection

- [#2531564](https://www.drupal.org/node/2531564) Улучшена сериализация контейнера.
- [#3310271](https://www.drupal.org/node/3310271) Улучшена сериализация строковых сервисов.
- [#3320315](https://www.drupal.org/node/3320315) Добавлена поддержка названий сервисов в верхнем регистре.

## Editor

- [#3306715](https://www.drupal.org/node/3306715) Для `editor_private_test` зависимость от `ckeditor` заменена на `editor_test`.

## Entity API

- [#3266589](https://www.drupal.org/node/3266589) Удалён заголовок `Link` из ответа страницы сущности.
- [#3311562](https://www.drupal.org/node/3311562) (отменено) В классы `Condition` и `ConditionAggregate` добавлено свойство `$sqlQuery`.
- [#3266287](https://www.drupal.org/node/3266287) Классы бандлов теперь могут объявлять свои собственные поля в `::bundleFieldDefinitions()`.
- [#3067024](https://www.drupal.org/node/3067024) Добавлен тест для проверки удаления ревизионной сущности для которой больше нет кодовой базы.

## Field

- [#3299024](https://www.drupal.org/node/3299024) `EntityReferenceFormatterTest` теперь использует `::label()` вместо `->name->value`.
- [#3269000](https://www.drupal.org/node/3269000) Улучшена документация `TestFieldWidgetMultiple::isApplicable()`.

## Field Layout

- [#3292560](https://www.drupal.org/node/3292560) Тесты модуля больше не используют тему оформления Classy.

## File

- [#2588013](https://www.drupal.org/node/2588013) Из `managed_file` элемента 
  удалён суффикс `<span class="ajax-new-content"></span>`.
- [#3189876](https://www.drupal.org/node/3189876) Улучшена документация для плагинов миграций.
- [#59750](https://www.drupal.org/node/59750) Элемент формы `file` теперь корректно обрабатывает `#required` свойство.
- [#2867336](https://www.drupal.org/node/2867336) Улучшена валидация размера файла.
- [#3279289](https://www.drupal.org/node/3279289) `\file_requirements()` теперь предоставляет значение по умолчанию для `SERVER_SOFTWARE`.

## Form API

- [#2965929](https://www.drupal.org/node/2965929) Ошибка «The form argument is not a valid form.» замена на две новых, одну на случай если класс формы не найден, а вторую, если форма не расширяет интерфейс.

## Forum

- [#2774399](https://www.drupal.org/node/2774399) Улучшена проверка на возможность удаления модуля.
- [#3196619](https://www.drupal.org/node/3196619) Удалено использование `forum_controller` при создании нового «форума».

## JavaScript

- [#3086931](https://www.drupal.org/node/3086931) Удалён файл `postcss.
  config.js`.
- [#3296096](https://www.drupal.org/node/3296096) Сообщение о депрекации `:tabbable` обновлено. JavaScript код будет удалён в Drupal 11.
- [#3307713](https://www.drupal.org/node/3307713) Удалена зависимость `@ckeditor/ckeditor5-dev-utils`.
- [#3306182](https://www.drupal.org/node/3306182) JavaScript зависимости обновлены на 07.09.2022.
- [#3306441](https://www.drupal.org/node/3306441) Зависимость `cspell` обновлена до версии 6.8.1.
- [#3308780](https://www.drupal.org/node/3308780) Библиотека glob обновлена до версии 8.0.3.
- [#3308781](https://www.drupal.org/node/3308781) Библиотека jsdom обновлена до версии 20.0.0.
- [#3308826](https://www.drupal.org/node/3308826) Библиотека eslint-plugin-yml обновлена до версии 1.2.0.
- [#3261163](https://www.drupal.org/node/3261163) PostCSS обновлён до 8 версии.
- [#3308821](https://www.drupal.org/node/3308821) Библиотека stylelint-config-standard обновлена до версии 28.0.0
- [#3317882](https://www.drupal.org/node/3317882) Зависимость `styleint` обновлена до версии 14.14.1, а также обновлена `stylint-config-standard` до 29.0.0.
- [#3319917](https://www.drupal.org/node/3319917) Удалена зависимость `raw-loader`.
- [#3317885](https://www.drupal.org/node/3317885) Обновлены зависимости `jsdom`, `terser`, `minimist` и `underscore` на 02.11.2022.
- [#3320370](https://www.drupal.org/node/3320370) Библиотека `tabbable` обновлена до версии 6.0.1.
- [#3320515](https://www.drupal.org/node/3320515) Обновлены зависимости `cspell` и `nightwatch` на 11.11.2022.
- [#3317879](https://www.drupal.org/node/3317879) Удалена зависимость `chromedriver`.
- [#3320518](https://www.drupal.org/node/3320518) Обновлена зависимость `webpack` до версии 3.1.0.
- [#3321003](https://www.drupal.org/node/3321003) Обновлена зависимость Shepherd до версии 9.1.1.
- [#3321002](https://www.drupal.org/node/3321002) Обновлена зависимость Babel до версии 7.20.2.
- [#3324378](https://www.drupal.org/node/3324378) Зависимости ядра обновлены на 01.12.2022.
- [#3324723](https://www.drupal.org/node/3324723) Зависимость `cspell` обновлена до версии 6.15.1.
- [#3325114](https://www.drupal.org/node/3325114) Зависимости `cspell`, `eslint`, `postcss-import`, `styleint`, `terser` и `webpack-cli` обновлены до последних актуальных версий на 06.12.2022.
- [#3325517](https://www.drupal.org/node/3325517) Зависимость `vm2` обновлена до версии 3.9.12.
- [#3326874](https://www.drupal.org/node/3326874) Библиотека jQuery обновлена до версии 3.6.2.
- [#3326896](https://www.drupal.org/node/3326896) Библиотека CKEditor обновлена до версии 35.4.0.

## JSON:API

- [#3127883](https://www.drupal.org/node/3127883) Улучшено сообщение об ошибке, если поле указано с опечаткой.
- [#3213752](https://www.drupal.org/node/3213752) Удалён неиспользуемый код в `JsonApiDocumentTopLevelNormalizerTest`.

## Help Topics

- [#3086075](https://www.drupal.org/node/3086075) Twig синтаксис, не 
  являющийся частью самой разметки (пример для документации), внутри Help 
  Topics теперь обрабатывается корректно.
- [#3281439](https://www.drupal.org/node/3281439) Тесты модуля больше не используют темы оформления Bartik и Seven.
- [#3204175](https://www.drupal.org/node/3204175) Добавлены ссылки на Security Advisories.
- [#3307697](https://www.drupal.org/node/3307697) Вызовы `url()` заменены на специальную функцию `help_route_link()`.
- [#3307691](https://www.drupal.org/node/3307691) Вызовы `url()` заменены на специальную функцию `help_route_link()`.
- [#3307700](https://www.drupal.org/node/3307700) Вызовы `url()` заменены на специальную функцию `help_route_link()`.
- [#3307692](https://www.drupal.org/node/3307692) Вызовы `url()` заменены на специальную функцию `help_route_link()`.
- [#3307694](https://www.drupal.org/node/3307694) Вызовы `url()` заменены на специальную функцию `help_route_link()`.
- [#3307696](https://www.drupal.org/node/3307696) Вызовы `url()` заменены на специальную функцию `help_route_link()`.

## Image

- [#3291622](https://www.drupal.org/node/3291622) Внесены улучшения в код `ImageUrlFormatter`.

## Language

- [#3316136](https://www.drupal.org/node/3316136) Улучшена документация для `LanguageNegotiationInterface`.

## Layout Builder

- [#2935999](https://www.drupal.org/node/2935999) Модуль больше не требует включения модуля Field UI.
- [#3111192](https://www.drupal.org/node/3111192) Если для макета была использована сущность, она добавляется в переменную шаблона `entity`.
- [#3044117](https://www.drupal.org/node/3044117) В `ConfigureSectionForm` добавлены методы для получения информации о текущем макете.
- [#3315490](https://www.drupal.org/node/3315490) Исправлены случайные провалы тестов `InlineBlockPrivateFilesTest`.

## Media

- [#3255887](https://www.drupal.org/node/3255887) Исправлена ошибка в 
  конструкторе `MediaThumbnailFormatter`.
- [#3275807](https://www.drupal.org/node/3275807) Тесты модуля больше не используют тему Classy.
- [#3310510](https://www.drupal.org/node/3310510) Улучшена ошибка в OEmbed плагине, если загрузка превью завершилась ошибкой.

## Media Library

- [#3278418](https://www.drupal.org/node/3278418) Тесты модуля больше не используют тему оформления Classy.

## Menu

- [#3321955](https://www.drupal.org/node/3321955) Улучшена проверка кеш-контекста для теста `DefaultMenuLinkTreeManipulatorsTest`.

## Migrate

- [#2919158](https://www.drupal.org/node/2919158) В плагин маппинга ID `sql` добавлена зависимость `MigrationPluginManager`.
- [#3254986](https://www.drupal.org/node/3254986) (отменено) `RequirementsException` больше не выводит массив требований в сообщении.

## Migrate Drupal

- [#3219539](https://www.drupal.org/node/3219539) Обновлена фикстура для Drupal 7.
- [#3176393](https://www.drupal.org/node/3176393) В `DrupalSqlBase::checkRequirements()` теперь используется `SourcePluginBase::getSourceModule()`.

## MySQL

- [#3296108](https://www.drupal.org/node/3296108) `mysql_requirements()` 
  теперь производит проверки, только если подключение использует MySQL.
- [#3311474](https://www.drupal.org/node/3311474) Проверка на совместимость с `translaction_isolation` теперь проверяет на корректную версию MySQL.

## Node

- [#1198120](https://www.drupal.org/node/1198120) Добавлены описания для 
  прав доступа «Edit own *».
- [#3264987](https://www.drupal.org/node/3264987) Исправлено двойное 
  присвоение переменной в `NodeViewsData`.
- [#3303278](https://www.drupal.org/node/3303278) Тесты связанные с модулем RDF перенесены в одноимённый модуль.
- [#3316813](https://www.drupal.org/node/3316813) Удалены упоминания `node_access_example.module`.

## Olivero

- [#3302052](https://www.drupal.org/node/3302052) Исправлена неполадка, 
  из-за которой выпадающий список `dropbutton` мог перекрываться подвалом темы.

## RDF

- [#3293814](https://www.drupal.org/node/3293814) Help Topic связанные с модулем перенесены непосредственно в RDF модуль.
- [#3303435](https://www.drupal.org/node/3303435) Опциональные конфигурации связанные с модулем перенесены в модуль RDF.
- [#3307230](https://www.drupal.org/node/3307230) Внесены улучшения в интеграцию модуля и Views.

## Render System

- [#3117291](https://www.drupal.org/node/3117291) Внесены улучшения в метод 
  `Element::isEmpty()`.

## REST

- [#3082053](https://www.drupal.org/node/3082053) Из теста `EntityResourceTestBase` удалён код тестирующий кеш-прослойку.

## Search

- [#3275843](https://www.drupal.org/node/3275843) Тесты модуля больше не используют тему оформления Classy.

## Seven

- [#3303787](https://www.drupal.org/node/3303787) Опциональная конфигурация `block.block.seven_help_search.yml` 
  перенесена из Help Topics модуля в тему.

## Shortcut

- [#3281440](https://www.drupal.org/node/3281440) Тесты модуля больше не 
  используют темы оформления Bartik и Seven.

## Standard Profile

- [#3271097](https://www.drupal.org/node/3271097) Стандартный профиль теперь 
  использует CKEditor5 в качестве редактора по умолчанию.

## System

- [#3281434](https://www.drupal.org/node/3281434) Тесты модуля больше не используют Bartik и Seven.
- [#2787529](https://www.drupal.org/node/2787529) Добавлена схема конфигурации для плагина-условия `current_theme`.

## Taxonomy

- [#3305410](https://www.drupal.org/node/3305410) Удалён тест `TaxonomyImageTest`.
- [#3117069](https://www.drupal.org/node/3117069) Исправлена неполадка в `OverviewTerms`, из-за которой могла появляться ошибка несуществующего индекса.
- [#2826592](https://www.drupal.org/node/2826592) После изменения существующего термина таксономии пользователь будет перенаправлен на страницу термина.
- [#3317758](https://www.drupal.org/node/3317758) `Term::getWeight()` теперь всегда возвращает `int`.

## Telephone

- [#2862922](https://www.drupal.org/node/2862922) Добавлена новая константа `TelephoneItem::MAX_LENGTH` с макс длиной для значения (256) и теперь используется вместо фиксированного значения.

## Theme System

- [#3285131](https://www.drupal.org/node/3285131) Инициализация `Drupal\Core\Theme\Registry::__construct()` без передачи параметра `$runtime_cache` объявлена устаревшей.

## Tracker

- [#3267314](https://www.drupal.org/node/3267314) Добавлены тесты для миграций покрывающие удаления модуля Tracker.

## Quick Edit

- [#3267258](https://www.drupal.org/node/3267258) Функционал интеграции модуля Quick Edit с модулем Editor, перенесён непосредственно в Quick Edit.
- [#3291047](https://www.drupal.org/node/3291047) Функционал интеграции модуля CKEditor 4/5 с модулем Editor, перенесён непосредственно в Quick Edit.
- [#3291018](https://www.drupal.org/node/3291018) `CKEditor5QuickEditLibraryTest` перенесён в Quick Edit.
- [#3292780](https://www.drupal.org/node/3292780) Код связанный с модулем и CKEditor 5 перенесён в модуль Quick Edit.

## Update

- [#3279279](https://www.drupal.org/node/3279279) Удалена ссылка «Скачать» со страницы доступных обновлений.

## Umami

- [#3302984](https://www.drupal.org/node/3302984) Установочный профиль больше не используем модуль RDF.
- [#3273665](https://www.drupal.org/node/3273665) Установочный профиль теперь использует CKEditor 5 в качестве 
  редактора.
- [#3304655](https://www.drupal.org/node/3304655) Исправлена неполадка в библиотеке `demo-umami-tour` которая по
  ошибке имела зависимость на библиотеку из темы оформления `seven`.
- [#2960583](https://www.drupal.org/node/2960583) В тему оформления `umami` добавлены шрифты используемые темой.

## User

- [#2563995](https://www.drupal.org/node/2563995) Оптимизирован код в методе 
  `RoleStorage::isPermissionInRoles()`.
- [#3282420](https://www.drupal.org/node/3282420) При использовании одноразовой ссылки для входа теперь предлагается «установить», а не «менять» пароль. Это изменение касается текста сообщения.
- [#3285593](https://www.drupal.org/node/3285593) Улучшена документация для `Section::getComponents()`.
- [#3110761](https://www.drupal.org/node/3110761) Улучшено описание email поля пользователя.
- [#3168624](https://www.drupal.org/node/3168624) Добавлен новый маршрут `user.edit` (`user/edit`) для открытия формы редактирования текущего пользователя.
- [#3319845](https://www.drupal.org/node/3319845) `user_post_update_update_migrated_roles_followup()` перенесён в Drupal 10 как `user_update_10000()`.

## Views

- [#3295813](https://www.drupal.org/node/3295813) Добавлено отсутствующее объявление свойства `ViewsEntitySchemaSubscriber::$viewsToSave`.
- [#2796045](https://www.drupal.org/node/2796045) Улучшено сообщение об ошибки при добавлении поля связью когда эта связь недоступна.
- [#2568889](https://www.drupal.org/node/2568889) (отменено) Исправлена неполадка, из-за которой раскрытый фильтр для текстового поля с обязательным значением мог показывать пустую ошибку.
- [#3309750](https://www.drupal.org/node/3309750) Исправлена неполадка с синтаксисом `callable` для PHP 8.2.
- [#3311466](https://www.drupal.org/node/3311466) Удалено свойство `ViewExecutable::$editing`.
- [#3299800](https://www.drupal.org/node/3299800) Исправлена неполадка в `CachePluginBase`, из-за которой могла не работать часть функционала если запросы был задекорирован.
- [#3230681](https://www.drupal.org/node/3230681) `ViewsExposedFilterBlock::build()` теперь всегда возвращает массив в качестве результата.
- [#2864115](https://www.drupal.org/node/2864115) Упрощен лейбл с «Create CSS class» на «Add HTML class».
- [#3266243](https://www.drupal.org/node/3266243) Из плагинов модуля удалены вызовы `trigger_error()`.
- [#3259090](https://www.drupal.org/node/3259090) Исправлена неполадка со сравнением значений на PHP 8.0.
- [#2810985](https://www.drupal.org/node/2810985) Удалены повторные проверки.

## Views UI

- [#2473877](https://www.drupal.org/node/2473877) Исправлена неполадка, 
  из-за которой индикатор прогресса оформлялся как пагинация и находился в 
  неположенном месте.
- [#2313073](https://www.drupal.org/node/2313073) Исправлена неполадка, из-за которой не работала передача «0» в 
  качестве контекстного фильтра.
- [#2975616](https://www.drupal.org/node/2975616) Раздел «Relationships» теперь находится над «Contextual Filters»

## Стандартный установочный профиль

- [#3243121](https://www.drupal.org/node/3243121) Профиль больше не устанавливает модуль RDF.
- [#3316921](https://www.drupal.org/node/3316921) Формат «Ограниченный HTML» больше не разрешает использовать `<span>`.

## Тестирование

- [#3281535](https://www.drupal.org/node/3281535) Внесены исправления для «Access to an undefined property».
- [#3302760](https://www.drupal.org/node/3302760) Добавлены фикстуры с БД Drupal 9.4.0.
- [#3313021](https://www.drupal.org/node/3313021) Удалён устаревший метод `KernelTestBase::prepareTemplate()`.
- [#3211992](https://www.drupal.org/node/3211992) Исправлено состояние гонки в `TestSiteApplicationTest::testInstallWithNonExistingFile()`.
- [#3293446](https://www.drupal.org/node/3293446) Kernel тесты теперь создают меньше статического кеша в `ExtensionDiscovery`.
- [#3183423](https://www.drupal.org/node/3183423) Удалён метод `\Drupal\FunctionalJavascriptTests\DrupalSelenium2Driver::attachFile()`.
- [#3256356](https://www.drupal.org/node/3256356) Улучшена интеграция браузерных тестов с Xdebug 3.
- [#3317504](https://www.drupal.org/node/3317504) Из `run-tests.sh` удалено упоминание что можно запускать тест конкретного метода.
- [#3259751](https://www.drupal.org/node/3259751) Добавлена возможность передачи аргументов в `chromedriver` при помощи env переменной `DRUPAL_TEST_WEBDRIVER_CLI_ARGS`.

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
- [#3281427](https://www.drupal.org/node/3281427) Миграции модулей Block и System больше не используют темы оформления Bartik и Seven.
- [#2967627](https://www.drupal.org/node/2967627) Исправлена неполадка при формировании кеша препроцессоров в Registry, которая могла приводить к временным дубликатам.
- [#3298319](https://www.drupal.org/node/3298319) `ExtensionDiscoveryTrait` больше не использует тему оформления Seven.
- [#3281444](https://www.drupal.org/node/3281444) Тесты для установочных профилей больше не используют Bartik и Seven.
- [#3262674](https://www.drupal.org/node/3262674) В качестве темы по умолчанию в режиме обслуживания теперь используется Claro.
- [#3281457](https://www.drupal.org/node/3281457) Тесты для Seven и Bartik перенесены в пространство имён 
  `FunctionalTests\Theme`.
- [#3303453](https://www.drupal.org/node/3303453) Тест `NoJavaScriptAnonymousTest` больше не использует модуль RDF.
- [#3223061](https://www.drupal.org/node/3223061) Улучшено отображение dropbutton по умолчанию с длинными названиями.
- [#3302800](https://www.drupal.org/node/3302800) Внесены корректировки в некоторые тесты со списком доступных тем.
- [#2937010](https://www.drupal.org/node/2937010) В `ContainerBuilder` портирован изменения из Symfony контейнера.
- [#3291797](https://www.drupal.org/node/3291797) Обновлены стили для 
  off-canvas элемента с использованием CSS переменных.
- [#3306720](https://www.drupal.org/node/3306720) `twig_theme_test` теперь 
  подключает библиотеки CKEditor 5.
- [#3304731](https://www.drupal.org/node/3304731) Оставшиеся тесты что 
  используют Classy, теперь используют Starterkit.
- [#3306897](https://www.drupal.org/node/3306897) Обновлены фикстуры с дампами БД Drupal 9.4.0.
- [#3307454](https://www.drupal.org/node/3307454) Тесты связанные с темой Classy перенесены непосредственно в тему.
- [#3308458](https://www.drupal.org/node/3308458) В `core.libraries.yml` внесены утерянные исправления из более новых веток.
- [#3308987](https://www.drupal.org/node/3308987) Удалены упоминания Stable темы.
- [#3274474](https://www.drupal.org/node/3274474) Исправлены ошибки PHPStan «Access to an undefined property».
- [#3308744](https://www.drupal.org/node/3308744) Улучшена работа с магическим свойством `connection` в `FileTransfer`.
- [#3298731](https://www.drupal.org/node/3298731) Исправлена неполадка с `ConstraintViolation::$arrayPropertyPath` для PHP 8.2.
- [#3310887](https://www.drupal.org/node/3310887) Mike Herchel 'mherchel' добавлен в качестве мейнтейнера CSS подсистемы ядра.
- [#3311383](https://www.drupal.org/node/3311383) Для PHP классов из ядра добавлен аттрибут `#[\AllowDynamicProperties]` чтобы уменьшить количество шума в логах на новых версиях PHP.
- [#3299853](https://www.drupal.org/node/3299853) Для базовых PHP классов из ядра добавлен аттрибут `#[\AllowDynamicProperties]`.
- [#3079404](https://www.drupal.org/node/3079404) В `.htaccess` добавлена пометка, что 301-й редирект не учитывает настройки Drupal и сохраняется на 2 недели по умолчанию.
- [#3311214](https://www.drupal.org/node/3311214) Внесены небольшие улучшения в `RecursiveContextualValidatorTest`.
- [#2254187](https://www.drupal.org/node/2254187) Улучшена производительность теста `NodeAccessBaseTableTest`.
- [#3087862](https://www.drupal.org/node/3087862) Исправлена неполадка в команде установки тестового сайта.
- [#2941148](https://www.drupal.org/node/2941148) Исправлены ошибки стандарта «Drupal.Commenting.FunctionComment.MissingReturnType».
- [#2937515](https://www.drupal.org/node/2937515) Исправлены ошибки стандартов «Fix Drupal.Array.Array.ArrayClosingIndentation, ArrayIndentation».
- [#3264947](https://www.drupal.org/node/3264947) Методам тестов `::setUp()` и `::tearDown()` добавлена документация.
- [#3309907](https://www.drupal.org/node/3309907) Исправлена неполадка в `WebAssert` приводящая к случайным провалам теста `BookTest::testGetTableOfContents`.
- [#3308162](https://www.drupal.org/node/3308162) `KernelTestBase::installConfig()` теперь перехватывает исключения и выбрасывает своё собственно с указанием проблемного модуля.
- [#3181778](https://www.drupal.org/node/3181778) Использование функции `t()` во всех плагинах заменено на `$this->t()`.
- [#2341553](https://www.drupal.org/node/2341553) В `AlreadyInstalledException` добавлена информация о том, почему невозможно использовать систему маршрутизации в процессе установки.
- [#3226139](https://www.drupal.org/node/3226139) В некоторых тестах `->will()` заменены на `->willReturnMap()`.
- [#3231694](https://www.drupal.org/node/3231694) Исправлен некорректный тип для `@return` в `TestFileCreationTrait`.
- [#3268829](https://www.drupal.org/node/3268829) Исправлены ошибки стандарта «Drupal.Commenting.DocComment.ShortSingleLine».
- [#3273317](https://www.drupal.org/node/3273317) В шаблоны по умолчанию `block*.html.twig` и `layout*.html.twig` добавлена переменная `in_preview`.
- [#3295880](https://www.drupal.org/node/3295880) В некоторых тестах `->will()` заменены на `->willReturn()`.
- [#3307972](https://www.drupal.org/node/3307972) Исправлено некорректное указание класса в `ModulesListForm`.
- [#3195033](https://www.drupal.org/node/3195033) Исправлена неполадка в стилях тем оформления с посещёнными ссылками, из-за которых это сказывалось на ссылках тулбара.
- [#3313435](https://www.drupal.org/node/3313435) «daffie» добавлен в качестве мейнтенера Database API.
- [#3271222](https://www.drupal.org/node/3271222) В стандартный robots.txt добавлены запреты на пути встроенных OEmbed элементов.
- [#2895352](https://www.drupal.org/node/2895352) Для элемента `tableselect` добавлена возможность отключения конкретных опций.
- [#3112452](https://www.drupal.org/node/3112452) В `.yml` файлах ядра исправлены некорректные отступы.
- [#3314353](https://www.drupal.org/node/3314353) Из `EntityTestRev` удалёно дублированное объявление view builder.
- [#3314523](https://www.drupal.org/node/3314523) Теперь, после обработки стилей при помощи PostCSS, запускается линтер для того чтобы конечные CSS файлы соответствовали стандартам Drupal.
- [#3316971](https://www.drupal.org/node/3316971) В `run-tests.sh` улучшена совместимость с будущими версиями PHP.
- [#3029782](https://www.drupal.org/node/3029782) Исправлена неполадка из-за которой `UniqueFieldValueValidator` в сообщении с ошибкой приводил лейбл поля к нижнему регистру.
- [#3318581](https://www.drupal.org/node/3318581) `ConfigImporter::doSyncStep()` теперь также может принимать `callable`.
- [#3312198](https://www.drupal.org/node/3312198) `fast_404` теперь учитывает что некоторые маршруты отвечают HTTP 404 в зависимости от разных условий и через исключение.
- [#3319780](https://www.drupal.org/node/3319780) longwave добавлен в MAINTAINERS.txt как временный менеджер релизов.
- [#3320483](https://www.drupal.org/node/3320483) Удалена неиспользуемая переменная `$pos` из `system.install`.
- [#3260401](https://www.drupal.org/node/3260401) Удалена настройка `block_interest_cohort` в связи с прекращением поддержки Google заголовка `Permissions-Policy`.
- [#3308369](https://www.drupal.org/node/3308369) `.htaccess` предоставляемый Drupal теперь блокирует доступ к `yarn.lock` и `package.json` файлам.
- [#3205578](https://www.drupal.org/node/3205578) Из репозитория удалён файл `/core/scripts/transliteration_data.php.txt`.
- [#3312089](https://www.drupal.org/node/3312089) `commit-code-check.sh` теперь запускает PHPCS в параллельном процессе.
- [#2514582](https://www.drupal.org/node/2514582) Добавлена документация по ленивым сервисам.
- [#3324540](https://www.drupal.org/node/3324540) Исправлены ошибки PHPCS приводящие к провалу проверки коммитов.
- [#3032746](https://www.drupal.org/node/3032746) Улучшена документация для настройки `reverse_proxy_addresses`.
- [#3279725](https://www.drupal.org/node/3279725) Для типа содержимого по умолчанию «Article» улучшено отображение по умолчанию (настройки полей).
- [#3255637](https://www.drupal.org/node/3255637) Передача `NULL` в качестве аргумента для `Html::escape()`, `Html::decodeEntities()` а также `FormattableMarkup::placeholderEscape()` объявлено устаревшим.
- [#3324801](https://www.drupal.org/node/3324801) Исправлены ошибки PHPStan L2 «Свойство Foo::$bar имеет неизвестный тип Baz».
- [#3324062](https://www.drupal.org/node/3324062) Исправлена регрессия в `Drupal.ajax`.
- [#3266006](https://www.drupal.org/node/3266006) Актуализирован `COPYRIGHT.txt`.
- [#3325772](https://www.drupal.org/node/3325772) Исправлены некорректные тайпхинты в `SchemaCheckTrait`.
- [#2828724](https://www.drupal.org/node/2828724) Улучшена логика для одноразовых ссылок входа.
- [#3327115](https://www.drupal.org/node/3327115) Улучшены правила `.htaccess` связанные с `yarn.lock` и `package.json`.