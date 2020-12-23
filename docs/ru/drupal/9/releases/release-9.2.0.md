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

## Editor

- [#3181290](https://www.drupal.org/project/drupal/issues/3181290) Удалён неиспользуемый код в `editor.admin.es6.js`.

## Entity System

- [#3138631](https://www.drupal.org/project/drupal/issues/3138631) Если поле содержит некорректную связь, сообщение об ошибке теперь будет более точно сообщать с чем связана проблема. 

## Form API

- [#2702233](https://www.drupal.org/project/drupal/issues/2702233) Добавлены JavaScript тесты для Form API `#states` состояний: `required`, `visible`, `invisible`, `expanded`, `checked`, `unchecked`.

## Help Topics

- [#3073476](https://www.drupal.org/project/drupal/issues/3073476) Документация для модулей migrate, migrate_drupal, migrate_drupal_multilingual и migrate_drupal_ui переделана в Help Topics.
- [#3047711](https://www.drupal.org/project/drupal/issues/3047711) Документация для модулей file, image, media, media_library и responsive_image переделана в Help Topics.

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

## Olivero

- [#3187884](https://www.drupal.org/project/drupal/issues/3187884) Удален лишний условный оператор из `block--system-branding-block.html.twig`.

## PostgreSQL DB Driver

- [#3185399](https://www.drupal.org/project/drupal/issues/3185399) Для определения предыдущего ID теперь используется `RETURNING`.

## System

- [#2409413](https://www.drupal.org/project/drupal/issues/2409413) Удалены неиспользуемые RSS настройки и описания.

## Taxonomy

- [#3170185](https://www.drupal.org/project/drupal/issues/3170185) `Drupal\taxonomy\Form\OverviewTerms` теперь использует `pager.manager` для получения текущей страницы вместо `Request`.

## Toolbar

- [#3174422](https://www.drupal.org/project/drupal/issues/3174422) Для тулбара добавлен класс `clearfix`.

## Umami Demo

- [#3051465](https://www.drupal.org/project/drupal/issues/3051465) Поле тегов для материалов теперь не переводимое и не создаёт автоматически несуществующие теги.
- [#3066570](https://www.drupal.org/project/drupal/issues/3066570) Роль редактора теперь имеет больше прав, в связи с чем может: управлять переводами, изучать страницы "помощи", просматривать таксономию, управлять ярлыками.

## Update

- [#2577407](https://www.drupal.org/project/drupal/issues/2577407) Установка нового модуля через интерфейс теперь имеет постоянный лейбл «Add».

## User

- [#3186752](https://www.drupal.org/project/drupal/issues/3186752) Аргумент `$langcode` для функции `_user_mail_notify()` помечен устаревшим.

## Views

- [#2628130](https://www.drupal.org/project/drupal/issues/2628130) Параметр `$database` для `\Drupal\node\Plugin\views\argument\Vid` помечен устаревшим.
- [#2925612](https://www.drupal.org/project/drupal/issues/2925612) Метод `StylePluginBase::wizardForm()` помечен устаревшим, так как нигде не используется.

## Тестирование

- [#3176655](https://www.drupal.org/project/drupal/issues/3176655) `GoutteDriver` помечен устаревшим, вместо него ядро теперь использует `BrowserKitDriver`.
- [#3132887](https://www.drupal.org/project/drupal/issues/3132887) `BrowserTestBase::drupalGetHeader()` помечен устаревшим. Используйте `$this->getSession()->getResponseHeader()`.
- [#3181329](https://www.drupal.org/project/drupal/issues/3181329) Удалён метод `BrowserTestBase::getResponseLogHandler()` так как он дублирует `BrowserHtmlDebugTrait::getResponseLogHandler()`.
- [#3184632](https://www.drupal.org/project/drupal/issues/3184632) Сравнения для submit инпутов с использованием xpath заменены на WebAssert.
- [#3139431](https://www.drupal.org/project/drupal/issues/3139431) Вызовы устаревшего `AssertLegacyTrait::assertFieldsByValue()` заменены на современные аналоги.
- [#3132919](https://www.drupal.org/project/drupal/issues/3132919) Вызовы `::assert*()` со сравнением в качестве ожидаемого значения заменены на `::assertNot*()`.

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