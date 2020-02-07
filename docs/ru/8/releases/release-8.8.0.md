---
id: release-8.8.0
title: 'Drupal 8.8.0'
path: /8/releases/8.8.0
core: 8
metatags:
  title: 'Drupal 8.8.0: Список изменений'
  description: 'Список изменений Drupal 8.8.0. Дата релиза 4 декабря 2019 г.'
---

**Дата релиза**: 4 декабря 2019 г.\
**Перевод на поддержку безопасности**: 3 июня 2020 г.\
**Окончание поддержки**: +- полгода после перевода на поддержку безопасности.

## Обновление с 8.7.x

**Важная информация перед обновлением:**

- Прежде чем обновляться, убедитесь, что сервер, на котором работает сайт, имеет версию PHP как минимум 7.0.8 (рекомендуется PHP 7.2+). [Drupal 8.7.0](release-8.7.0.md) последний релиз который поддерживает PHP 5.6.
- Если вы используете модуль Pathauto, **вам необходимо обновить модуль, как минимум, до версии 1.6**, а только затем обновлять ядро. Если вы пользуетесь [Composer](../../composer/composer.md), то Drupal 8.8.0 указывает Pathauto версии 8.x-1.5 и ниже как конфликтные.

## Путь до конфигураций синхронизации теперь хранится в $settings вместо $config_directories

