---
title: Internal Page Cache
slug: wiki/9/page-cache
metatags:
  title: 'Drupal 9: Internal Page Cache'
  description: Internal Page Cache — статическое кеширование страниц для анонимных пользователей.
---

**Internal Page Cache** — стандартный модуль кеширования страниц для анонимных пользователей.

> [!TIP]
> Данный модуль включён по умолчанию в [стандартном профиле](../distributions/standard/index.md).

> [!NOTE]
> Не путайте данный модуль с [Internal Dynamic Page Cache](../dynamic-page-cache/index.md).

## Введение

Internal Page Cache — отвечает за статическое кеширование страниц для анонимных пользователей.

При обработке ответа сайта на запрос, данный модуль сохраняет результат ответа «как есть» в cache bin `page`.

При обработке входящего запроса он проверяет, есть ли кеш для входящего запроса и если он есть, сразу отдаёт его в качестве ответа прерывая все последующие операции.

В отличие от [Internal Dynamic Page Cache](../dynamic-page-cache/index.md), данный тип кеша работает на уровне Middleware API — это означает что он сработает на самых ранних этапах и если найдёт кеш, многие Drupal API даже не будут запущены.

### Преимущества

* Работает на очень ранних этапах и хранит готовый статичный ответ, не требующий дополнительной обработки, что позволяет получать TTFB < 10ms.
* Если включен модуль [Internal Dynamic Page Cache](../dynamic-page-cache/index.md) и Internal Page Cache не сможет обработать запрос, то IDPC его подстрахует. Они лишь дополняют друг друга.

### Недостатки

* Данный кеш не поддерживает кеш-контексты.
* Данный кеш учитывает полный URL при формировании CID (включая query параметры). Это может привести к тому, что кеш таблицу раздует до огромных размеров если сайт большой.
* Данный кеш работает только для анонимных пользователей, так как авторизованные пользователи уже могут иметь динамическое содержимое. В таком случае сработает [Internal Dynamic Page Cache](../dynamic-page-cache/index.md).
* Так как данный модуль отвечает результатом из БД без дополнительной обработки, все Drupal API будут пропущены. Это приводит к ситуации, что могут не работать [ленивые строители](../lazy-builder/index.md), такие, как Big Pipe. Но если ваша стратегия рендера учитывает это, то такие ленивые строители продолжат работать.
* Кеш не работает с сессионными запросами. Это означает, что API, инициализирующий сессии, такой как Temp Store API, приведёт к отключению данного кеша для пользователей, для которых был задействован данный API.
* IPC — не золотой молоток. Учитывая специфику работы данного кеша, он может стать бутылочным горлышком производительности, если счёт идёт на миллионы сущностей и постоянную инвалидации и генерацию кеша для них. Если вы начинаете испытывать проблемы с IPC, стоит сравнить работу только с IDPC, возможно он начнёт иметь такую же или даже лучше производительность, за счёт фрагментированного кеширования. А разгрузка БД от постоянных операций IPC сделает IDPC ещё быстрее.
* Модуль не исключает необходимость заботиться о кеш-метаданных в своём коде. Не стоит забывать про холодный кеш, на котором модуль вас не прикроет.

## Политики кеширования запросов и ответов

Данный модуль предоставляет политики запросов и ответов, которые позволяют отключить данный тип кеша при определённых условиях. Для этого используются сервисы с метками `page_cache_request_policy` и `page_cache_response_policy`.

**Политика кеширования запроса** отвечает за то, необходимо ли предпринимать попытки поиска кеша.

**Политика кеширования ответа** отвечает за то, необходимо ли создавать кеш для данного запроса и ответа.

Подобные сервисы создаются только для описания ситуаций, которые требуют отключения кеша, во всех остальных случаях кеш по умолчанию создаётся и производится попытка поиска и для этого ничего не нужно делать.

### Стандартные политики кеширования запросов

По умолчанию не предоставляется ни одной политики кеширования запросов. Тем не менее менеджер политик основывается на `\Drupal\Core\PageCache\DefaultRequestPolicy`.

Данная политика отключает поиск кеша для запроса в следующих ситуациях:

* Запрос выполняется в режиме CLI.
* Запрос произведён с не кешируемым методом: любой метод отличный от `GET` и `POST`.
* Запрос имеет сессию.

### Стандартные политики кеширования ответов

По умолчанию не предоставляется ни одной политики кеширования ответов.

### Создание политики кеширования запроса

Для того чтобы создать политику кеширования запроса, вам необходимо создать класс реализующий `\Drupal\Core\PageCache\RequestPolicyInterface` и объявить его как [сервис](../services/index.md) с меткой `page_cache_request_policy`.

Данный класс должен реализовать метод `check()` который принимает один аргумент:

