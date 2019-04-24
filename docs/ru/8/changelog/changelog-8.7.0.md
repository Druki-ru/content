---
id: changelog-8.7.0
title: 'Drupal 8.7.0'
path: /docs/8/changelog/8.7.0
core: 8
---

> [!NOTE]
> Список изменений не полный.

## Обновление с 8.6.x

> [!WARNING]
> Более подробная и точная информация появится с релизом. Указаны приблизительные действия для обновления.

Модуль JSON:API теперь в ядре. Необходимо деинсталлировать и удалить данный модуль с проекта (включая {Composer}(composer) зависимость). После обновления, включить данный модуль из ядра.

Если вы использовали данный модуль как зависимость для своих, замените зависимость с `jsonapi:jsonapi` на `drupal:jsonapi`.

## Подробные изменения

### Прекращение поддержи PHP 5.5 и 5.6

- [#2938726](https://www.drupal.org/node/2938726)

Начиная с релиза Drupal 8.7.0, прекращается официальная поддержка PHP 5.5 и 5.6 версий. Drupal 8.6.x становится последним релизом Drupal 8 с поддержкой PHP 5.

Уже имеющиеся сайты продолжат работать на данных версиях после обновления, но стабильность  не гарантируется. Установка новых сайтов на PHP ниже 7-й версии будет запрещена.

### JavaScript Messages API

- [#2930536](https://www.drupal.org/node/2930536), [#2935209](https://www.drupal.org/node/2935209), [#3002643](https://www.drupal.org/node/3002643)

Данное изменение представляет новое API для JavaScript, при помощи которого вы можете отображать системные сообщения на странице. По умолчанию, все сообщения будут показаны в том же месте, где и системные сообщения отправленные сервером.

Данный API также позволяет отображать сообщения в других местах на странцие, но для этого коду придется отвечать за HTML разметку элемента, в которое помещается сообщение.

#### Настройка и примеры

В ядро добавлена новая {библиотека}(libraries:8) `drupal.message`, которую необходимо подключать там, где вы хотите использовать данный API.

Пример создания нового экземпляра класса сообщений:

```javascript
const messages = new Drupal.messages();
```

Пример добавления сообщения на странице:

```javascript
messages.add('Hello World!');
```

Добавление сообщения на страницу также может принимать вторым аргументом объект `options`, который может содержать:

- `id`: Строка с идентификатором сообщения, чтобы можно было удалить его в дальнейшем. Не указывайте `id` равный `type`.
- `type`: Строковое значение с типом сообщения. Допустимые значения: "status", "error" и "warning". По умолчанию установлено значение "status".
- `announce`: Строковое значение для скрин-ридеров. Если вы не хотите чтобы значение попало в `Drupal.announce()`, оставьте значение пустым.
- `priority`: Строкове значение с приоритетом в соответствии с `Drupal.announce()`.

Пример удаления сообщения:

```javascript
messages.remove('status-1234567890');
```

Может принимать в качестве аргумента как конкретное значение с `id`, так и `type`, для удаления всех сообщений определенного типа. Вы также можете передать массив из значений, которые необходимо удалить.

Пример очистки всех сообщений:

```javascript
messages.clear();
```

#### Изменения в темплейте

В связи с внедрением данного API, также есть небольшие изменения для темплейта системных сообщений. Если ваша тема переопределеяет `status-messages.html.twig`, и не использует `extends`, то убедитесь что у вас добавлен аттрибут `data-drupal-messages`, чтобы данный API знал где выводить сообщения.

Например:

```twig
<div data-drupal-messages>
 {% for type, messages in message_list %}
   <div role="contentinfo" aria-label="{{ status_headings[type] }}"{{ attributes|without('role', 'aria-label') }}>
     {% if type == 'error' %}

     {% endif %}
   </div>
 {% endfor %}
</div>
```

#### Изменения в status_messages

Также изменения коснулись и рендер элемента `status_messages`. Теперь он может принимать новое значение `#include_fallback`. Если установить данное значение в `TRUE`, то вместо сообщений будет отреденрена только обертка под системные сообщения.

```php
$element = [
  '#type' => 'status_messages',
  '#include_fallback' => TRUE,
];
```

```html
<div data-drupal-messages-fallback class="hidden"></div>
```

Скорее всего, вам не потребуется использовать данное значение, так как Drupal по умолчанию добавляет фолбек разметку на все страницы. Тем не менее, если ваш код предоставляет варианты отображения страницы, вам необходимо проследить за данной областью самостоятельно используя данную возможность.

### Media Library виджет готовится к изменениям UI/UX

- [#3024762](https://www.drupal.org/node/3024762)

TODO, должны быть ещё минимум 2 изменения на эту тему.

## Прочие изменения

- [#2709919](https://www.drupal.org/node/2709919) Фукция `_system_rebuild_module_data()` и подобные ей заменены на сервисы.
- [#2994780](https://www.drupal.org/node/2994780) Модуль комментариев больше не запоминает IP адреса для комментариев по умолчанию. Существующие сайты продолжат логировать IP, но это может быть изменено в **settings.php** `$conf['comment.settings']['log_ip_addresses'] = FALSE;`.
- [#2929327](https://www.drupal.org/node/2929327) Сущность workflow теперь имеет дополнительные операции доступа.
- [#2988067](https://www.drupal.org/node/2988067) Новый интерфейс `SynchronizableInterface` доступе для всех типов сущностей.
- [#2981627](https://www.drupal.org/node/2981627) В `EntityAdapter` добавлен семантический метод `getEntity()` в дополнение к `getValue()`.
- [#2997122](https://www.drupal.org/node/2997122) Теперь вы можете использовать вызываемые {сервисы}(services:8) в качестве {контроллеров}(routing:8), при помощи магического метода [__invoke](https://php.net/manual/en/language.oop5.magic.php#object.invoke).
- [#2998929](https://www.drupal.org/node/2998929) Добавлен трейт `EntityOwnerTrait`, который может быть добавлен к сущностям в связке с `owner` ключем для сущности.
- [#2999035](https://www.drupal.org/node/2999035) `Schema::fieldSetDefault` и `Schema::fieldSetNoDefault` помечены устаревшими в пользу `Schema::changeField`.
- [#2997808](https://www.drupal.org/node/2997808) Добавлены три новых {хука}(hooks:8) `hook_aggregator_fetcher_info_alter()`, `hook_aggregator_parser_info_alter()`, `hook_aggregator_processor_info_alter()` для редактирования плагинов модуля Aggregator.
- [#2996668](https://www.drupal.org/node/2996668) Добавлено новое свойства для элемента формы `#label_for`.
- [#2999418](https://www.drupal.org/node/2999418) Хранилище секций Layout Builder модуля теперь могут предоставлять свои вкладки (local tasks).
- [#3000572](https://www.drupal.org/node/3000572) MySQL `Schema::renameTable()` теперь ввсегда возвращает `NULL`.
- [#2992821](https://www.drupal.org/node/2992821) Изменены проверки прав доступа к формам создания, редактирования и удаление пользоватлей. Теперь они более гибкие.
- [#2986827](https://www.drupal.org/node/2986827) В построитель запросов добавлен новый метод-условие `alwaysFalse()`, который позволяет "обнулить" результат запроса.
- [#3000819](https://www.drupal.org/node/3000819) Атрибут `data-drupal-link-system-path` теперь устанавливается только для системных путей и {маршрута}(routing:8) `<front>`.
- [#3001134](https://www.drupal.org/node/3001134) Статус "модерирования" по умолчанию для модуля workflow теперь можно изменять из администартивного интерфейса.
- [#3003360](https://www.drupal.org/node/3003360) `KernelTestBase::installSchema()` теперь будет выдавать уведомление о устаревшем API.
- [#3001224](https://www.drupal.org/node/3001224) Вкладка ревизий теперь отображается даже при 1 ревизии.
- [#3000051](https://www.drupal.org/node/3000051) Функция `drupal_http_header_attributes()` помечена устаревшей. Используйте `HtmlResponseAttachmentsProcessor::formatHttpHeaderAttributes()`.
- [#3001535](https://www.drupal.org/node/3001535) Добавлены новые методы `get()` и `getMultiple()` для класс `Drupal\migrate\Row`.
- [#3001533](https://www.drupal.org/node/3001533) Аргумент `$process_plugin_manager` удален для `Drupal\migrate\Plugin\migrate\process\MigrationLookup::_construct`.
- [#2997500](https://www.drupal.org/node/2997500) Функция `db_ignore_replica()` помечена устаревшей и переработана в сервис `\Drupal::service('database.replica_kill_switch')->trigger();`.
- [#3002321](https://www.drupal.org/node/3002321) Добавлен трейт `DeprecatedServicePropertyTrait` для упрощения пометок "устаревших" сервисов в свойствах объектов.
- [#2756875](https://www.drupal.org/node/2756875) Функция `drupal_check_incompatibility()` и `\Drupal\Core\Extension\ModuleHandler::parseDependency()` перемещены в `Drupal\Core\Extension\Dependency`.
- [#3001283](https://www.drupal.org/node/3001283) Метод `Drupal\migrate\Plugin\migrate\process\MigrationLookup::skipOnEmpty()` удален. Используйте `::skipInvalid()`.
- [#3001247](https://www.drupal.org/node/3001247) Плагин обработчик `migration_lookup` для Migrate API теперь принимает 0 как допустимое значение.
- [#2993171](https://www.drupal.org/node/2993171) CSS больше не "импортирует" стили на страницах, а использует `<link>` элемент. Стоит расценивать данное изменение как прекращение поддержкие IE 9 и 10 версии. Если вам требуется данная возможность, рекомендуется использовать модуль [IE9 Compatibility](https://www.drupal.org/project/ie9).
- [#3006487](https://www.drupal.org/node/3006487) Плагин источник `d6/VariableTranslation` помечен устаревшим. Вместо `variable_translation` используйте `d6_variable_translation`.
- [#2982512](https://www.drupal.org/node/2982512) Добавлена поддержка объявление полей бандлов в коде при помощи `FieldDefinition`.
- [#2979986](https://www.drupal.org/node/2979986) Функция `taxonomy_check_vocabulary_hierarchy()` и `\Drupal\taxonomy\VocabularyInterface::setHierarchy()` помечены устаревшими.
- [#3014010](https://www.drupal.org/node/3014010) `ConfigInstallerInterface` получил новый метод `getSourceStorage()`.
- [#3014611](https://www.drupal.org/node/3014611) `PluralTranslatableMarkup::DELIMITER` помечен устаревшим в пользу `Drupal\Component\Gettext\PoItem::DELIMITER`.
- [#3006470](https://www.drupal.org/node/3006470) {Плагины}(plugins:8) `MigrateField` теперь имеют "вес" (`weight`).
- [#3009286](https://www.drupal.org/node/3009286) Плагины `MigrateField` email, entityreference и number_default перемещены из `Core\Field` в модуль `field`.
- [#2880055](https://www.drupal.org/node/2880055) `datetime` и `datelist` элементы теперь учитывают свойство `#date_timezone`, которое по умолчанию получается при помощи `drupal_get_user_timezone()`.
- [#2925510](https://www.drupal.org/node/2925510) Исправлена ошибка с отображением заголовков у блоков с раскрытыми фильтрами Views.
- [#2986918](https://www.drupal.org/node/2986918) Плагины Views `ViewsDisplayExtender` теперь могут добавлять свою валидацию данных.
- [#3015367](https://www.drupal.org/node/3015367) Добавлен новый {хук}(hooks:8) `hook_entity_preload()`. Данный хук позволяет подменять данные, прежде чем сущности будут загружены, например, ревизию для загрузки.
- [#3009182](https://www.drupal.org/node/3009182) Добавлен новый класс `\Drupal\Core\Utility\TableSort`, который заменяет все функции из `tablesort.inc`, которые теперь помечены устаревшими.
- [#3016699](https://www.drupal.org/node/3016699) Плагины `Block` и `Condition` теперь используют `context_definitions` ключ, для описания своих контекстов, вместо `context`.
- [#3018097](https://www.drupal.org/node/3018097) CSS id для заголовка пагинации теперь имеет уникальный id вместо `pagination-heading`. Для вывода данного значения в шаблонах используйте `{{ heading_id }}`.
- [#3018742](https://www.drupal.org/node/3018742) Медиа сущности больше не доступны по `/media/{id}`, даже для пользователей с пермишеном `view media`. Данное поведение меняется в настройках медиа модуля.
- [#3016262](https://www.drupal.org/node/3016262) Плагины `SectionStorage` для Layout Builder больше не поддерживают возможность указывать свой список секций. Теперь они доллжны использовать `context_definitions` в аннотации.
- [#3012353](https://www.drupal.org/node/3012353) Плагины для хранилиза Layout Section теперь загружаются используя данные из `context_defintions`.
- [#3020367](https://www.drupal.org/node/3020367) `DateTimeIso8601::getDateTime()` теперь работает как задуманно.
- [#3019830](https://www.drupal.org/node/3019830) `$file->url()` помечен устаревшим методом, используйте `$file->createFileUrl()`.
- [#3020137](https://www.drupal.org/node/3020137) Идентификатор комментария перемещен в разметку самого комментария.
- [#3009364](https://www.drupal.org/node/3009364) Плагин обработки Migrate API `d6_search_configuration_rankings` признан устаревшим. Используйте `search_configuration_rankings`.
- [#3021135](https://www.drupal.org/node/3021135) Оптимизировано использование маршрутизации для REST модуля.
- [#3018300](https://www.drupal.org/node/3018300) `Action` плагины `EmailAction`, `GotoAction`, `MessageAction` перенесены в неймспейс `Drupal\Core\Action\Plugin\Action`.
- [#3022574](https://www.drupal.org/node/3022574) `LayoutBuilderEntityViewDisplay::getRuntimeSections()` помечен устаревшим.
- [#3022118](https://www.drupal.org/node/3022118) Плагины `SectionStorage` теперь должны реализовывать `isApplicable()`.
- [#3020140](https://www.drupal.org/node/3020140) Layout Builder теперь поставляется с секцией "один ряд".
- [#3024321](https://www.drupal.org/node/3024321) `canonical` ссылки теперь имеют в значениях абсолютные URL, вместо относительных. Хоть абсолютные URL и являются правильными, согласно [RFC6596](https://tools.ietf.org/html/rfc6596), Google их [не поддерживает](https://github.com/GoogleChrome/lighthouse/issues/3178).
- 

## Ссылки

- [Change records for Drupal](https://www.drupal.org/list-changes/drupal) (англ)