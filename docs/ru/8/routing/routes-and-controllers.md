---
id: routes-and-controllers
title: Маршруты и контроллеры
path: /docs/8/routing/routes-and-controllers
core: 8
category:
  area: Маршрутизация
  order: 2
metatags:
  title: 'Drupal 8: Маршруты и контроллеры'
  description: 'Разбираемся для чего нужен маршрут (роут), а для чего контроллер, и как они дополняют друг друга.'
---

Маршрутизация состоит из двух частей:

1. Роут (маршрут) - содержит информацию о будущей странице, её путь, какой контроллер использовать, заголовок, требования и т.д.
1. Контроллер - объект, отвечающий за формирование результата по запрошенному маршруту.

## Маршруты

Маршруты задаются в специальном файле модуля `*.routing.yml`, который должен находиться в корне модуля рядом с `*.info.yml`. Если ваш модуль имеет название `foo`, в таком случае файл будет называться `foo.routing.yml`.

В данном файле вы объявляете все необходимые вашему модулю маршруты и описываете их при помощи YAML формата. Например:

```yaml
foo.content:
  path: '/my-custom-page' 
  defaults: 
    _controller: '\Drupal\foo\Controller\FooController::content' 
    _title: 'Hello World'
  requirements: 
    _permission: 'access content' 
```

> [!NOTE]
> В Drupal принято, чтобы маршруты имели название в следующем формате `modulename.route_name`.

Данный пример объясняет Drupal, что маршрут с названием `foo.content` должен быть обработан по адресу `/my-custom-page`. Когда пользователь обратится по данному маршруту, для него будет произведена проверка, имеет ли он доступ `access content`. Если не имеет, то пользователь получит ответ HTTP 403 "Недостаточно прав", если же имеет, тогда результат, который возвращает контроллер. В качестве заголовка страницы будет использована строка "Hello World".

## Контроллеры

Drupal 8 использует компонент Symfony - [HTTP Kernel](https://symfony.com/doc/current/components/http_kernel.html), который получает запрос и опрашивает другие системы в надежде получить от них результат, который необходимо вернуть пользователю. Результатом является один из объектов, наследующих `Symfony\Component\HttpFoundation\Response` или непосредственно данный объект. В случае с Drupal, мы также имеем возможность вернуть render array в качестве результата. Drupal обнаружит его, проведет все необходимые обработки и операции с ним, а затем вернет корректный ответ при помощи `Response`.

Например, если мы хотим создать контроллер для примера из маршрутизации выше, то результат может быть примерно таким:

```php
<?php

namespace Drupal\foo\Controller;

use Drupal\Core\Controller\ControllerBase;

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

}
```

В результате чего, Drupal вернет в качестве контента страницы строку "Hello World" на языке пользователя.

> [!NOTE]
> При помощи {Drush}(drush), вы можете быстро генерировать код для контроллеров, для этого воспользуйтесь командой `drush generate controller`.

## Ссылки

- [Drupal 8 Hello World: Пишем свой первый модуль](https://niklan.net/blog/66), Niklan, 2014
