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

### _system_rebuild_module_data() и подобные функции заменены на сервисы

- [#2709919](https://www.drupal.org/node/2709919)

В Drupal имеются некоторые внутренние функции для управления местоположением модулей и тем, обновление данной информации и.т. Данные функции перенесены в {сервисы}(services:8).

- `system_get_info('module')` => `\Drupal::service('extension.list.module')->getAllInstalledInfo()`
- `system_get_info('profile')` => `\Drupal::service('extension.list.profile')->getAllInstalledInfo()`
- `system_get_info('theme')` => `\Drupal::service('extension.list.theme')->getAllInstalledInfo()`
- `system_get_info('theme_engine')` => `\Drupal::service('extension.list.theme_engine')->getAllInstalledInfo()`
- `system_get_info('module', 'module_name')` => `\Drupal::service('extension.list.module')->getExtensionInfo('module_name')`
- `system_get_info('profile', 'profile_name')` => `\Drupal::service('extension.list.profile')->getExtensionInfo('profile_name')`
- `system_get_info('theme', 'theme_name')` => `\Drupal::service('extension.list.theme')->getExtensionInfo('theme_name')`
- `system_get_info('theme_engine', 'theme_engine_name') ` => `\Drupal::service('extension.list.theme_engine')->getExtensionInfo('theme_engine_name')`
- `system_list('theme')` => `\Drupal::service('theme_handler')->listInfo()`
- `system_list_reset()` => `\Drupal::service('extension.list.TYPE')->reset()`
- `system_rebuild_module_data()`:
  - `\Drupal::service('extension.list.module')->reset();`: Сброс информации о модулях.
  - `$module_list = \Drupal::service('extension.list.module')->reset()->getList();`: Сброс информации и получение всех модулей.
  - `$module_list = \Drupal::service('extension.list.module')->getList();`: Получение всех модулей из кэша.
- `system_register()` удален без замены.

### Модуль комментариев больше не запоминает IP адреса для комментариев по умолчанию

- [#2994780](https://www.drupal.org/node/2994780)

Модуль комментариев больше не будет записывать IP адреса по умолчанию при отправке комментариев.

Существующие сайты продолжат логировать IP, но это может быть изменено в **settings.php**.

```php
$conf['comment.settings']['log_ip_addresses'] = FALSE;
```

### Сущность workflow теперь имеет дополнительные операции доступа

- [#2929327](https://www.drupal.org/node/2929327)

Сущность `workflow` теперь поддерживает дополнительные операции прав доступа, которые отражают состояния и переходы.

Список доступных операций состояний и перехода для состояния "foo" и перехода "bar".

- `add-state`: Предоставляет пользователю добавлять новое состояние.
- `update-state:foo`: Предоставляет пользователю права на редактирование состояния "foo".
- `delete-state:foo`: Предоставляет пользователю права на удаление состояния "foo".
- `add-transition`: Предоставляет пользователю права на добавление нового перехода.
- `update-transition:bar`: Предоставляет пользователю права на редактирование перехода "bar".
- `delete-transition:bar`: Предоставляет пользователю права на удаление перехода "bar".

Пример {хука}(hooks:8) определение прав доступа для рабочего процесса `fancy_workflow`:

```php
/**
 * Implements hook_ENTITY_TYPE_access() for the workflow entity.
 */
function fancy_workflows_workflow_access(WorkflowInterface $workflow, $operation, AccountInterface $account) {
  if ($operation === 'add-transition' && $workflow->getTypePlugin()->getPluginId() === 'fancy_workflow') {
    return AccessResult::allowedIfHasPermission($account, 'administer fancy workflow transitions');
  }
}
```

### JavaScript Messages API

- [#2930536](https://www.drupal.org/node/2930536)

Данное изменение представляе новое API для JavaScript, при помощи которого вы можете отображать системные сообщения на странице. По умолчанию, все сообщения будут показаны в том же месте, где и системные сообщения отправленные сервером.

Данный API также позволяет отображать сообщения в других местах на странцие, но для этого коду придется отвечать за HTML разметку элемента, в которое помещается сообщение.

#### Настройка и примеры

В ядро добавлена новая {библиотека}(libraries:8) `drupal.message`, которую необходимо подключать там, где вы хотите использовать данный API.

Пример создания нового экземпляра класс сообщений:

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

## Прочие изменения

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

## Ссылки

- [Change records for Drupal](https://www.drupal.org/list-changes/drupal) (англ)