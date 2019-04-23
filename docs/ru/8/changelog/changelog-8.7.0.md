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

## Прочие изменения

- [#2988067](https://www.drupal.org/node/2988067) Новый интерфейс `SynchronizableInterface` доступе для всех типов сущностей.
- [#2981627](https://www.drupal.org/node/2981627) В `EntityAdapter` добавлен семантический метод `getEntity()` в дополнение к `getValue()`.


## Ссылки

- [Change records for Drupal](https://www.drupal.org/list-changes/drupal) (англ)