- [#3018145](https://www.drupal.org/node/3018145)

Путь до конфигураций синхронизации теперь хранится в `$settings['config_sync_directory']` файла **settings.php**.

Возможность указания нескольких конфигурационных директорий в `$config_directories` помечена устаревшим. Если ваш собственный или контрибный код опирается на данную особенность, вам необходимо перенести настройки либо в `$settings`, либо в другое хранилище, например, в State или конфигурации напрямую.

Как результат упрощения настройки пути до директории, были также помечены устаревшими функции `config_get_config_directory()` и `drupal_install_config_directories()`, а также константа `CONFIG_SYNC_DIRECTORY`.

Для получения пути до данной директории, вместо старого варианта `config_get_config_directory(CONFIG_SYNC_DIRECTORY)` используйте `Settings::get('config_sync_directory')`.

### Обновление settings.php

**Было**

```php
$config_directories['sync'] = 'sites/default/files/config_YLZJmmpOqc_KBWbMc2I58ky3-8c7qtg4G-OpSqFClHs5E0NL9YMFgyF4RRTv8IFdl_kAMs_Bdw/sync';
````

**Стало**

```php
$settings['config_sync_directory'] = 'sites/default/files/config_YLZJmmpOqc_KBWbMc2I58ky3-8c7qtg4G-OpSqFClHs5E0NL9YMFgyF4RRTv8IFdl_kAMs_Bdw/sync';
```

## Классы для визуализации были удалены для filter модуля

- [#3061520](https://www.drupal.org/node/3061520)

Все CSS классы (`filter-wrapper`, `filter-guidelines`, `filter-list` и `filter-help`), которые использовались для справочного раздела о текстовых форматах ввода, были удалены. Также были удалены все стили для данных классов из **filter.admin.css**.

> [!NOTE]
> Данное изменение не повлияет на темы, которые наследуются от Stable или Classy.

Если вы хотите вернуть данные классы обратно, можете воспользоваться следующим кодом в своей теме:

```php
/**
 * Implements hook_element_info_alter().
 */
function THEMENAME_element_info_alter(array &$info) {
  if (array_key_exists('text_format', $info)) {
    $info['text_format']['#process'][] = 'THEMENAME_process_text_format';
  }
}

/**
 * #process callback, for adding classes to filter components.
 *
 * @param array $element
 *   Render array for the text_format element.
 *
 * @return array
 *   Text_format element with the filter classes added.
 */
function THEMENAME_process_text_format(array $element) {
  $element['format']['#attributes']['class'][] = 'filter-wrapper';
  $element['format']['guidelines']['#attributes']['class'][] = 'filter-guidelines';
  $element['format']['format']['#attributes']['class'][] = 'filter-list';
  $element['format']['help']['#attributes']['class'][] = 'filter-help';

  return $element;
}
```

## Функции обратного вызова для рендера должны быть замыканием, либо реализовывать `TrustedCallbackInterface` или `RenderElementInterface`

- [#2966725](https://www.drupal.org/node/2966725)

Недавние обновления безопасности показали, что система рендера нуждается в более строгих требованиях к функциям обратного вызова, смотрите:

- [SA-CORE-2018-002](https://www.drupal.org/sa-core-2018-002)
- [SA-CORE-2018-004](https://www.drupal.org/sa-core-2018-004)
- [Code execution via Twig templates](https://www.drupal.org/project/drupal/issues/2860607)

Если у вас есть код, который добавляет функции обратного вызова для рендера - `#access_callback`, `#lazy_builder`, `#pre_render` или `#post_render`, то данный код необходимо обновить. На данный момент это будет лишь выводить сообщения ошибки устаревшего кода, но в конечном итоге будет вызываться исключение.

### Процедурные функции обратного вызова

Если ваша функция обратного вызова написана в процедурном стиле, например, `color_block_view_pre_render()`, вам необходимо перенести его в объект, который реализует `\Drupal\Core\Security\TrustedCallbackInterface`.

Пример:

```php
class ColorBlock implements TrustedCallbackInterface {

  /**
   * {@inheritdoc}
   */
  public static function trustedCallbacks() {
    return ['preRender'];
  }

  /**
   * #pre_render callback: Sets color preset logo.
   */
  public static function preRender($build) {
    // Do what the color_block_view_pre_render() function did. For the minimal change you can call the procedural function.
  }

}
```

В связи с этим, в ядре произошли следующие изменения:

- `drupal_pre_render_links()` заменен `\Drupal\Core\Render\Element\Link::preRenderLinks()`
- `color_block_view_pre_render()` заменен `\Drupal\color\ColorBlock::preRender()`
- `filter_form_access_denied()` заменен `\Drupal\filter\Element\TextFormat::accessDeniedCallback()`
- `history_attach_timestamp()` заменен `\Drupal\history\HistoryRenderCallback::lazyBuilder()`
- `_toolbar_do_get_rendered_subtrees()` заменен `\Drupal\toolbar\Controller\ToolbarController::preRenderGetRenderedSubtrees()`
- `toolbar_prerender_toolbar_administration_tray()` заменен `\Drupal\toolbar\Controller\ToolbarController::preRenderAdministrationTray()`
- `views_pre_render_views_form_views_form()` заменен `\Drupal\views\Form\ViewsFormMainForm::preRenderViewsForm()`

### Функции обратного вызова в объектах

Если ваша функция обратного вызова является экземпляром `RenderElementInterface`, никаких действий производить не требуется. В остальных случаях ваш объект должен реализовывать `\Drupal\Core\Security\TrustedCallbackInterface`. Например, `\Drupal\node\Plugin\Search\NodeSearch` имеет функцию обратного вызова в `#pre_render` `::removeSubmittedInfo()`. Необходимо добавить `TrustedCallbackInterface` для `NodeSearch` и реализовать `TrustedCallbackInterface::trustedCallbacks()` примерно следующим образом:

```php
  /**
   * {@inheritdoc}
   */
  public static function trustedCallbacks() {
    return ['removeSubmittedInfo'];
  }
```

### Анонимные функции обратного вызова

Использовать их не рекомендуется, так как они не могут быть сериализованы. Тем не менее, если вы их используете, вам не нужно ничего обновлять, так как они являются "доверенными".

## Ассеты библиотек в *.libraries.yml теперь могут быть помечены как устаревшие

- [#3064022](https://www.drupal.org/node/3064022)

Drupal использует `*.libraries.yml` файлы для [объявления библиотек](../libraries/libraries.md) модулями и темами. Иногда необходимо приостановить предоставление библиотеки. Это может быть вызвано тем, что EOL библиотеки подходит к концу, или библиотека больше по каким-то причинам больше не требуется. Так как удаление возможно только в мажорных версиях для сохранения обратной совместимости, теперь их можно помечать устаревшими.

Для этого было добавлено новое свойство `deprecated`, в котором можно указать причину устаревания библиотеки. Оно поддерживает токен подстановки `%library_id%`, которое можно использовать в сообщении, оно будет заменено на название библиотеки. При запросе данной библиотеки `LibraryDiscovery` будет выдавать ошибку.

**Было**

```yaml
jquery.ui.effects.scale:
  version: *jquery_ui_version
  license: *jquery_ui_license
  js:
    assets/vendor/jquery.ui/ui/effects/effect-scale-min.js: { minified: true }
  dependencies:
    - core/jquery.ui.effects.core
```

**Стало**

```yaml
jquery.ui.effects.scale:
  version: *jquery_ui_version
  license: *jquery_ui_license
  js:
    assets/vendor/jquery.ui/ui/effects/effect-scale-min.js: { minified: true }
  dependencies:
    - core/jquery.ui.effects.core
  deprecated: The "%library_id%" asset library is deprecated in drupal:8.8.0 and will be removed in drupal:9.0.0. Require jQuery UI as an explicit dependency or create a pure JavaScript solution. See https://www.drupal.org/node/3064015
```

## Исключение модулей из синхронизации конфигураций

- [#3079028](https://www.drupal.org/node/3079028)

В ядро добавлена возможность указывать список [модулей](../modules/modules.md), который будут исключены из в момент экспорта конфигураций.

Для того чтобы исключить модули из конфигураций, их необходимо перечислить в настройке `config_exclude_modules`.

```php
$settings['config_exclude_modules'] = ['devel'];
```

> [!WARNING]
> Все конфигурации, которые явно или неявно зависимы от перечисленных модулей, также будут исключены из экспорта.

## Представлены официальные Composer шаблоны для установки Drupal

- [#3082474](https://www.drupal.org/node/3082474) 

Ранее, Drupal проекты управляемые через Composer использовали шаблоны проектов вроде [drupal-composer/drupal-project](../../composer/composer-drupal-project.md) для управления зависимостями и проектом. Начиная с Drupal 8.8.0 аналогичные темплейты предоставляются официально ядром.

Вы можете выбирать из двух шаблонов:

- [drupal/recommended-project](../../composer/drupal-recommended-project.md) — рекомендуемый шаблон для создания новых сайтов на Drupal, при котором корень проекта находится на уровень выше. Это означает, что файл "index.php", папка "core" и т.д. расположены в директории "web", вместо того чтобы располагаться рядом с "composer.json" и "vendor" директорией в корне проекта. Данная структура рекомендуется, потому что она позволяет настроиться веб-сервер, что он будет предоставлять доступ только к "web" директории. Папка "vendor" и подобные будут находиться за пределами корня "web" директории, что положительно для безопасности.
- [drupal/legacy-project](../../composer/drupal-legacy-project.md) — данный шаблон создает новый сайт со структурой как она была в [Drupal 8.7.0](./release-8.7.0.md) и ранее. Файл "index.php" , "core" директория и т.д. расположены в корне проекта рядом с "composer.json" и "vendor" директорией. Используйте данный шаблон только если у вас нет возможности использовать рекомендуемый шаблон.

Для создания нового проекта используйте желаемый шаблон, например:

```shell script
composer -n create-project drupal/recommended-project:^8.8@dev my-project
```

> [!NOTE]
> Узнайте больше о том как управлять Drupal проектом при помощи [Composer](../../composer/composer.md).

## Синонимы путей конвертированы в сущности

- [#3013865](https://www.drupal.org/node/3013865)

Начиная с Drupal 8.8.0, алиасы путей конвертированы в ревизионные контент-сущности `path_alias`.

Сервис `path.alias_storage` продолжит работать для поддержки обратной совместимости, но его хуки помечены устаревшими.

Представленные ниже изменения рекомендуются для того чтобы полностью использовать все возможности новой системы и подготовить свой код к [Drupal 9](../../9/drupal-9.md).

### Используйте объекты синонимов вместо массивов

Устаревший массив предоставляющий информацию о синониме:

```php
$alias = [
  'source' => '/a/system/path',
  'alias' => '/a-path-alias',
  'langcode' => 'en',
];
```

Используйте методы объекта синонима для получения значений:

- `$alias['source']` становится `$path_alias->getPath()`
- `$alias['alias']` становится `$path_alias->getAlias()`
- `$alias['langcode']` становится `$path_alias->language()->getId()`

### Хуки

Используйте [хуки](../hooks/hooks.md) для сущностей, вместо старых, предназначенных для старых синонимов.

Ранее:

```php
hook_path_insert($path)
hook_path_update($path)
hook_path_delete($path)
```

Сейчас:

```php
hook_entity_insert(Drupal\Core\Entity\EntityInterface $entity)
hook_path_alias_insert(Drupal\Core\Path\PathAliasInterface $path_alias)

hook_entity_update(Drupal\Core\Entity\EntityInterface $entity)
hook_path_alias_update(Drupal\Core\Path\PathAliasInterface $path_alias)

hook_entity_delete(Drupal\Core\Entity\EntityInterface $entity)
hook_path_alias_delete(Drupal\Core\Path\PathAliasInterface $path_alias)
```

Использование хуков, помеченных устаревшими, в ядре заменены следующим образом:

```php
menu_link_content_path_insert() -> menu_link_content_path_alias_insert()
menu_link_content_path_update() -> menu_link_content_path_alias_update()
menu_link_content_path_delete() -> menu_link_content_path_alias_delete()

system_path_insert() -> PathAlias::postSave()
system_path_update() -> PathAlias::postSave()
system_path_delete() -> PathAlias::postDelete()
```

### Хранилище синонимов

Ранее:

```php
Drupal::service('path.alias_storage')->load($conditions)
Drupal::service('path.alias_storage')->save($source, $alias, $langcode = LanguageInterface::LANGCODE_NOT_SPECIFIED, $pid = NULL)
Drupal::service('path.alias_storage')->delete($conditions)
```

Новый вариант:

```php
Drupal::entityTypeManager()->getStorage('path_alias')->load($id)
Drupal::entityTypeManager()->getStorage('path_alias')->loadByProperties(array $values)
Drupal::entityTypeManager()->getStorage('path_alias')->save($path_alias)
Drupal::entityTypeManager()->getStorage('path_alias')->delete($path_alias)
```

### Формы

Объекты форм и их ID изменены следующим образом:

- `Drupal\path\Form\AddForm` (`path_admin_add`) -> `Drupal\path\PathAliasForm` (`path_alias_form`)
- `Drupal\path\Form\EditForm` (`path_admin_edit`) -> `Drupal\path\PathAliasForm` (`path_alias_form`)
- `Drupal\path\Form\DeleteForm` (`path_alias_delete`) -> `Drupal\Core\Entity\ContentEntityDeleteForm` (`path_alias_delete_form`)
- `Drupal\path\Form\PathFormBase` -> `Drupal\path\PathAliasForm`

Код, использующий хуки `hook_form_alter()` или `hook_form_FORM_ID_alter()` должен быть обновлён.

### Маршруты

Также были изменены маршруты:

- `path.admin_add` -> `entity.path_alias.add_form`
- `path.admin_edit` -> `entity.path_alias.edit_form`
- `path.delete` -> `entity.path_alias.delete_form`
- `path.admin_overview` -> `entity.path_alias.collection`

Маршрут `path.admin_overview_filter` помечен устаревшим и ему на замену пришел стандартный маршрут сущностей `entity.path_alias.collection`.

### Миграции

Плагин назначения `url_alias` помечен устаревшим в пользу общего для сущностей `entity:path_alias`. Миграции для Drupal 6 и 7 обновлены должным образом.

## Подсистема синонимов путей ядра перенесена в модуль path_alias

- [#3092086](https://www.drupal.org/node/3092086)

Система синонимов путей всегда была частью ядра Drupal. После того как синонимы стали сущностями, был создан новый модуль с API — `path_alias`. Данный модуль является обязательным в Drupal 8.8.0 и последующих релизах, но в конечном итоге, начиная с [Drupal 9](../../9/drupal-9.md) он будет опциональным.

Следующие сервисы подверглись изменениям:

- `path.alias_manager` помечен устаревшим и заменен новым сервисом `path_alias.manager`.
- `path.alias_whitelist` стал алиасом для нового сервиса `path_alias.whitelist`. Сам сервис перестал существовать. Если вам необходимо в своих сервисах сохранить поддержку с [Drupal 8.7.0](release-8.7.0.md) и ниже, используйте `ContainerBuilder::findDefinition('path.alias_whitelist')`.
- `path_subscriber` помечен устаревшим и заменен новым сервисом `path_alias.subscriber`.
- `path_processor_alias` помечен устаревшим и заменен новым сервисом `path_alias.path_processor`.

Следующие интерфейсы заменены на новые:

- `Drupal\Core\Path\AliasManagerInterface` на `Drupal\path_alias\AliasManagerInterface`
- `Drupal\Core\Path\AliasManager` на `Drupal\path_alias\AliasManager`
- `Drupal\Core\Path\AliasWhitelistInterface` на `Drupal\path_alias\AliasWhitelistInterface`
- `Drupal\Core\Path\AliasWhitelist` на `Drupal\path_alias\AliasWhitelist`
- `Drupal\Core\EventSubscriber\PathSubscriber` на `Drupal\path_alias\EventSubscriber\PathAliasSubscriber`
- `Drupal\Core\PathProcessor\PathProcessorAlias` на `Drupal\path_alias\PathProcessor\AliasPathProcessor`

**Требуется обновление кода**

Для того чтобы код был совместим с текущим (Drupal 8.8.0) релизом, необходимо произвести следующие изменения:

- Если ваш код как-то изменяет или заменяет сервис `path.alias_manager`, вы должны сделать аналогично с новым сервисом `path_alias.manager`.
- Если ваш код изменяет или заменяет сервис `path.alias_whitelist`, вам необходимо заменить его использование на `path_alias.whitelist `, либо использовать `ContainerBuilder::findDefinition('path.alias_whitelist')`.

Для того чтобы код был совместим с будущим [Drupal 9](../../9/drupal-9.md), необходимо произвести следующие изменения:

- Все использования API, обращающиеся к старым сервисам или объектам должны быть заменены на новые.
- Если ваши объекты реализют легаси интерфейсы, вам необходимо их заменить на новые.
- Классы и интерфейсы которые расширяют легаси варианты, должны заменить их на новые варианты.

Самое важное, как указано выше, модуль `path_alias` будет опциональным в Drupal 9, таким образом, код должен быть отрефакторен с учётом того, что данный API может отсутствовать, а также, явно указывать зависимость на новый модуль, если она требуется.

## Изменения браузеров, поддерживаемых Drupal ядром

- [#3079238](https://www.drupal.org/node/3079238)

Начиная с Druapl 8.8.0, Drupal ядро поддерживает следующие браузеры:

- Последние две мажорные версии:
  - Браузеры для ПК:
    - Google Chrome
    - Firefox
    - Safari
    - Microsoft Edge
    - Opera
  - Мобильные браузеры:
    - Safari for iOS
- Последнюю мажорную версию:
  - Браузеры для ПК:
    - Firefox ESR
    - Internet Explorer
  - Мобильные браузеры:
    - Chrome for Android
    - Chrome for iOS
    - UC Browser
    - Opera Mini
    - Samsung Internet
    
Поддержка браузеров означает:

- Drupal принимает сообщения об ошибках на поддерживаемых браузерах.
- Контрибуторы сами проверяют значительные изменения в нескольких браузерах на предмет регрессий, но это зависит от конкретных контрибуторов по конкретной задаче. На данный момент Drupal не имеет автоматизированных тестов для всех поддерживаемых браузеров.
- По умолчанию, Drupal ядро не принимает сообщения об ошибках для неподдерживаемых браузеров. Разработчики и команда безопасности Drupal будут принимать решения о значимости исправления в каждом конкретном случае, и вносить изменения при необходимости.
- Drupal может начать использование возможностей браузера, если все поддерживаемые браузеры имеют аналогичную поддержку или соответствующие полифилы. В каждой мажорной версии, полифилы, которые больше не требуются для хотя бы одного поддерживаемого браузера, будут удалены.

> [!NOTE]
> Это также означает, что сборщики ES5 и ES6 для JavaScript в ядре, также нацелены на данные браузеры.

## Новая экспериментальная тема административного интерфейса — Claro

- [#3087157](https://www.drupal.org/node/3087157)

В ядро добавлена новая экспериментальная тема оформления для административного интерфейса — Claro.

![Claro - Node edit](https://www.drupal.org/files/issues/2019-09-07/claro_node_add.png)

## Оформление и темизация

- [#3060703](https://www.drupal.org/node/3060703) Добавлена новая переменная `file_size` для шаблона **file-link.html.twig**.
- [#3074716](https://www.drupal.org/node/3074716) Переменная `link` в `template_preprocess_file_link()` теперь формируется как рендер массив, вместо готовой строки.
- [#3066713](https://www.drupal.org/node/3066713) Класс подтверждения сообщения о смене пароля изменен с `password-confirm` на `password-confirm-message`. В темах Stable и Classy будут присутствовать оба класса.
- [#2960810](https://www.drupal.org/node/2960810) Добавлены новые варианты для шаблонов страниц, которые позволят оформлять страницы для отличных от HTTP 200 ответов: `page--401.html.twig`, `page--403.html.twig`, `page--404.html.twig` и `page--4xx.html.twig`. Если для страниц данных ошибок указаны ноды, данные шаблоны не будут работать.
- [#3066038](https://www.drupal.org/node/3066038) `base theme` значение в `*.info.yml` файле темы является обязательным.
- [#3061281](https://www.drupal.org/node/3061281) Добавлена JavaScript функция `Drupal.theme.checkbox` для идентичной темизации `checkbox` элементов (`Drupal.theme('checkbox')`).
- [#3066722](https://www.drupal.org/node/3066722) Добавлена новая функция `Drupal.theme.ajaxProgressBar` при помощи которой можно изменить обертку над AJAX индикатором прогресса.
- [#3066723](https://www.drupal.org/node/3066723) Классы для AJAX индикатора прогресса перенесены в новый элемент. Данное изменение не касается тем `stable` и `classy`.
- [#3084859](https://www.drupal.org/node/3084859) Для разработки тем и использования современных возможностей CSS, Drupal ядро теперь использует PostCSS.
- [#3086531](https://www.drupal.org/node/3086531) [Темы оформления](../themes/themes.md) теперь могут быть помечены как экспериментальные: `experimental: true`.
- [#3064015](https://www.drupal.org/node/3064015) jQuery UI постепенно выводится из ядра и помечается устаревшим.
- [#3067969](https://www.drupal.org/node/3067969) jQuery UI библиотеки, помеченные устаревшими, больше не используются ядром. Данные библиотеки выносятся в соответствующие модули, если они вам по прежнему нужны. Например [jQuery UI Accordion](https://www.drupal.org/project/jquery_ui_accordion) — там же вы найдете все аналогичные проекты.
- [#3086383](https://www.drupal.org/node/3086383) html5shif полифил помечен устаревшим.
- [#3086643](https://www.drupal.org/node/3086643) [Popper.js](https://popper.js.org/) добавлен в ядро на замену jQuery UI Position.
- [#3086669](https://www.drupal.org/node/3086669) domready помечен устаревшим.
- [#3086653](https://www.drupal.org/node/3086653) matchMedia полифил помечен устаревшим.
- [#3089511](https://www.drupal.org/node/3089511) classList полифил помечен устаревшим.
- [#3084730](https://www.drupal.org/node/3084730) jQuery UI Sortable помечен устаревшим и использование данной библиотеке в ядре прекращено. Ранее, где применялась данная библиотека теперь используется [SortableJS](https://github.com/SortableJS/Sortable).

## Core API

- [#3056639](https://www.drupal.org/node/3056639) `MailManagerInterface::mail()` теперь позволяет переопределять сообщение об ошибке.
- [#3035273](https://www.drupal.org/node/3035273) Несколько функций относящихся к uri и scheme файлов помечены устаревшими и перенесены в `\Drupal\Core\StreamWrapper\StreamWrapperManagerInterface`.
- [#3059344](https://www.drupal.org/node/3059344) Добавлен подкласс `\Symfony\Component\Validator\ConstraintViolation` в Drupal `\Drupal\Core\Validation\ConstraintViolation` для использования в `\Drupal\Core\TypedData\Validation\ExecutionContext::addViolation()`.
- [#3054692](https://www.drupal.org/node/3054692) `\Drupal\system\SystemRequirements::phpVersionWithPdoDisallowMultipleStatements()` помечен устаревшим.
- [#3067713](https://www.drupal.org/node/3067713) Добавлен новый хук `hook_element_plugin_alter()`.
- [#3009387](https://www.drupal.org/node/3009387) Функция `drupal_get_user_timezone()` была заменена на [подписчик события](../events/events.md). Теперь можно использовать стандартную функцию PHP `date_default_timezone_get()` для получения актуального часового пояса.
- [#3075098](https://www.drupal.org/node/3075098) Для удобства переиспользования `\Drupal\Component\PhpStorage\FileStorage::htaccessLines()`, данный метод был помечен устаревшим и его логику перенесли в `\Drupal\Component\FileSecurity\FileSecurity::htaccessLines()`. Также, теперь пакет `drupal/core-php-storage` имеет зависимость на `drupal/core-file-security`.
- [#3068163](https://www.drupal.org/node/3068163) Для "select" элемента формы добавлена возможность указывать сортировку опций.
- [#2981313](https://www.drupal.org/node/2981313) Сортировка по умолчанию для заголовка таблицы, теперь может быть указана в параметре `initial_click_sort`.
- [#3086401](https://www.drupal.org/node/3086401) Скаффолд операции "append" теперь могут применяться к файлам которые не являются "скаффолдными".
- [#3024684](https://www.drupal.org/node/3024684) Для виджетов автодополнения `entity_reference_autocomplete` и `entity_reference_autocomplete_tags` добавлена возможность указывать кол-во отображаемых результатов.
- [#3086403](https://www.drupal.org/node/3086403) Добавлена новая Ajax команда `MessageCommand`, для установки системных сообщений в ответе.
- [#2779457](https://www.drupal.org/node/2779457) Пагинация теперь является сервисом. В связи с этим, все связанные глобальные переменные и функции помечены устаревшими.

## Content Moderation

- [#3056217](https://www.drupal.org/node/3056217) Добавлен новый метод `getOriginalState()` для сервиса `content_moderation.moderation_information` и `\Drupal\content_moderation\ModerationInformationInterface`. Это позволит получать предыдущее состояние ревизии в момент подготовки к сохранению новой.
- [#3052114](https://www.drupal.org/node/3052114) Для материалов, которые находятся в процессе "синхронизации", больше не будет создаваться новая ревизия.

## Entity API

- [#2835616](https://www.drupal.org/node/2835616) Функции `entity_get_display()` и `entity_get_form_display()` получили замену в виде сервиса `entity_display.repository`.
- [#3033656](https://www.drupal.org/node/3033656) Функции для просмотра сущностей помечены устаревшими.
- [#3075567](https://www.drupal.org/node/3075567) `EntityType::getLowercaseLabel()` помечен устаревшим. `EntityType::getSingularLabel()` теперь возвращает название сущности в нижнем регистре.
- [#3086279](https://www.drupal.org/node/3086279) Добавлен новый API для установки типа сущности с поддержкой полей `EntityDefinitionUpdateManager::installFieldableEntityType()`.

## Migrate API

- [#3043694](https://www.drupal.org/node/3043694) Возможность передачи массива для `link_uri` плагина источника Migrate API помечено устаревшим.
- [#3054173](https://www.drupal.org/node/3054173) Объекты, возвращаемые `getMessageIterator()` теперь содержат ID `source` и `destination`.
- [#2929443](https://www.drupal.org/node/2929443) Модули теперь могут объявлять свой статус обновления до Drupal 8.
- [#3073707](https://www.drupal.org/node/3073707) Миграции теперь могут валидировать сущность. В случае провала валидации, сохранение сущности не произойдет.
- [#3060969](https://www.drupal.org/node/3060969) `MigrateIdMapInterface::getMessageIterator()` помечен устаревшим в пользу `MigrateIdMapInterface::getMessages()`.
- [#3047268](https://www.drupal.org/node/3047268) Добавлены два новых сервиса: `migrate.lookup` и `migrate.stub`.

## User

- [#3038171](https://www.drupal.org/node/3038171) Модуль `user` теперь предоставляет новый маршрут поддерживающий стандарт [RFC5785](https://github.com/WICG/change-password-url), позволяющий менять пароль по well-known пути `/.well-known/change-password` (произведет 301-й редирект на форму восстановления пароля Drupal).
- [#3051463](https://www.drupal.org/node/3051463) Функции `user_delete()` и `user_delete_multiple()` помечены устаревшими.

## Views

- [#3040204](https://www.drupal.org/node/3040204) Установка идентификатора раскрытого фильтра Views теперь обрабатываются корректно.
- [#3047897](https://www.drupal.org/node/3047897) Конструктор `NodeNewComments` изменен. В него добавили два новых параметра, которые ожидают экземпляры объектов `EntityTypeManagerInterface` и `EntityFieldManagerInterface`, соответственно. Для старого кода будет выводиться предупреждение об ошибке, и сервисы будут автоматически получены из глобального контейнера.
- [#2869168](https://www.drupal.org/node/2869168) Добавлена возможность ограничивать доступные операторы для раскрытых фильтров.
- [#3074409](https://www.drupal.org/node/3074409) `<channel>` элемент, создаваемый в RSS лентах Views, больше не рендерится в процессе preprocess обработки.
- [#3066604](https://www.drupal.org/node/3066604) `Drupal\views\Form\ViewsExposedForm` теперь принимает информацию о текущем пути (`CurrentPathStack`) в качестве параметра конструктора.
- [#3023427](https://www.drupal.org/node/3023427) `Drupal\views\Plugin\views\field\LinkBase` теперь требует `EntityManager` и `LanguageManager`.
- [#3088233](https://www.drupal.org/node/3088233) Добавлены новые [хуки](../hooks/hooks.md) для изменения Views UI.
- [#2791359](https://www.drupal.org/node/2791359) `Drupal\views\Plugin\EntityReferenceSelection\ViewsSelection::__construct` теперь также принимает `$renderer` параметр.
- [#3087832](https://www.drupal.org/node/3087832) Из конфигураций Views удалено значение `core`. Если ваши конфигурации в профиле, модуле или теме используют его, то его необходимо удалить.
- [#3090442](https://www.drupal.org/node/3090442) Вызов `ViewsData::get()` без аргумента `$key` помечен устаревшим.
- [#3072765](https://www.drupal.org/node/3072765) Views UI больше не подключает библиотеку `jquery.ui.tabs`.

## File API

- [#3038437](https://www.drupal.org/node/3038437) Функция `file_scan_directory()` помечена устаревшей и перенесена в сервис `file_system`.
- [#3039255](https://www.drupal.org/node/3039255) Функция `file_directory_temp()` помечена устаревшей и перенесена в сервис `file_system`.

## JSON:API

- [#3078389](https://www.drupal.org/node/3078389) Элементы ссылок теперь сериализуются с двойным дефисом (`--`) вместо двоеточия (`:`).
- [#3078036](https://www.drupal.org/node/3078036) Изменена сигнатура метода `Drupal\jsonapi\Context\FieldResolver::resolveInternalEntityQueryPath()`.
- [#3084710](https://www.drupal.org/node/3084710) Нормалайзер сущности предоставляемый JSON:API был удален.
- [#3084746](https://www.drupal.org/node/3084746) `\Drupal\jsonapi\ResourceType\ResourceType::getFieldMapping()` помечен устаревшим.
- [#3084721](https://www.drupal.org/node/3084721) Свойство `\Drupal\jsonapi\ResourceType\ResourceType::$disabledFields` и `ResourceType::$invertedFieldMapping` помечены устаревшими.
- [#3085275](https://www.drupal.org/node/3085275) JSON:API теперь сериализует отображаемое имя пользователя в новом поле `display_name`, которое предназначено только для чтения. Поле `name` по прежнему содержит оригинальное, не измененное имя пользователя.
- [#3079797](https://www.drupal.org/node/3079797) Типы ресурсов JSON:API теперь могут быть программно настроены при помощи [событий](../events/events.md).
- [#3088385](https://www.drupal.org/node/3088385) `Drupal\jsonapi\JsonApiResource\Link::merge` теперь предоставляет ошибку сравнения, когда принята попытка создать ссылку с разными типами связей.
- [#3087821](https://www.drupal.org/node/3087821) Построение объекта ссылки JSON:API при помощи массива с типами ресурсов помечено устаревшим. Теперь можно указывать только один конкретный тип.
- [#3087598](https://www.drupal.org/node/3087598) JSON:API теперь возвращает коды ошибок в виде строк, а не чисел, как этого требует спецификация.

## Media и Media Library

- [#3075165](https://www.drupal.org/node/3075165) `Drupal\media_library\Form\FileUploadForm` теперь принимает `file_usage` [сервис](../services/services.md) в конструкторе.
- [#3085857](https://www.drupal.org/node/3085857) Добавлен шаблон для оформления ошибок вставки медиа элементов. **Stable и Stark темы не предоставляют шаблоны по умолчанию.**
- [#3087129](https://www.drupal.org/node/3087129) Улучшена поддержка мультиязычных сайтов для Media Library виджетов. Дублей больше не будет!
- [#3087775](https://www.drupal.org/node/3087775) Добавлен новый фильтр "Встраиваемые медиа". Он вводит поддержку тега `<drupal-media>` и трансформацию его в встроенную медиа-сущность.
- [#3088444](https://www.drupal.org/node/3088444) Добавлены фильтры для Views, определяющие доступ к медиа-сущностям по статусу опубликован/не опубликован.
- [#3087431](https://www.drupal.org/node/3087431) Media Library теперь стабильный модуль.
- [#3089300](https://www.drupal.org/node/3089300), [#3089298](https://www.drupal.org/node/3089298) Media Library больше не предоставляет классы для оформления. Они перенесены в темы Classy и Seven.
- [#3089245](https://www.drupal.org/node/3089245) Media Library теперь предоставляет шаблоны `media-library-wrapper.html.twig` и `media-library-item.html.twig` для более точного и гибкого оформления.
- [#3089217](https://www.drupal.org/node/3089217) `\Drupal\media_library\Form\AddFormBase` теперь требует указывать ID формы для всех дочерних форм.

## Help Topics

- [#3072984](https://www.drupal.org/node/3072984) Содержимое Help Topics теперь "индексируемо" (можно искать по ним).

## Search

- [#3075696](https://www.drupal.org/node/3075696) Функции для поискового индекса перенесены в сервис `search.index`, а функции помечены устаревшими.

## Content Moderation

- [#3087336](https://www.drupal.org/node/3087336) Content Moderation теперь совместим с Workspaces.
- [#3087295](https://www.drupal.org/node/3087295) Методы сервиса `content_moderation.moderation_information` `isLatestRevision`, `getLatestRevision`, и `getLatestRevisionId` помечены устаревшими.

## Workspaces

- [#3092447](https://www.drupal.org/node/3092447) Добавлена возможность создания рабочих подпространств для обеспечения разветвленного процесса создания содержимого. Например, теперь можно отправлять содержимое с разных локальных версий сайта на сервер для разработки, а он уже все вместе сможет отправить на боевую версию.
- [#3096868](https://www.drupal.org/node/3096868) Сущность `workspace_association` была удалена. Она была предназначена для внутреннего использования, а её возможности теперь выполняют ревизии.

## Тестирование

- [#3030340](https://www.drupal.org/node/3030340) Объект `WebTestBase` помечен устаревшим.
- [#3056869](https://www.drupal.org/node/3056869) Поддержка PHPUnit 4.x прекращена, теперь будет запрашиваться версия ^6.5. В соответствии с данным изменением, [Drush](../../drush.md) команды `drupal-phpunit-upgrade` и `drupal-phpunit-upgrade-check` больше не нужны и были удалены.
- [#3057326](https://www.drupal.org/node/3057326) Передача File сущности в качестве первого аргумента в `assertFileExists` и `assertFileNotExists` помечена устаревшей.
- [#2906685](https://www.drupal.org/node/2906685) Добавлен новый метод `JSWebAssert::waiteForElementRemoved()`, который позволяет дождаться удаление определенного элемента со страницы, прежде чем продолжить.
- [#2949692](https://www.drupal.org/node/2949692) `Drupal\simpletest\TestDiscovery` помечен устаревшим в пользу `Drupal\Core\Test\TestDiscovery`.
- [#3075252](https://www.drupal.org/node/3075252) Simpletest функции тестирования БД помечены устаревшими и перенесены в `Drupal\Core\Test\TestDatabase`.
- [#2948547](https://www.drupal.org/node/2948547)  Simpletest функции связанные с PHPUnit теперь являются классами `\Drupal\Core\Test\PhpUnitTestRunner` и `\Drupal\Core\Test\JUnitConverter`.
- [#3077623](https://www.drupal.org/node/3077623) Конструктор `\Drupal\Core\Extension\Extension` теперь валидирует ввод в режиме разработки.
- [#3076634](https://www.drupal.org/node/3076634) Функции `simpletest_clean_*()` были помечены устаревшими и перенесены в соответствующие классы.
- [#3082134](https://www.drupal.org/node/3082134) Название сервиса `simpletest.config_schema_checker` изменено на `testing.config_schema_checker`, так как он не имеет отношения к Simpletest модулю.
- [#3083549](https://www.drupal.org/node/3083549) Опция `--browser` для `run-tests.sh` помечена устаревшей.
- [#3083055](https://www.drupal.org/node/3083055) Для установочного профиля "testing" теперь необходимо явно указывать тему оформления для установки. Значение по умолчанию `classy` удалено.
- [#3082383](https://www.drupal.org/node/3082383) Добавлена новый тип теста `BuildTest`.
- [#3085950](https://www.drupal.org/node/3085950) Тесты Nightwatch теперь используются Stark тему по умолчанию, вместо Classy.
- [#3078763](https://www.drupal.org/node/3078763) PHPUnit 7 может быть использован для тестов на PHP 7.1+.
- [#3041703](https://www.drupal.org/node/3041703) `\Drupal\Tests\taxonomy\Functional\TaxonomyTestTrait` перемещен в Traits директорию `\Drupal\Tests\taxonomy\Traits\TaxonomyTestTrait`.
- [#3082086](https://www.drupal.org/node/3082086) `assertTrue()` и `assertFalse()` помечены устаревшими для PHPUnit Kernel, Functional и FunctionalJavascript тестов.
- [#3091784](https://www.drupal.org/node/3091784) Модуль SimpleTest помечен устаревшим.

## Конфигурации

- [#3037022](https://www.drupal.org/node/3037022) Новый сервис `ExportStorage` для помощи при экспортировании конфигураций.
- [#2943918](https://www.drupal.org/node/2943918) `ConfigImporter` теперь также получает сервис `extension.list.module` в качестве аргумента.
- [#3066005](https://www.drupal.org/node/3066005) 🔥 Добавлены события для конфигураций - `ConfigEvents`: `STORAGE_TRANSFORM_IMPORT`, `STORAGE_TRANSFORM_EXPORT`, `STORAGE_TRANSFORM`. Теперь вы можете подписываться на данные события и влиять на конфигурации. _На данный момент, поведение будет работать только с активным экспериментальным модулем `config_environment`._

## Стандарты

- [#3041002](https://www.drupal.org/node/3041002) CSS стандарты для сортировки теперь обрабатываются с использованием stylelint-order плагином.

## Прочие изменения

- [#3033540](https://www.drupal.org/node/3033540) Формы модуля `action` были перенесены в `src/Form`.
- [#2853355](https://www.drupal.org/node/2853355) Добавлен новый трейт `ConfigurableTrait` для уменьшения кода плагинов.
- [#3050078](https://www.drupal.org/node/3050078) Third party settings для форматтеров полей теперь передаются вместе с рендер элементом и доступы по ключу `#third_party_settings`.
- [#3051983](https://www.drupal.org/node/3051983) Функция `drupal_schema_get_field_value()` помечена устаревшей.
- [#2769027](https://www.drupal.org/node/2769027) Для SQLite включена опция [WAL](https://www.sqlite.org/wal.html) (Write-Ahead Logging) по умолчанию.
- [#3045094](https://www.drupal.org/node/3045094) Поле "сводка" для поля "Текст с сводкой" теперь может быть помечено как обязательное.
- [#3041017](https://www.drupal.org/node/3041017) 🔥 Drupal теперь имеет свой собственный Scaffold Composer плагин, который добавлен в ядро.
- [#3017233](https://www.drupal.org/node/3017233) `ThemeHandlerInterface` помечен устаревшим. На его замену добавили новый сервис `theme_installer`.
- [#3063510](https://www.drupal.org/node/3063510) `\Drupal\Core\Cache\Apcu4Backend` помечен устаревшим. Если вы пользовались им, все вызовы должны быть перенаправлены на `\Drupal\Core\Cache\ApcuBackend`.
- [#3068527](https://www.drupal.org/node/3068527) `HelpSection` плагины теперь могут указывать вес (`weight`).
- [#3070036](https://www.drupal.org/node/3070036) Теперь для блока поиска можно выбирать, куда будет перенаправлен пользователь при отправке запроса.
- [#3069730](https://www.drupal.org/node/3069730) `wikimedia/composer-merge-plugin` удалён в пользу путей до репозиториев.
- [#3059717](https://www.drupal.org/node/3059717) Добавлен [Composer](../../composer/composer.md) плагин `drupal/core-vendor-cleanup`, который позволяет подчищать в пакетах после установки и обновления различные директории. Например test и documentation. Что должно позволить облегчить проекты и улучшить безопасность.
- [#3075873](https://www.drupal.org/node/3075873) Скафолд файлы теперь также будут присутствовать в `drupal/core` пакете. Добавлены соответствующие тесты, которые следят чтобы они были идентичны при дублировании.
- [#3072313](https://www.drupal.org/node/3072313) Для элемента формы `dropbutton` добавлена настройка `#dropbutton_type`.
- [#3021778](https://www.drupal.org/node/3021778), [#2874695](https://www.drupal.org/node/2874695) `menu_local_tabs()`, `menu_primary_local_tasks()` и `menu_secondary_local_tasks()` помечены устаревшими.
- [#2940126](https://www.drupal.org/node/2940126) `file_ensure_htaccess()` и `file_save_htaccess()` помечены устаревшими.
- [#3030645](https://www.drupal.org/node/3030645) `tracker_page()` и `tracker.pages.inc` помечены устаревшими. Логика была перенесена в `\Drupal\tracker\Controller\TrackerController::buildContent`.
- [#3052704](https://www.drupal.org/node/3052704) `drupal_installation_attempted()` помечен устаревшим.
- [#3085704](https://www.drupal.org/node/3085704) Для формы восстановления пароля добавлена защита от флуда.
- [#3000069](https://www.drupal.org/node/3000069) `drupal_process_states()` помечен устаревшим.
- [#3084856](https://www.drupal.org/node/3084856) `NodeController::add()` помечен устаревшим.
- [#3086614](https://www.drupal.org/node/3086614) Конфигурации [установочных профилей](../distributions/distributions.md) теперь имеют приоритет над конфигурациями по умолчанию, предоставляемых модулями.
- [#3082634](https://www.drupal.org/node/3082634) Добавлены `Drupal.deprecationError()` и `Drupal.deprecatedProperty()` для запуска ошибок устаревшего кода для JavaScript.
- [#3071740](https://www.drupal.org/node/3071740) Добавлен трейт `CacheTagsChecksumTrait`.
- [#3086773](https://www.drupal.org/node/3086773) Форкнутая версия `SimpleAnnotationReader` от Doctrine теперь предоставляется Drupal ядром и должна быть использована вместо оригинального.
- [#3075385](https://www.drupal.org/node/3075385) Базовые поля `aggregator_feed.title`, `aggregator_item.title` и `taxonomy_term.name` теперь учитывают настройки отображения.
- [#3081957](https://www.drupal.org/node/3081957) Модуль Place Block помечен устаревшим и будет удален в [Drupal 9](../../9/drupal-9.md).
- [#3075102](https://www.drupal.org/node/3075102) Плагины редактора CKEditor теперь принимают `state` в качестве параметра конструктора.
- [#3089181](https://www.drupal.org/node/3089181) Сервис `argument_resolver.raw_parameter` был удалён.
- [#3093409](https://www.drupal.org/node/3093409) Текущая версия ядра больше не задается в ручную, а обновляется composer в процессе обновления.
- [#3057191](https://www.drupal.org/node/3057191) `\Drupal\Component\Utility\Crypt::randomBytes()` помечен устаревшим в пользу стандартной PHP функции `random_bytes()`.
- [#3057322](https://www.drupal.org/node/3057322) `\Drupal\Component\Utility\Unicode::caseFlip()` помечен устаревшим, так как в PHP 7 он больше не нужен.
- [#3054488](https://www.drupal.org/node/3054488) `\Drupal\Component\Utility\Crypt::hashEquals()` помечен устаревшим в пользу стандартной PHP функции `hash_equals()`.
- [#3087592](https://www.drupal.org/node/3087592) Прекращена официальная поддержка веб-сервера Hiawatha.

## См. также

- [Drupal 8.7.0](release-8.7.0.md) — предыдущий минорный релиз.

## Ссылки

- [Drupal 8.8.0](https://www.drupal.org/project/drupal/releases/8.8.0) (англ.), drupal.org, 4 декабря 2019
