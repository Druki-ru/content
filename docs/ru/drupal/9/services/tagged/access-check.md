---
id: access-check
title: Access Check
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

**Access Check** — [сервисы](../services.md) с меткой `access_check` предоставляют различные проверки доступа.

## Введение

**Access Check** используется Drupal для проверки прав доступа в различных ситуациях.

Данные сервисы тесно связаны с [маршрутизацией](../../routing/routing.md) и используются при объявлении маршрутов для контроллеров. Это основное их применение и назначение, но они могут быть использованы и за пределами маршрутизации.

## Стандартные Authentication Provider

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

@todo закончить список

