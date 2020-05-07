---
id: route-access-control
title: Контроль доступа
path: /9/routing/access-control
core: 9
category:
  area: Маршрутизация
  order: 4
search-keywords:
  - доступ защита маршрутов контроллеров права доступа
metatags:
  title: 'Drupal 9: Контроль доступа к маршрутам'
  description: 'Контролируем доступ к маршрутам при помощи _custom_access, или собственного сервиса Access Check.'
---

Каждый маршрут в Drupal должен иметь, по крайней мере одну, проверку доступа. Данные требования указываются в `requirements` разделе маршрута.

В данном разделе маршрута указываются какие Access Check сервисы будут использованы для проверки доступа. Ядро предоставляет достаточное количество данных проверок, ознакомиться с  ними вы можете в материале [структура маршрутов](structure-of-routes.md).

Из предоставленного списка, можно выделить `_custom_access`, который позволяет указать в качестве проверки доступа необходимый объект и его метод. Например: `_custom_access: '\Drupal\foo\Controller\FooController::access'`.

Также вы можете объявлять свои access check сервисы и использовать их в маршрутах.

## Использование _custom_access

Данная проверка доступа подразумевает что вы напишите собственную логику для маршрута. Вы должны указать в качестве значения полный путь, включая неймспейс, до объекта и его метода, который будет вызван для проверки прав доступа к конкретному маршруту.

Пример использования:

```yaml
foo.content:
  path: '/my-custom-page' 
  defaults: 
    _controller: '\Drupal\foo\Controller\FooController::content' 
    _title: 'Hello World'
  requirements: 
    _custom_acess: '\Drupal\foo\Controller\FooController::access' 
```

В качестве результата данный метод должен возвращать экземпляр объекта `Drupal\Core\Access\AccessResultInterface`.

Пример проверки прав доступа:

```php
<?php

namespace Drupal\foo\Controller;

use Drupal\Core\Controller\ControllerBase;
use Drupal\Core\Access\AccessResult;
use Drupal\Core\Session\AccountInterface;

/**
 * An foo controller.
 */
class FooController extends ControllerBase {

  /**
   * Returns a render-able array for a test page.
   */
  public function content() {
    $build = [
      '#markup' => $this->t('Hello World!'),
    ];
    return $build;
  }

  /**
   * Returns access result for controller.
   */
  public function access(AccountInterface $account) {
    return AccessResult::allowedIf(
      $account->hasPermission('do example things')
      && $account->hasPermission('do other things')
    );
  }

}
```

## Написание Access Check сервиса

Разработчики могут описывать свои access check сервисы. Например `_custom_access` выше тоже является access check сервисом.

Отличие access check от `_custom_access` в том, что собственный сервис вы можете применять к любым маршрутам. Он должен быть максимально универсальным в пределах тех маршрутов, к которым предназначен, тогда как в `_custom_access`, скорее всего, вы напишите индивидуальную проверку доступа, которая касается только данного маршрута и больше никакого.

Access check является [сервисом](../services/services.md) и поддерживает все его возможности. Объекты, используемые для access check сервисов рекомендуется создавать в `src/Access`. 

Пример объекта с проверкой доступа:

```php
<?php
namespace Drupal\foo\Access;

use Drupal\Core\Session\AccountInterface;
use Drupal\Core\Access\AccessResult;
use Drupal\Core\Routing\Access\AccessInterface;

/**
 * Checks access for displaying configuration translation page.
 */
class FooAccessCheck implements AccessInterface {

  /**
   * A custom access check.
   *
   * @param \Drupal\Core\Session\AccountInterface $account
   *   Run access checks for this account.
   *
   * @return \Drupal\Core\Access\AccessResultInterface
   *   The access result.
   */
  public function access(AccountInterface $account) {
    // Check permissions and combine that with any custom access checking needed. Pass forward
    // parameters from the route and/or request as needed.
    return ($account->hasPermission('do example things') && $this->someOtherCustomCondition()) ? AccessResult::allowed() : AccessResult::forbidden();
  }

}
```

Метод должен возвращаться экземпляр объекта `Drupal\Core\Access\AccessResultInterface` в качестве результата проверки прав доступа.

В качестве аргументов метод проверки доступа принимает:

- Первым делом все динамические части маршрута `{name}`. Они должны идти в том же порядке что и в `path` и теми же названиями.
- Любой из данных объектов в любом порядке:
  - `\Symfony\Component\HttpFoundation\Request $request`.
  - `\Symfony\Component\Routing\Route $route`
  - `\Drupal\Core\Routing\RouteMatch $route_match`
  - `\Drupal\Core\Session\AccountInterface $account`

Далее вам необходимо объявить данный объект как сервис, например:

```yaml
services:
  foo.access_checker:
    class: Drupal\foo\Access\FooAccessCheck
    tags:
      - { name: access_check, applies_to: _foo_access_check }
```

После чего вы можете применять его к любым маршрутам на сайте:

```yaml
foo.content:
  path: '/my-custom-page' 
  defaults: 
    _controller: '\Drupal\foo\Controller\FooController::content' 
    _title: 'Hello World'
  requirements: 
    _foo_access_check: 'TRUE' 
```

## Ссылки

- [Drupal 8: Сервис access_check — гибкая и переиспользуемая проверка прав доступа к маршрутам](https://niklan.net/blog/199), Niklan, 2019
