---
title: Подписка на событие
slug: 8/events/subscribe
core: 8
metatags:
  title: 'Drupal 8: Подписка на событие'
  description: 'Подписка на событие Drupal 8.'
category:
  area: События
  title: Подписка на событие
  order: 3
---

Подписка на событие осуществляется при помощи [создания](../../services/create/index.md) [сервиса](../../services/index.md) с меткой.

Сервис должен реализовывать `Symfony\Component\EventDispatcher\EventSubscriberInterface`. Данный интерфейс требует описать метод `getSubscribedEvents()`, который должен возвращать массив с событиями, на которые необходимо подписаться и что вызывать в случае возникновения события.

Результатом метода может быть:

- `['eventName' => 'methodName']`: Подписка на событие `eventName`, при возникновении которого будет вызван метод `methodName` текущего объекта.
- `['eventName' => ['methodName', $priority]]`: По умолчанию приоритет равен 0. Вы можете влиять на то, как рано или поздно вызовется ваш подписчик относительно остальных в пределах данного события. Подписчики вызываются в порядке убывания приоритета.
- `['eventName' => [['methodName1', $priority], ['methodName2']]]`: Вы можете указывать сколько угодно вызовов для одного события.

Хорошей практикой является создание сервисов-подписчиков в `/src/EventSubsriber`, но это не жесткое требование.

Пример объекта подписчика на событие:

```php
<?php

namespace Drupal\foo\EventSubscriber;

use Drupal\foo\Event\FooEvent;
use Drupal\foo\Event\FooEvents;
use Symfony\Component\EventDispatcher\EventSubscriberInterface;

/**
 * Foo event subscriber
 */
class FooSubscriber implements EventSubscriberInterface {

  /**
   * {@inheritdoc}
   */
  public static function getSubscribedEvents() {
    return [
      FooEvents::EVENT_STARTED => ['doSomething', 100],
    ];
  }

  /**
   * Example for DummyFrontpageEvent::PREPROCESS_HTML.
   */
  public function doSomething(FooEvent $event) {
    $foo_value = $event->getFoo();
  }

}
```

Далее,  регистрируем объект в качестве [сервиса](../../services/index.md) с меткой:

```php
services:
  foo.foo_subscriber:
    class: Drupal\foo\EventSubscriber\FooSubscriber
    tags:
      - { name: event_subscriber }
```

> [!TIP]
> Для быстрой генерации подписчика можно использовать [Drush](../../../../drush/index.md) команду `drush generate event-subscriber`.

## Ссылки

- [Drupal 8: Events — создание и использование событий](https://niklan.net/blog/170), Niklan, 2018
