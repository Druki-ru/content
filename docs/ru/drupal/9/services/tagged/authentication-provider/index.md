---
title: Authentication Provider
slug: wiki/9/authentication-provider
core: 9
search-keywords:
  - authentication_provider
  - authentication provider
  - идентификация
  - авторизация
  - аутентификация
metatags:
  title: 'Drupal 9: Authentication Provider (Провайдер аутентификации)'
  description: 'Сервисы с меткой authentication_provider позволяют идентифицировать пользователей сайта.'
---

**Authentication Provider** — [сервисы](../../index.md) с меткой `authentication_provider` позволяющие идентифицировать пользователей сайта.

## Введение

**Authentication Provider** используется Drupal для идентификации пользователей сайта. Это основной элемент авторизации в Drupal.

Благодаря данным сервисам есть возможность предоставлять уникальные и собственные способы авторизации, например:

- Авторизация при помощи API ключа в запросе.
- Автоматическая авторизация с определённых IP адресов.

Провайдеры аутентификации могут быть как глобальными, и будут срабатывать на каждое обращение к сайту, так и точечными и подключаться только к необходимым [маршрутам](../../../routing/index.md).

## Стандартные Authentication Provider

Drupal ядро предоставляет следующие провайдеры аутентификации:

- `basic_auth.authentication.basic_auth` (`basic_auth`) (модуль Basic Auth): Данный провайдер позволяется авторизоваться на сайте с использованием Basic Auth авторизации при помощи [HTTP заголовка](https://developer.mozilla.org/ru/docs/Web/HTTP/%D0%97%D0%B0%D0%B3%D0%BE%D0%BB%D0%BE%D0%B2%D0%BA%D0%B8/Authorization).
- `user.authentication.cookie` (`cookie`) (модуль User): Провайдер, который используется для основной авторизации на сайте при помощи пары логина и пароля. Данный провайдер производит идентификацию пользователя по кукам, которые добавляются при успешной авторизации.

## Создание Authentication Provider

Класс Authentication Provider должен реализовывать `Drupal\Core\Authentication\AuthenticationProviderInterface` и быть объявлен как сервис с меткой.

Класс должен реализовывать два обязательных метода:

- `applies()`: Должен вернуть логическое значение `TRUE`, если данный Authentication Provider подходит для текущего запроса, `FALSE`, если нет.
- `authenticate()`: Должен вернуть объект `\Drupal\Core\Session\AccountInterface` с пользователем если он был идентифицирован или `NULL`, если определить пользователя не удалось.

Пример идентификации пользователя по API ключу в параметрах запроса:

```php
<?php

namespace Drupal\example\Authentication\Provider;

use Drupal\Core\Authentication\AuthenticationProviderInterface;
use Drupal\Core\Entity\EntityTypeManagerInterface;
use Drupal\Core\Session\AccountInterface;
use Symfony\Component\HttpFoundation\Request;

/**
 * Provides authentication provider based on API key in query.
 */
final class ApiKeyQueryProvider implements AuthenticationProviderInterface {

  /**
   * The entity type manager.
   *
   * @var \Drupal\Core\Entity\EntityTypeManagerInterface
   */
  protected $entityTypeManager;

  /**
   * {@inheritdoc}
   */
  public function __construct(EntityTypeManagerInterface $entity_type_manager) {
    $this->entityTypeManager = $entity_type_manager;
  }

  /**
   * {@inheritdoc}
   */
  public function applies(Request $request): bool {
    return $request->query->has('api-key') && !empty($request->query->get('api-key'));
  }

  /**
   * {@inheritdoc}
   */
  public function authenticate(Request $request): ?AccountInterface {
    $user_storage = $this->entityTypeManager->getStorage('user');
    $result = $user_storage->getQuery()
      ->condition('field_api_key', $request->query->get('api-key'))
      ->execute();

    // No users with that key found.
    if (empty($result)) {
      return NULL;
    }

    $user_id = reset($result);
    return $user_storage->load($user_id);
  }

}
```

Затем необходимо объявить данный класс как сервис с меткой. Authentication Provider могут и должны предоставлять дополнительные значения при объявлении, в противном случае, они будут пропускаться.

Список дополнительных значений для сервиса:

- `provider_id`: (обязательно) Машинное имя для провайдера, оно будет использоваться для отсылки к нему.
- `global`: Логическое значение, является ли данный провайдер глобальным или нет. Если задано `TRUE`, то данный провайдер будет вызываться на каждый запрос, если `FALSE`, то только на те маршруты, где явно указан его `provider_id`. По умолчанию `FALSE`.

Пример объявления класса выше в виде глобального провайдера с идентификатором `api_key_query`:

```yaml
services:
  authentication.example.api_key_query:
    class: Drupal\example\Authentication\Provider\ApiKeyQueryProvider
    arguments: ['@entity_type.manager']
    tags:
      - { name: authentication_provider, provider_id: api_key_query, global: TRUE }
```

## Использование провайдеров на маршрутах

[Маршруты](../../../routing/index.md) в Drupal имеют опцию `_auth`, содержащую список провайдеров которые будут использованы для идентификации пользователя по данному маршруту.

Drupal автоматически добавляет все глобальные Authentication Provider на все маршруты при их компиляции. Если вы хотите использовать конкретный провайдер авторизации на нужных маршрутах, вам необходимо добавить это в ручном режиме.

Пример объявления маршрута с поддержкой только `api_key_query` Authentication Provider:

```yaml
example.page:
  path: '/example' 
  defaults: 
    _controller: '\Drupal\example\Controller\ExampleController::content' 
    _title: 'Hello World'
  requirements: 
    _permission: 'access content'
  options:
    _auth: ['api_key_query']
```

В примере выше, идентификация пользователя будет происходить с использованием только `api_key_query` провайдера. Это значит, что стандартная система авторизации (`cookie`) не сработает, и если `api_key_query` не сможет идентифицировать пользователя, то страница будет загружена под гостем.

> [!TIP]
> Если у маршрута указано несколько провайдеров, то они будут выполняться в порядке приоритета, от большего к меньшему. Как только один из них вернёт `AccountInterface` обработка закончится и все последующие не будут вызваны.

## Ссылки

- [Drupal 8: Authentication API — создание Authentication Provider](https://niklan.net/blog/166), Niklan, 2018.
