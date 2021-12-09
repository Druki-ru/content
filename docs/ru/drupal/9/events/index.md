---
title: События
slug: wiki/9/events
core: 9
search-keywords:
  - как вызвать событие
  - подписчик на событие
  - как подписаться на событие
metatags:
  title: 'Drupal 9: События'
  description: 'События в Drupal 9 позволяют подписываться на определенные этапы работы системы.'
---

## Введение

**События** (англ. Events) — способ внедрения в обработку запроса на различных этапах работы Drupal.

Каждое событие имеет уникальное имя в виде строки, а также объекта, который содержит всю необходимую информацию для данного события.

События предоставляются как Symfony, ядром Drupal, так и сторонними модулями.

Их можно разделить на 3 части:

- **Event** — объект события, содержит всю важную информацию для своего события.
- **Event Dispatcher** — [сервис](../services/index.md), позволяющий производить вызов события.
- **Event Subscriber** — сервис, подписывающийся на различные события и обрабатывающий их.

## Создание события

Создание события состоит из описания объекта события, а также, вызова события в нужный момент с передачей данного объекта.

### Создание объекта события

Объект события передается в момент вызова события всем, кто подписался на него. Он позволяет передать всю необходимую информацию, какая может оказаться полезной.

События всегда получают объект события, но событие необязательно должно описывать свой объект. Если цель вашего события лишь сообщить, что что-то произошло в системе, и оно не несет никакой полезной нагрузки в виде данных, данный этап можно пропустить. В данном случае будет создан стандартный `Symfony\Component\EventDispatcher\Event`.

Во всех остальных случаях, для определенного типа событий создаётся свой собственный, который расширяет `Symfony\Component\EventDispatcher\Event`.

Пример объекта события:

```php
<?php

namespace Drupal\foo\Event;

use Symfony\Component\EventDispatcher\Event;

/**
 * Provides custom event object.
 */
class FooEvent extends Event {

  /**
   * Some event started.
   */
  const EVENT_STARTED = 'foo.something.started';

  /**
   * The foo value.
   *
   * @var string
   */
  protected $foo;

  /**
   * Constructs a FooEvent object.
   *
   * @param string $foo
   *   The foo value.
   */
  public function __construct($foo) {
    $this->foo = $foo;
  }

  /**
   * Gets foo value.
   *
   * @return string
   *   The foo value.
   */
  public function getFoo() {
    return $this->foo;
  }

}
```

Таким образом мы создали объект `FooEvent` и подготовили для него название `foo.something.started`. Обратите внимание, что название события задаётся в константе объекта. Обращение к событиям, как правило, происходит при помощи `EventObject::EVENT_NAME`, поэтому, это является хорошей практикой.

### Вызов события

Когда у вас есть объект вашего события, вам его необходимо вызывать. Для вызова события используется [сервис](../../services/index.md) `event_dispatcher`.

Сервис `event_dispatcher` содержит всего один единственный метод `dispatch()`, который принимает в качестве аргументов:

- `$event_name`: Строковое название события.
- `Event $event`: (опционально) Объект события, который расширяет `Symfony\Component\EventDispatcher\Event`.

Пример вызова события:

```php
$event = new FooEvent('Hello World');

/** @var \Drupal\Component\EventDispatcher\ContainerAwareEventDispatcher $dispatcher */
$dispatcher = \Drupal::service('event_dispatcher');
$dispatcher->dispatch(FooEvent::EVENT_STARTED, $event);
```

### Рекомендация регистрации событий

В примере выше мы создали константу `EVENT_STARTED` с названием события непосредственно в объекте события. Хорошей практикой является создания отдельного объекта, который содержит названия всех событий объявляемых модулем или определенную группу события. Как правило, подобный объект создают рядом с объектом события и называют `FooEvents`.

Пример:

```php
<?php

namespace Drupal\foo\Event;

/**
 * Provides list of foo events..
 */
final class FooEvents {

  /**
   * Some event started.
   */
  const EVENT_STARTED = 'foo.something.started';

}
```

## Подписка на событие

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

Далее, регистрируем объект в качестве [сервиса](../../services/index.md) с меткой:

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