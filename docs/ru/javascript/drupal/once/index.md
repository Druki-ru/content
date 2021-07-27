---
title: 'Drupal JavaScript Once API'
slug: drupal-once
metatags:
  title: 'Drupal JavaScript Once API (@drupal/once)'
  description: '@drupal/once — JavaScript библиотека отмечающая DOM элементы как обработанные для предотвращения многократных инициализаций.'
---

**@drupal/once** — JavaScript библиотека отмечающая DOM элементы как обработанные для предотвращения многократных инициализаций.

## Введение

В JavaScript написанном в [Drupal](../../../drupal/index.md) принято использовать `Drupal.behaviors`. Данная особенность позволяет получать оповещения о готовности документа, а также об изменениях DOM, которые были произведены при помощи Drupal API.

Функции, объявленные через `Drupal.behaviors` вызываются как только документ страницы будет готов (аналог события `DOMContentLoaded`), а также каждый раз как производятся различные вызовы Drupal JS API, например получение AJAX ответа. Это позволяет JS всегда иметь актуальную информацию о текущем документе и его изменениях.

Данная особенность также вызывает проблему, что одна и та же функция может вызваться неограниченное число раз. Например, регистрация события `click` на второй вызов зарегистрируется второй раз, и будет вызывать функцию обратного вызова в момент клика дважды.

Для того чтобы избежать данных проблем, в Drupal используется Once API. Данный API позволяет помечать найденные элементы уникальным идентификатором и предотвращать повторные вызовы функций или событий на данные элементы.

## API

### once

`once(id, selector, [context])` - фильтрует `NodeList` или массив элементов, удаляя оттуда уже обработанные ранее элементы по указанному `id`, тем самым предотвращая повторную обработку элементов.

В качестве результата возвращает массив уникальных элементов, ранее не проходивших фильтрацию по указанному `id`.

Параметры:

- `id` (`string`): Уникальный идентификатор выборки, в пределах которого будет производиться фильтрация уникальных элементов.
- `selector` (`NodeList` | `Array.<Element>` | `Element` | `string`): Список элементов, который необходимо отфильтровать на уникальность.
- (опционально) `context` (`Document` | `Element`), по умолчанию `document`: Элемент, который будет использован как контекст для `querySelectorAll()`.

Примеры использования с множеством элементов:

```javascript
const elements = once('my-once-id', '[data-myelement]');
// NodeList.
once('my-once-id', document.querySelectorAll('[data-myelement]'));
// Array or Array-like of Element.
once('my-once-id', jQuery('[data-myelement]'));
// A CSS selector without a context.
once('my-once-id', '[data-myelement]');
// A CSS selector with a context.
once('my-once-id', '[data-myelement]', document.head);
// Single Element.
once('my-once-id', document.querySelector('#some-id'));
```

Пример использования для получения одного элемента:

```javascript
// Once always returns an array, even when passing a single element. Some
// forms that can be used to keep code readable.
// Destructuring:
const [myElement] = once('my-once-id', document.body);
// By changing the resulting array, es5 compatible.
const myElement = once('my-once-id', document.body).shift();
```

### once.remove

`once.remove(id, selector, [context])` — убирает метку с элемента о его использовании с уникальным идентификатором `id`.

Если данная метка удалена с элемента, он будет отфильтрован заново при последующем подходящем вызове `once()`.

В качестве результата возвращает массив элементов, который ранее были отфильтрованы с указанным `id` и теперь не имеют данной метки.

Параметры:

- `id` (`string`): Уникальный идентификатор выборки.
- `selector` (`NodeList` | `Array.<Element>` | `Element` | `string`): Список элементов, которые необходимо удалить из фильтрации.
- (опционально) `context` (`Document` | `Element`), по умолчанию `document`: Элемент, который будет использован как контекст для `querySelectorAll()`.

Примеры использования:

```javascript
const elements = once.remove('my-once-id', '[data-myelement]');
// NodeList.
once.remove('my-once-id', document.querySelectorAll('[data-myelement]'));
// Array or Array-like of Element.
once.remove('my-once-id', jQuery('[data-myelement]'));
// A CSS selector without a context.
once.remove('my-once-id', '[data-myelement]');
// A CSS selector with a context.
once.remove('my-once-id', '[data-myelement]', document.head);
// Single Element.
once.remove('my-once-id', document.querySelector('#some-id'));
```

### once.filter

`once.filter(id, selector, [context])` - позволяет отфильтровать элементы, которые были обработаны с указанным `id`.

Работает как `once()` и `remove()` но не вносит никаких изменений в элемент.

В качестве результата возвращает массив уникальных элементов, прошедших через `once()` с указанным `id`.

Параметры:

- `id` (`string`): Уникальный идентификатор выборки.
- `selector` (`NodeList` | `Array.<Element>` | `Element` | `string`): Список элементов, которые необходимо удалить из фильтрации.
- (опционально) `context` (`Document` | `Element`), по умолчанию `document`: Элемент, который будет использован как контекст для `querySelectorAll()`.

Примеры использования:

```javascript
const filteredElements = once.filter('my-once-id', '[data-myelement]');
// NodeList.
once.filter('my-once-id', document.querySelectorAll('[data-myelement]'));
// Array or Array-like of Element.
once.filter('my-once-id', jQuery('[data-myelement]'));
// A CSS selector without a context.
once.filter('my-once-id', '[data-myelement]');
// A CSS selector with a context.
once.filter('my-once-id', '[data-myelement]', document.head);
// Single Element.
once.filter('my-once-id', document.querySelector('#some-id'));
```

### once.find

`once.find([id], [context])` — находит элементы в пределах контекста прошедшие обработку с указанным `id`.

Параметры:

- (опционально) `id` (`string`): Идентификатор по которому элементы прошли обработку.
- (опционально) `context` (`Document` | `Element`), по умолчанию `document`: Элемент, который будет использован как контекст для `querySelectorAll()`.

Примеры использования:

```javascript
const oncedElements = once.find('my-once-id');
// Call without parameters, return all elements with a `data-once` attribute.
once.find();
// Call without a context.
once.find('my-once-id');
// Call with a context.
once.find('my-once-id', document.head);
```

## once в Drupal

Данная [библиотека](../../../drupal/9/libraries/index.md) (`core/once`) поставляется в Drupal начиная с [версии 9.2.0](../../../drupal/releases/9/9.2.x/9.2.0/index.md).

Для того чтобы использовать её, укажите в качестве зависимости для своей библиотеки:

```yaml
# mymodule.libraries.yml
myfeature:
  js: 
    js/myfeature.js: {}
  dependencies:
    - core/drupal
    - core/once
```

```javascript
# js/myfeature.js
(function (Drupal, once) {
  Drupal.behaviors.myfeature = {
    attach(context) {
      const elements = once('myfeature', '.myfeature', context);
    }
  };
}(Drupal, once));
```

## Ссылки

- [@drupal/once](https://www.npmjs.com/package/@drupal/once), npmjs.com