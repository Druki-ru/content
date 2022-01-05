---
title: Access Check
slug: wiki/9/access-check
core: 9
search-keywords:
  - access_check
  - access check
  - проверка доступа
  - права доступа
  - разрешения
metatags:
  title: 'Drupal 9: Access Check (проверка доступа)'
  description: 'Сервисы с меткой access_check предоставляют различные проверки доступа.'
---

**Access Check** — [сервисы](../../index.md) с меткой `access_check` предоставляют различные проверки доступа.

## Введение

**Access Check** используется Drupal для проверки прав доступа в различных ситуациях.

Данные сервисы тесно связаны с [маршрутизацией](../../../routing/index.md) и используются при объявлении маршрутов для контроллеров. Это основное их применение и назначение, но они могут быть использованы и за пределами маршрутизации.

## Стандартные Access Check

Drupal ядро предоставляет следующие проверки доступа:

- **Drupal Core:**
  - `access_check.default` (`_access`): Предоставляет доступ к маршруту на основе логического значения.
  - `access_check.entity` (`_entity_access`): Представляет общую проверку доступа на основе сущности. В качестве значения ожидает `[entity_type_id].[operation]`, например `node.update`.
  - `access_check.entity_bundles` (`_entity_bundles`): Предоставляет проверку что маршрут содержит сущность определенного подтипа. Например `node:news|page`.
  - `access_check.entity_create` (`_entity_create_access`): Предоставляет доступ если у пользователя есть права на создания определённой сущности. Например `user`.
  - `access_check.entity_create_any` (`_entity_create_any_access`): Предоставляет доступ если у пользователя есть права на создание любой сущности любого подтипа. Например `node`.
  - `access_check.entity_delete_multiple` (`_entity_delete_multiple_access`): Предоставляет доступ если у пользователя имеются права на массовое удаление сущности конкретного типа. Например `node`.
  - `access_check.theme` (`_access_theme`): Предоставляет доступ если у пользователя имеются права на просмотр конкретной темы оформления.
  - `access_check.custom` (`_custom_access`): Предоставляет доступ на основе результата вызова функции обратного вызова указанной в качестве значения. Например `Drupal\foo\AccessCheck::access`.
  - `access_check.csrf` (`_csrf_token`): Предоставляет проверку доступа на основе CSRF токена в параметрах запроса.
  - `access_check.header.csrf`: Предоставляет проверку доступа на основе CSRF токена в заголовке запроса.
- **Book:**
  - `access_check.book.removable` (`_access_book_removable`): Производит проверку, может ли запрошенная нода быть удалена из подшивки.
- **Config Translation:**
  - `config_translation.access.overview` (`_config_translation_overview_access`): Производит проверку, может ли текущий пользователь получить доступ к странице обзора перевода конфигураций.
  - `config_translation.access.form` (`_config_translation_form_access`): Производит проверку доступа к формам добавления, редактирования или удаления переводов.
- **Contact:**:
  - `access_check.contact_personal` (`_access_contact_personal_tab`): Производит проверку доступа к контактной странице пользователя.
- **Content Moderation:**:
  - `access_check.latest_revision` (`_access_check.latest_revision`): Производит проверки доступа к вкладке с последними ревизиями содержимого.
- **Content Translation:**:
  - `content_translation.delete_access` (`_access_content_translation_delete`): Производит проверку, может ли пользователь удалить перевод.
  - `content_translation.overview_access` (`_access_content_translation_overview`): Производит проверку, может ли пользователь просматривать переводы содержимого.
  - `content_translation.manage_access` (`_access_content_translation_manage`): Производит проверку, имеет ли пользователь доступ к CRUD операциям.
- **Field UI:**
  - `access_check.field_ui.view_mode` (`_field_ui_view_mode_access`): Производит проверку, может ли пользователь настраивать отображение полей.
  - `access_check.field_ui.form_mode` (`_field_ui_form_mode_access`): Производит проверку, может ли пользователь настраивать отображение формы.
- **JSON:API:**
  - `access_check.jsonapi.relationship_field_access` (`_jsonapi_relationship_field_access`): Проверяет доступа на основе связанных маршрутов.
- **Layout Builder:**
  - `access_check.entity.layout_builder_access` (`_layout_builder_access`): Производит проверку, может ли пользователь использовать Layout Builder.
- **Media:**
  - `access_check.media.revision` (`_access_media_revision`): Производит проверку, может ли пользователь просматривать ревизии мультимедиа.
- **Node:**
  - `access_check.node.revision` (`_access_node_revision`): Производит проверку, может ли пользователь просматривать ревизии содержимого.
  - `access_check.node.add` (`_node_add_access`): Производит проверку, может ли пользователь просматривать страницы создания содержимого.
  - `access_check.node.preview` (`_node_preview_access`): Производит проверку, может ли пользователь использовать предпросмотр содержимого.
- **Quickedit:**
  - `access_check.quickedit.entity_field` (`_access_quickedit_entity_field`): Производит проверку, может ли пользователь редактировать конкретное поле при помощи in-place редактора.
- **Settings Tray:**
  - `access_check.settings_tray.block.has_overrides` (`_access_block_has_overrides_settings_tray_form`): Производит проверку, может ли пользователь редактировать конкретный блок.
  - `access_check.settings_tray.block.settings_tray_form` (`_access_block_plugin_has_settings_tray_form`): Производит проверку, есть ли у блока настройка `settings_tray`.
