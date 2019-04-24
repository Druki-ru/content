---
id: changelog-8.7.0
title: 'Drupal 8.7.0'
path: /docs/8/changelog/8.7.0
core: 8
---

## Обновление с 8.6.x

> [!WARNING]
> Более подробная и точная информация появится с релизом. Указаны приблизительные действия для обновления.

Модуль JSON:API теперь в ядре. Необходимо деинсталлировать и удалить данный модуль с проекта (включая {Composer}(composer) зависимость). После обновления, включить данный модуль из ядра.

Если вы использовали данный модуль как зависимость для своих, замените зависимость с `jsonapi:jsonapi` на `drupal:jsonapi`.

## Подробные изменения

### Прекращение поддержи PHP 5.5 и 5.6

- [#2938726](https://www.drupal.org/node/2938726), [#3044409](https://www.drupal.org/node/3044409)

Начиная с релиза Drupal 8.7.0, прекращается официальная поддержка PHP 5.5 и 5.6 версий. Drupal 8.6.x становится последним релизом Drupal 8 с поддержкой PHP 5.

Уже имеющиеся сайты продолжат работать на данных версиях после обновления, но стабильность  не гарантируется, также будет показано предупреждение о старой версии PHP. Установка новых сайтов на PHP ниже 7-й версии будет запрещена.

**Начиная с {Drupal 8.8.0}(changelog-8.8.0:8) минимальная версия PHP 7.0.8**.

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

### Изменения Entity Update API

- [#3029997](https://www.drupal.org/node/3029997), [#3034742](https://www.drupal.org/node/3034742), [#3046576](https://www.drupal.org/node/3046576)

#### Обновление схем

Начиная с Drupal 8.7.0, предоставлен новый Entity Update API для конвертации схемы контент сущностей из одного состояния в другое.

Обратите внимание что если производится обновление для сущностей, где уже имеется содержимое, необходимо использовать `hook_post_update_NAME()`.

Новый API:

- `\Drupal\Core\Entity\EntityDefinitionUpdateManager` получил новый метод `updateFieldableEntityType(EntityTypeInterface $entity_type, array $field_storage_definitions, array &$sandbox = NULL)`.
- `\Drupal\Core\Entity\EntityTypeListenerInterface` получил новый метод `onFieldableEntityTypeUpdate(EntityTypeInterface $entity_type, EntityTypeInterface $original, array $field_storage_definitions, array $original_field_storage_definitions, array &$sandbox = NULL)`.
- `\Drupal\Core\Entity\EntityStorageInterface` получил новый метод `restore(EntityInterface $entity)`.
- Добавлен новый трейт `\Drupal\Core\Entity\Sql\SqlFieldableEntityTypeListenerTrait`.

По умолчанию, перед обновлением схемы, автоматически будут созданы резервные копии изменяемых таблиц в БД, а также сохранены после успешного обновления. Вы можете изменить данное поведение в settings.php `$settings['entity_update_backup'] = FALSE;`.

Примеры конвертации сущностей для поддержки ревизий можно наблюдать в ядре: [#2880149](https://www.drupal.org/project/drupal/issues/2880149), [#2880152](https://www.drupal.org/project/drupal/issues/2880152).

#### Автоматические обновления сущностей удалены

В Drupal 8 был представлен концепт автоматического обновления сущностей, который позволял обновлять схемы сущностей и их полей с минимальными усилиями, например, используя команду {Drush}(drush) `drush entity-updates`.

Тем не менее, на практике, автоматические обновления показали что они опасны, потому что ведут к непредсказуемым результатам, критическим ошибкам. Использование данной возможности было прекращено в ядре ещё до релиза Drupal 8.

Начиная с Drupal 8.7.0 автоматические обновления схем сущностей и полей больше недоступны. Теперь, когда хранилище для данных или сущности нужно создать, обновить или удалить, необходимо прописать это явно, используя [Update API](https://api.drupal.org/api/drupal/core!lib!Drupal!Core!Extension!module.api.php/group/update_api/8.7.x) и [Entity Definition Update Manager](https://api.drupal.org/api/drupal/core!lib!Drupal!Core!Entity!EntityDefinitionUpdateManagerInterface.php/interface/EntityDefinitionUpdateManagerInterface/8.7.x).

Entity Definition Update Manager теперь не поддерживает изменения таблиц, лдя этого необходимо использовать представленный новый API для обновления схем.

#### Примеры

**Установка** нового типа сущности.

```php
/**
 * Implements hook_update_N().
 */
function example_update_8701() {
  \Drupal::entityDefinitionUpdateManager()->installEntityType(new ConfigEntityType([
    'id' => 'rest_resource_config',
    'label' => new TranslatableMarkup('REST resource configuration'),
    'config_prefix' => 'resource',
    'admin_permission' => 'administer rest resources',
    'label_callback' => 'getLabelFromPlugin',
    'entity_keys' => ['id' => 'id'],
    'config_export' => [
      'id',
      'plugin_id',
      'granularity',
      'configuration',
    ],
  ]));
}
```

**Обновление** существующего типа сущности, когда это **не затрагивает актуальную схему**.

```php
function example_update_8701() {
  $definition_update_manager = \Drupal::entityDefinitionUpdateManager();
  $entity_type = $definition_update_manager->getEntityType('comment');
  $keys = $entity_type->getKeys();
  $keys['published'] = 'status';
  $entity_type->set('entity_keys', $keys);
  $definition_update_manager->updateEntityType($entity_type);
}
```

**Обновление** существующего типа сущности, когда это **также меняет схему** (например, делает сущность ревизионной). Обратите внимание что используется `hook_post_update_NAME()`, так как это произведет вызов необходимых событий при изменении свойств сущности `revisionable` и `translatable`.

```php
/**
 * Implements hook_post_update_NAME().
 */
function taxonomy_post_update_make_taxonomy_term_revisionable(&$sandbox) {
  $definition_update_manager = \Drupal::entityDefinitionUpdateManager();
  /** @var \Drupal\Core\Entity\EntityLastInstalledSchemaRepositoryInterface $last_installed_schema_repository */
  $last_installed_schema_repository = \Drupal::service('entity.last_installed_schema.repository');

  $entity_type = $definition_update_manager->getEntityType('taxonomy_term');
  $field_storage_definitions = $last_installed_schema_repository->getLastInstalledFieldStorageDefinitions('taxonomy_term');

  // Update the entity type definition.
  $entity_keys = $entity_type->getKeys();
  $entity_keys['revision'] = 'revision_id';
  $entity_keys['revision_translation_affected'] = 'revision_translation_affected';
  $entity_type->set('entity_keys', $entity_keys);
  $entity_type->set('revision_table', 'taxonomy_term_revision');
  $entity_type->set('revision_data_table', 'taxonomy_term_field_revision');
  $revision_metadata_keys = [
    'revision_default' => 'revision_default',
    'revision_user' => 'revision_user',
    'revision_created' => 'revision_created',
    'revision_log_message' => 'revision_log_message',
  ];
  $entity_type->set('revision_metadata_keys', $revision_metadata_keys);

  // Update the field storage definitions and add the new ones required by a
  // revisionable entity type.
  $field_storage_definitions['langcode']->setRevisionable(TRUE);
  $field_storage_definitions['name']->setRevisionable(TRUE);
  $field_storage_definitions['description']->setRevisionable(TRUE);
  $field_storage_definitions['changed']->setRevisionable(TRUE);

  $field_storage_definitions['revision_id'] = BaseFieldDefinition::create('integer')
    ->setName('revision_id')
    ->setTargetEntityTypeId('taxonomy_term')
    ->setTargetBundle(NULL)
    ->setLabel(new TranslatableMarkup('Revision ID'))
    ->setReadOnly(TRUE)
    ->setSetting('unsigned', TRUE);

  $field_storage_definitions['revision_default'] = BaseFieldDefinition::create('boolean')
    ->setName('revision_default')
    ->setTargetEntityTypeId('taxonomy_term')
    ->setTargetBundle(NULL)
    ->setLabel(new TranslatableMarkup('Default revision'))
    ->setDescription(new TranslatableMarkup('A flag indicating whether this was a default revision when it was saved.'))
    ->setStorageRequired(TRUE)
    ->setInternal(TRUE)
    ->setTranslatable(FALSE)
    ->setRevisionable(TRUE);

  $field_storage_definitions['revision_translation_affected'] = BaseFieldDefinition::create('boolean')
    ->setName('revision_translation_affected')
    ->setTargetEntityTypeId('taxonomy_term')
    ->setTargetBundle(NULL)
    ->setLabel(new TranslatableMarkup('Revision translation affected'))
    ->setDescription(new TranslatableMarkup('Indicates if the last edit of a translation belongs to current revision.'))
    ->setReadOnly(TRUE)
    ->setRevisionable(TRUE)
    ->setTranslatable(TRUE);

  $field_storage_definitions['revision_created'] = BaseFieldDefinition::create('created')
    ->setName('revision_created')
    ->setTargetEntityTypeId('taxonomy_term')
    ->setTargetBundle(NULL)
    ->setLabel(new TranslatableMarkup('Revision create time'))
    ->setDescription(new TranslatableMarkup('The time that the current revision was created.'))
    ->setRevisionable(TRUE);

  $field_storage_definitions['revision_user'] = BaseFieldDefinition::create('entity_reference')
    ->setName('revision_user')
    ->setTargetEntityTypeId('taxonomy_term')
    ->setTargetBundle(NULL)
    ->setLabel(new TranslatableMarkup('Revision user'))
    ->setDescription(new TranslatableMarkup('The user ID of the author of the current revision.'))
    ->setSetting('target_type', 'user')
    ->setRevisionable(TRUE);

  $field_storage_definitions['revision_log_message'] = BaseFieldDefinition::create('string_long')
    ->setName('revision_log_message')
    ->setTargetEntityTypeId('taxonomy_term')
    ->setTargetBundle(NULL)
    ->setLabel(new TranslatableMarkup('Revision log message'))
    ->setDescription(new TranslatableMarkup('Briefly describe the changes you have made.'))
    ->setRevisionable(TRUE)
    ->setDefaultValue('');

  $definition_update_manager->updateFieldableEntityType($entity_type, $field_storage_definitions, $sandbox);

  return t('Taxonomy terms have been converted to be revisionable.');
}
```

**Удаление** типа сущности.

```php
function example_update_8701() {
  $entity_update_manager = \Drupal::entityDefinitionUpdateManager();
  $entity_type = $entity_update_manager->getEntityType('entity_type_id');
  $entity_update_manager->uninstallEntityType($entity_type);
}
```

---

**Установка** нового хранилища поля.

```php
function example_update_8701() {
  $field_storage_definition = BaseFieldDefinition::create('boolean')
    ->setLabel(t('Revision translation affected'))
    ->setDescription(t('Indicates if the last edit of a translation belongs to current revision.'))
    ->setReadOnly(TRUE)
    ->setRevisionable(TRUE)
    ->setTranslatable(TRUE);

  \Drupal::entityDefinitionUpdateManager()
    ->installFieldStorageDefinition('revision_translation_affected', 'block_content', 'block_content', $field_storage_definition);
}
```

**Обновление** существующего хранилища поля.

```php
function example_update_8701() {
  $entity_definition_update_manager = \Drupal::entityDefinitionUpdateManager();
  $field_storage_definition = $entity_definition_update_manager->getFieldStorageDefinition('hostname', 'comment');
  $field_storage_definition->setDefaultValueCallback(Comment::class . '::getDefaultHostname');
  $entity_definition_update_manager->updateFieldStorageDefinition($field_storage_definition);
}
```

**Удаление** существующего хранилища поля.

```php
function example_update_8701() {
  $definition_update_manager = \Drupal::entityDefinitionUpdateManager();
  if ($content_translation_status = $definition_update_manager->getFieldStorageDefinition('content_translation_status', 'taxonomy_term')) {
    $definition_update_manager->uninstallFieldStorageDefinition($content_translation_status);
  }
}
```

Если по каким-то причинам вам нужно старое поведение, воспользуйтесь модулем для разработчиков [Devel Entity Updates](https://www.drupal.org/project/devel_entity_updates).

### Изменения File API

- [#3006851](https://www.drupal.org/node/3006851), [#3042234](https://www.drupal.org/node/3042234)

Следующие функции были помечены устаревшими:

```php
file_unmanaged_copy($source, $destination, $replace)
file_unmanaged_prepare($source, $destination, $replace)
file_unmanaged_move($source, $destination, $replace)
file_unmanaged_delete($path)
file_unmanaged_delete_recursive($path, $callback)
file_unmanaged_save_data($data, $destination, $replace)
file_prepare_directory($dir)
file_destination($destination, $replace)
file_create_filename($basename, $directory)
```

Вместо них необходимо использовать соответствующие методы {сервиса}(services:8) `file_system`.

```php
try {
  \Drupal::service('file_system')->copy($source, $destination, $replace);
  \Drupal::service('file_system')->move($source, $destination, $replace);
  \Drupal::service('file_system')->delete($path);
  \Drupal::service('file_system')->deleteRecursive($path, $callback);
  \Drupal::service('file_system')->saveData();
  \Drupal::service('file_system')->prepareDirectory($directory, $options);
  \Drupal::service('file_system')->getDestinationFilename($destination, $replace);
  \Drupal::service('file_system')->createFilename($basename, $directory);
} 
catch (\Drupal\Core\File\Exception\FileException $e) {
  // Log or set message or doing something else.
}
```

Соответветствующим образом константы `FILE_EXISTS_RENAME`, `FILE_EXISTS_REPLACE` и `FILE_EXISTS_ERROR` помечены устаревшими. Вместо них используйте константы интерфейса `FileSystemInterface::EXISTS_RENAME`, `FileSystemInterface::EXISTS_REPLACE`, `FileSystemInterface::EXISTS_ERROR`.

Изменения также коснулись аргумента `$destination` для методов `copy()`, `move()` и `saveData()` - теперь они обязательны. Если вы хотите сохранить старое поведение, сохраняя файлы в корень основной директории, вы можете использовать `'public://'` или `file_default_scheme() . '://'` в качестве значения аргумента.

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
- [#3011154](https://www.drupal.org/node/3011154) Процедурная функция `twig_without()` помечена устаревшей. Используйте сервис `twig.extension` и метод `without()`. Это изменение **не касается** Twig фильтра `without`.
- [#3001185](https://www.drupal.org/node/3001185) Сервис `session_handler.write_check` удален из `core.services.yml`.
- [#2934242](https://www.drupal.org/node/2934242) Хуки `hook_test_group_started()`, `hook_test_group_finished()`, `hook_test_finished()` помечены устаревшими.
- [#2946161](https://www.drupal.org/node/2946161) `ConfigurablePluginInterface` помечен устаревшим в пользу `ConfigurableInterface`, `DependentPluginInterface`.
- [#3029284](https://www.drupal.org/node/3029284) `RevisionableInterface`, `TranslatableInterface` и прочие интерфейыс, предназначенные для сущностей, теперь расширяют `EntityInterface`.
- [#3029850](https://www.drupal.org/node/3029850) Добавлен новый рендер елемент `layout_builder` для отображения интерфейса Layout Builder.
- [#3030415](https://www.drupal.org/node/3030415) Разрешение "Use the administration toolbar" переименовано в "Use the toolbar".
- [#3021276](https://www.drupal.org/node/3021276) Добавлена новая Ajax команда `AnnounceCommand`.
- [#3031697](https://www.drupal.org/node/3031697) Доступ к данным Layout Builder при помощи REST закрыт, до тех пор, пока не [будет решено](https://www.drupal.org/project/drupal/issues/2942975), как правильно его отдавать.
- [#3032274](https://www.drupal.org/node/3032274) Добавили хук для управления формами Layout Builder.
- [#3042512](https://www.drupal.org/node/3042512) А потом его удалили. 🤫 Ведь у нас уже есть `hook_entity_form_display_alter()`. 💅
- [#2997196](https://www.drupal.org/node/2997196) Добавлен новый интерфейс `EmailValidatorInterface`, для тайпхинтинга сервиса `email.validator`.
- [#2955581](https://www.drupal.org/node/2955581) Исправлина нормализация для полей "Date" и "Date range", которые настроены на хранение "Даты и времени" или "Только даты".
- [#3030634](https://www.drupal.org/node/3030634) Трейт `SerializationTrait` помечен устаревшим.
- [#3035096](https://www.drupal.org/node/3035096) Секции лейаутов теперь могут иметь third-party settings.
- [#3002434](https://www.drupal.org/node/3002434) Переменная `attributes` в `hook_process_html()` теперь всегда будет массивом.
- [#3035507](https://www.drupal.org/node/3035507) Названия файлов теперь также получаются суффикс `_NUMBER` если были переименованы при помощи `file_save_upload()` и `FILE_EXISTS_RENAME`.
- [#3035954](https://www.drupal.org/node/3035954) CSS классы для Layout Builder теперь следуют [BEM стандартам](http://getbem.com/introduction/). 👀
- [#2925634](https://www.drupal.org/node/2925634) Рендер базовых полей для сущности node теперь учитывает настройки полей. Таким образом, вы можете выключить поля `node.title`, `node.uid`, `node.created` и всю соответствующую обработку.
- [#3035166](https://www.drupal.org/node/3035166) Списки секций теперь различают пустые секции, которы были удалены от тех, что только что были созданы.
- [#3036709](https://www.drupal.org/node/3036709) Доступен новый хелпер метод для подключения сервиса 'current_user' в Kernel тестах.
- [#3036823](https://www.drupal.org/node/3036823) `Drupal\Component\DependencyInjection\Container` больше не реализует `Symfony\Component\DependencyInjection\ResettableContainerInterface`. В дальнейшем планируется реализовывать `Symfony\Component\DependencyInjection\ResettableContainerInterface`, когда будет решено, какой версии будет Symfony (4 или 5) в Drupal 9.
- [#3036722](https://www.drupal.org/node/3036722) Доступен новый API для управления вариантами сущности.
- [#3006076](https://www.drupal.org/node/3006076) `Drupal\migrate_drupal\Plugin\migrate::PLUGIN_METHOD` помечен устаревшим.
- [#2954670](https://www.drupal.org/node/2954670) Добавлен новый сервис `migrate_drupal.field_discovery`, который упрощает процесс обнаружения и добавления обработчиков для полей при миграции `node` из Drupal 6 и сущностей с полями из Drupal 7.
- [#2961643](https://www.drupal.org/node/2961643) Сериализуемые свойства полей теперь остаются в сериализованном состоянии при загрузки из хранилища.
- [#3036689](https://www.drupal.org/node/3036689) Тесты ядра теперь устанавливают все необходимые сущности, прежде чем устанавливать любые другие конфигурации.
- [#3029856](https://www.drupal.org/node/3029856) Аннотации `ContextDefinition` теперь могут указывать ограничения (`constraints`).
- [#3039034](https://www.drupal.org/node/3039034) Ссылки в меню теперь поддерживают ревизионность.
- [#2897789](https://www.drupal.org/node/2897789) Термины таксономии теперь ревизионные.
- [#3039683](https://www.drupal.org/node/3039683) `\Drupal\Core\Validation\TranslatorInterface` больше не расширяет `\Symfony\Component\Translation\TranslatorInterface`.
- [#3041438](https://www.drupal.org/node/3041438) Новый стабильный модуль JSON:API в ядре.
- [#3042154](https://www.drupal.org/node/3042154) Произведено обновление PHP зависимостей.
- [#3040966](https://www.drupal.org/node/3040966) Хранилища контент сущностей и `EntityQuery` теперь используют последнее установленное описание сущности и хранилища полей, а не активное на данный момент.
- [#3042396](https://www.drupal.org/node/3042396) Содержимое для oEmbed теперь может быть адаптивным.
- [#3043523](https://www.drupal.org/node/3043523) Media сущности теперь имеют новое разрешение для "просмотра неопубликованного содержимого".
- [#3043164](https://www.drupal.org/node/3043164) Поле-хранилище Layout Builder теперь не переводимое.
- [#3043944](https://www.drupal.org/node/3043944) Объект `InlineBlockUsage` теперь реализует `InlineBlockUsageInterface`.
- [#3043943](https://www.drupal.org/node/3043943) Объект `LayoutBuilderSampleEntityGenerator` теперь реализует `SampleEntityGeneratorInterface`.
- [#3003756](https://www.drupal.org/node/3003756) Обновлен Drupal Coder/PHPCS.
- [#3047051](https://www.drupal.org/node/3047051) Заголовок и описание ссылки теперь могут быть изменены в отличных от стандартных Workspaces, а также в формах редактирования `node` сущности при использовании content moderation.

## Ссылки

- [Change records for Drupal](https://www.drupal.org/list-changes/drupal) (англ)