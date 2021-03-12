---
id: create-event
title: Создание события
path: /9/events/create
core: 9
metatags:
  title: 'Drupal 9: Создание события'
  description: 'Создание события и его вызов в Drupal 9.'
category:
  area: События
  title: Создание и вызов
  order: 2
---

Создание события состоит из описания объекта события, а также, вызова события в нужный момент с передачей данного объекта.

## Создание объекта события

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

## Вызов события

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

## Рекомендации

### Регистрация названий событий

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

## Ссылки

- [Drupal 8: Events — создание и использование событий](https://niklan.net/blog/170), Niklan, 2018