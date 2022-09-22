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

## Big Pipe

- [#3294720](https://www.drupal.org/node/3294720) `Drupal.attachBehaviors()` 
  теперь вызывается на элементы до окончания загрузки.

## Block

- [#3281429](https://www.drupal.org/node/3281429) Тесты, не использующие миграции, обновлены чтобы не использовать темы Bartik и Seven.
- [#3105880](https://www.drupal.org/node/3105880) Из теста `BlockRepositoryTest` удалён неиспользуемый код.
- [#3061148](https://www.drupal.org/node/3061148) Исправлена неполадка, из-за которой заголовки отключеных блоков проходили через двойное экранирование.
- [#3301663](https://www.drupal.org/node/3301663) Из метода `\Drupal\block\BlockForm::buildVisibilityInterface()` 
  удалена проверка устаревшего условия `node_type`.

## Block Content

- [#3053881](https://www.drupal.org/node/3053881) Исправлена неполадка, из-за которой возврат старой ревизии с 
  использованием собственных блоков могло приводить к вызову `EntityChangedConstraint`.
- [#3307182](https://www.drupal.org/node/3307182) smustgrave добавлен в качестве мейнтейнера `block_content`.

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
- [#3268306](https://www.drupal.org/node/3268306) Исправлена неполадка, из-за которой могли не сохранятся 
  собственные HTML-теги, например: `<drupal-media>`, `<drupal-entity>`, `<foobar>`.
- [#3283776](https://www.drupal.org/node/3283776) Внесены улучшения в `CKEditor5PluginDefinition::getElements()`.
- [#3306216](https://www.drupal.org/node/3306216) Прозрачные иконки теперь 
  форсируются быть полностью видимыми.
- [#3222756](https://www.drupal.org/node/3222756) Добавлена поддержка 
  загрузки изображений из внешних источников.
- [#3231336](https://www.drupal.org/node/3231336) Внесены улучшения в `HtmlRestrictions.`
- [#3280343](https://www.drupal.org/node/3280343) Актуализированы `@todo` комментарии.

## Colors

- [#3281430](https://www.drupal.org/node/3281430) Тесты модуля, не связанные с миграциями, больше не используют Bartik.

## Comment

- [#2958241](https://www.drupal.org/node/2958241) Внесены улучшения в 
  `CommentSelection` для корректной работы комментариев с различными типами 
  сущностей.
- [#3308912](https://www.drupal.org/node/3308912) Улучшена производительность теста `CommentEditTest`.

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

## Configuration System

- [#3295735](https://www.drupal.org/node/3295735) Улучшена работа теста `ConfigImportUITest` с темой Olivero.
- [#2232051](https://www.drupal.org/node/2232051) Запись конфигурации в файл больше не вызывает `chmod()`.

## Dependency Injection

- [#2531564](https://www.drupal.org/node/2531564) Улучшена сериализация контейнера.
- [#3310271](https://www.drupal.org/node/3310271) Улучшена сериализация строковых сервисов.

## Editor

- [#3306715](https://www.drupal.org/node/3306715) Для `editor_private_test` зависимость от `ckeditor` заменена на `editor_test`.

## Entity API

- [#3266589](https://www.drupal.org/node/3266589) Удалён заголовок `Link` из ответа страницы сущности.

## Field Layout

- [#3292560](https://www.drupal.org/node/3292560) Тесты модуля больше не используют тему оформления Classy.

## File

- [#2588013](https://www.drupal.org/node/2588013) Из `managed_file` элемента 
  удалён суффикс `<span class="ajax-new-content"></span>`.

## Forum

- [#2774399](https://www.drupal.org/node/2774399) Улучшена проверка на возможность удаления модуля.

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

## JSON:API

- [#3280302](https://www.drupal.org/node/3280302) В `JsonApiDocumentTopLevelNormalizerTest` исправлен вызов с лишним аргументом.

## Help Topics

- [#3086075](https://www.drupal.org/node/3086075) Twig синтаксис, не 
  являющийся частью самой разметки (пример для документации), внутри Help 
  Topics теперь обрабатывается корректно.
- [#3281439](https://www.drupal.org/node/3281439) Тесты модуля больше не используют темы оформления Bartik и Seven.

## Layout Builder

- [#2935999](https://www.drupal.org/node/2935999) Модуль больше не требует включения модуля Field UI.

## Media

- [#3255887](https://www.drupal.org/node/3255887) Исправлена ошибка в 
  конструкторе `MediaThumbnailFormatter`.
- [#3275807](https://www.drupal.org/node/3275807) Тесты модуля больше не используют тему Classy.

## Media Library

- [#3278418](https://www.drupal.org/node/3278418) Тесты модуля больше не используют тему оформления Classy.

## MySQL

- [#3296108](https://www.drupal.org/node/3296108) `mysql_requirements()` 
  теперь производит проверки, только если подключение использует MySQL.

## Node

- [#1198120](https://www.drupal.org/node/1198120) Добавлены описания для 
  прав доступа «Edit own *».
- [#3264987](https://www.drupal.org/node/3264987) Исправлено двойное 
  присвоение переменной в `NodeViewsData`.
- [#3303278](https://www.drupal.org/node/3303278) Тесты связанные с модулем RDF перенесены в одноимённый модуль.

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

## User

- [#2563995](https://www.drupal.org/node/2563995) Оптимизирован код в методе 
  `RoleStorage::isPermissionInRoles()`.
- [#3282420](https://www.drupal.org/node/3282420) При использовании одноразовой ссылки для входа теперь предлагается «установить», а не «менять» пароль. Это изменение касается текста сообщения.
- [#3285593](https://www.drupal.org/node/3285593) Улучшена документация для `Section::getComponents()`.

## Views

- [#3295813](https://www.drupal.org/node/3295813) Добавлено отсутствующее объявление свойства `ViewsEntitySchemaSubscriber::$viewsToSave`.
- [#2796045](https://www.drupal.org/node/2796045) Улучшено сообщение об ошибки при добавлении поля связью когда эта связь недоступна.
- [#2568889](https://www.drupal.org/node/2568889) Исправлена неполадка, из-за которой раскрытый фильтр для текстового поля с обязательным значением мог показывать пустую ошибку.
- [#3309750](https://www.drupal.org/node/3309750) Исправлена неполадка с синтаксисом `callable` для PHP 8.2.

## Views UI

- [#2473877](https://www.drupal.org/node/2473877) Исправлена неполадка, 
  из-за которой индикатор прогресса оформлялся как пагинация и находился в 
  неположенном месте.
- [#2313073](https://www.drupal.org/node/2313073) Исправлена неполадка, из-за которой не работала передача «0» в 
  качестве контекстного фильтра.

## Стандартный установочный профиль

- [#3243121](https://www.drupal.org/node/3243121) Профиль больше не устанавливает модуль RDF.

## Тестирование

- [#3281535](https://www.drupal.org/node/3281535) Внесены исправления для «Access to an undefined property».
- [#3302760](https://www.drupal.org/node/3302760) Добавлены фикстуры с БД Drupal 9.4.0.

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
- [#3270734](https://www.drupal.org/node/3270734) Тесты модулей Editor и 
  CKEditor 5 больше не используют CKEditor 4.
- [#3291797](https://www.drupal.org/node/3291797) Обновлены стили для 
  off-canvas элемента с использованием CSS переменных.
- [#3306720](https://www.drupal.org/node/3306720) `twig_theme_test` теперь 
  подключает библиотеки CKEditor 5.
- [#3304731](https://www.drupal.org/node/3304731) Оставшиеся тесты что 
  используют Classy, теперь используют Starterkit.
- [#3285054](https://www.drupal.org/node/3285054) В темы Claro и Olivero 
  добавлено отключение стилей CKEditor 5 `ckeditor5-stylesheets: false`.
- [#3306897](https://www.drupal.org/node/3306897) Обновлены фикстуры с дампами БД Drupal 9.4.0.
- [#3307454](https://www.drupal.org/node/3307454) Тесты связанные с темой Classy перенесены непосредственно в тему.
- [#3308458](https://www.drupal.org/node/3308458) В `core.libraries.yml` внесены утерянные исправления из более новых веток.
- [#3308987](https://www.drupal.org/node/3308987) Удалены упоминания Stable темы.
- [#3274474](https://www.drupal.org/node/3274474) Исправлены ошибки PHPStan «Access to an undefined property».
- [#3308744](https://www.drupal.org/node/3308744) Улучшена работа с магическим свойством `connection` в `FileTransfer`.
- [#3298731](https://www.drupal.org/node/3298731) Исправлена неполадка с `ConstraintViolation::$arrayPropertyPath` для PHP 8.2.