* `Request $request`: Объект текущего запроса, для которого проводится проверка.

В качестве результата метод должен вернуть одно из следующих значений:

* `RequestPolicyInterface::ALLOW`: Поиск кеша для текущего запроса разрешён.
* `RequestPolicyInterface::DENY`: Поиск кеша для текущего запроса запрещён.
* `NULL`: Текущая политика не имеет никаких предпочтений и её результат стоит пропустить.

Пример класса `src/PageCache/RequestPolicy/UtmQueryRequestPolicy.php`:

```php
<?php

namespace Drupal\example\PageCache\RequestPolicy;

use Drupal\Core\PageCache\RequestPolicyInterface;
use Symfony\Component\HttpFoundation\Request;

/**
 * Provides cache policy to disable page cache for UTM requests.
 */
final class UtmQueryRequestPolicy implements RequestPolicyInterface {

  /**
   * {@inheritdoc}
   */
  public function check(Request $request): ?string {
    $utm_params = [
      'utm_source',
      'utm_medium',
      'utm_campaign',
      'utm_term',
      'utm_content',
    ];
    if (\array_intersect($utm_params, $request->query->keys())) {
      return RequestPolicyInterface::DENY;
    }
    return RequestPolicyInterface::ALLOW;
  }

}
```

Объявляем данный класс как сервис с меткой в `example.services.yml`:

```yaml
services:
  example.utm_query_page_cache_request_policy:
    class: Drupal\example\PageCache\RequestPolicy\UtmQueryRequestPolicy
    tags:
      - { name: page_cache_request_policy }
```

Пример выше будет блокировать поиск кеша для запросов где присутствует хоть одна перечисленная UTM метка в query запросе.

### Создание политики кеширования ответа

Для того чтобы создать политику кеширования ответа, вам необходимо создать класс реализующий `\Drupal\Core\PageCache\ResponsePolicyInterface` и объявить его как [сервис](../services/index.md) с меткой `page_cache_response_policy`.

Данный класс должен реализовать метод `check()` который принимает следующие аргументы:

* `Response $response`: Объект текущего ответа, для которого проводится проверка.
* `Request $request`: Объект текущего запроса, для которого проводится проверка.

В качестве результата метод должен вернуть одно из следующих значений:

* `RequestPolicyInterface::DENY`: Сохранение текущего ответа в кеш запрещен.
* `NULL`: Текущая политика не имеет никаких предпочтений и её результат стоит пропустить.

Пример класса `src/PageCache/ResponsePolicy/UtmQueryResponsePolicy.php`:

```php
<?php

namespace Drupal\example\PageCache\ResponsePolicy;

use Drupal\Core\PageCache\ResponsePolicyInterface;
use Symfony\Component\HttpFoundation\Request;
use Symfony\Component\HttpFoundation\Response;

/**
 * Provides cache policy to disable page cache for UTM requests.
 */
final class UtmQueryResponsePolicy implements ResponsePolicyInterface {

  /**
   * {@inheritdoc}
   */
  public function check(Response $response, Request $request): ?string {
    $utm_params = [
      'utm_source',
      'utm_medium',
      'utm_campaign',
      'utm_term',
      'utm_content',
    ];
    if (\array_intersect($utm_params, $request->query->keys())) {
      return ResponsePolicyInterface::DENY;
    }
    return NULL;
  }

}
```

Объявляем данный класс как сервис с меткой в `example.services.yml`:

```yaml
services:
  example.utm_query_dynamic_page_cache_response_policy:
    class: Drupal\example\PageCache\ResponsePolicy\UtmQueryResponsePolicy
    tags:
      - { name: page_cache_response_policy }
```

Пример выше будет блокировать сохранение кеша для запросов где присутствует хоть одна перечисленная UTM метка в query запросе.

## Дополнительная информация

### Заголовок X-Drupal-Cache

Когда данный модуль включен, он всегда добавляет к ответам заголовок `X-Drupal-Cache`. Данный заголовок может принимать следующие значения:

* `HIT`: Ответ сформирован из кеша.
* `MISS`: Ответ сформирован без участия кеша, но он успешно сохранён для будущих запросов — холодный кеш.

### Не кешируемые ответы

Модуль, помимо политик кеширования ответов, не будет сохранять результат в кеш в следующих ситуациях:

* Ответ не является реализацией экземпляром `\Drupal\Core\Cache\CacheableResponseInterface`.
* Ответ является реализацией `\Symfony\Component\HttpFoundation\BinaryFileResponse` или `\Symfony\Component\HttpFoundation\StreamedResponse`.

## Смотрите также

* [Internal Dynamic Page Cache](../dynamic-page-cache/index.md) — фрагментированное кеширование с поддержкой динамического содержимого.
