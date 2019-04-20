---
id: route-access-control
title: Контроль доступа
path: /docs/8/routing/access-control
core: 8
category-area: Маршрутизация
category-order: 4
search-keywords:
  - доступ защита маршрутов контроллеров права доступа
---

Каждый маршрут в Drupal должен иметь, по крайней мере одну, проверку доступа. Данные требования указываются в `requirements` разделе маршрута.

В данном разделе маршрута указываются какие Access Check сервисы будут использованы для проверки доступа. Ядро предоставляет достаточное количество данных проверок, ознакомиться с  ними вы можете в материале {структура маршрутов}(structure-of-routes:8).

Из предоставленного списка, можно выделить `_custom_access`, который позволяет указать в качестве проверки доступа необходимый объект и его метод. Например: `_custom_access: '\Drupal\foo\Controller\FooController::access'`.

Также вы можете объявлять свои access check сервисы и использовать их в маршрутах.

## Использование _custom_access

Данная проверка доступа подразумевает что вы напишите собственную логику проверки для маршрута. Вы должны указать в качестве значения полный путь, включая неймспейс, до объекта и его метода, который будет вызван для проверки прав доступа к конкретному маршруту.

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