- **System:**
  - `access_check.cron` (`_access_system_cron`): Производит проверку, может ли пользователь получить доступ к странице запуска [регулярных операций](../../../hooks/cron/index.md).
  - `access_check.db_update` (`_access_system_update`): Производит проверку, может ли пользователь использовать страницу обновления базы данных системы.
- **Update:**
  - `access_check.update.manager_access` (`_access_update_manager`): Производит проверку, включена ли [настройка](../../../settings-php/index.md) `allow_authorize_operations`.
- **User:**
  - `access_check.permission` (`_permission`): Производит проверку на наличие указанных прав доступа у пользователя.
  - `access_check.user.register` (`_access_user_register`): Производит проверку, может ли пользователь получить доступ к маршрутам связанными с регистрацией.
  - `access_check.user.role` (`_role`): Производит проверку на наличие определённой роли(ей) у пользователя.
  - `access_check.user.login_status` (`_user_is_logged_in`): Производит проверку, является ли пользователь авторизованным.
- **Workflows:**
  - `workflows.access_check.extended_permissions` (`_workflow_access`): Производит проверку, имеет ли пользователь доступ к состояниям и переходам.
- **Workspaces:**
  - `access_check.workspaces.active_workspace` (`_has_active_workspace`): Производит проверку, имеются ли у пользователя активные рабочие области.
  
## Создание Access Check

Класс Access Check должен реализовывать `\Drupal\Core\Routing\Access\AccessInterface` и быть объявлен как сервис с меткой.

Интерфейс не выдвигает никаких требований к классу, он используется в качестве проверки, что класс является реализацией данного типа.

Класс должен (в случае использования как проверка для маршрута) реализовывать всего один метод `::access()` (см. `\Drupal\Core\DependencyInjection\Compiler\RegisterAccessChecksPass`, может быть заменён при объявлении сервиса). Данный метод достаточно магический и его аргументы собираются следующим образом:

- Если это Access Check для маршрута, то он должен принимать слаги маршрута в том порядке, в котором они объявлены в пути маршрута. Например `path: '/example/{foo}/{node}'` будут переданы как `access($foo, NodeInterface $node)`.
- Далее, вы можете принять до четырёх дополнительных аргументов в любом порядке, вы должны лишь предоставить корректные тайпхинты:
  - `\Symfony\Component\HttpFoundation\Request $request`
  - `\Symfony\Component\Routing\Route $route`
  - `\Drupal\Core\Routing\RouteMatch $route_match`
  - `\Drupal\Core\Session\AccountInterface $account`

> [!IMPORTANT]
> Не перепутайте `$account` и `$user`! `$account` — текущий пользователь, `$user` — пользователь из маршрута. Если ваша проверка связана с пользователем, убедитесь что вы проводите её для корректного пользователя.

В качестве результата метод должен вернуть объект типа `\Drupal\Core\Access\AccessResultInterface`.

Пример разрешения доступа если ID текущего пользователя равен 17:

```php
<?php

namespace Drupal\example\Access;

use Drupal\Core\Session\AccountInterface;
use Drupal\Core\Access\AccessResult;
use Drupal\Core\Routing\Access\AccessInterface;

/**
 * Provides custom access check.
 */
final class UserIdAccessCheck implements AccessInterface {
  
  /**
   * A custom access check.
   *
   * @param \Drupal\Core\Session\AccountInterface $account
   *   Run access checks for this account.
   *
   * @return \Drupal\Core\Access\AccessResultInterface
   *   The access result.
   */
  public function access(AccountInterface $account): AccessResult {
    return AccessResult::allowedIf($account->id() == 17);
  }

}
```

Затем необходим объявить данный класс как сервис с меткой. Access Check могут опционально передавать следующие значения при объявлении сервиса:

- `applies_to`: Название строки для маршрутов. При указании данной строки маршруту, данный Access Check будет подключен к нему автоматически. Если данное значение не указать, вы должны объявить метод `applies(Route $route)`, который должен вернуть логическое значение, применим ли данный Access Check к маршруту или нет.
- `method`: Название метода, который будет вызван для проверки значения. По умолчанию `access`.
- `needs_incoming_request`: Логичное значение, требуется ли данной проверке текущий запрос (`\Symfony\Component\HttpFoundation\Request $request`). Если указать `TRUE`, то он будет вызываться только когда доступен объект запроса.

Пример объявления сервиса:

```yaml
services:
  access_check.example.user_id:
    class: Drupal\example\Access\UserIdAccessCheck
    tags:
      - { name: access_check, applies_to: _example_user_id }
```

## Использование на маршрутах

Для того чтобы использовать Access Check на маршрутах, их необходимо указать в параметре `requirements` с любым значением. По умолчанию, если Access Check не использует своё значение как аргумент, принято указывать `'TRUE'`.

Пример подключения проверки`_example_user_id` при объявлении маршрута:

```yaml
example.page:
  path: '/example'
  defaults:
    _controller: '\Drupal\example\Controller\ExampleController::buildPage'
    _title: 'Hello World'
  requirements:
    _example_user_id: 'TRUE'
```

## Получение значения из маршрута

Если вы хотите дать возможность передавать примитивные значения при указании требования в маршруте, вы можете получать их из `$route` объекта.

Например, если вы хотите получить значение "foo bar" из `_example_user_id: 'foo bar'`:

```php
$route->getRequirement('_example_user_id');
```

## Смотрите также

- [Маршрутизация](../../../routing/index.md) — основное применение Access Check.

## Ссылки

- [Drupal 8: Сервис access_check — гибкая и переиспользуемая провека прав доступа к маршрутам](https://niklan.net/blog/199), Niklan, 2019.

