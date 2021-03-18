---
title: Internal Dynamic Page Cache
slug: 9/dynamic-page-cache
metatags:
  title: 'Drupal 9: Internal Dynamic Page Cache'
  description: Internal Dynamic Page Cache — кеширует страницы, в том числе с динамическим содержимым.
---

**Internal Dynamic Page Cache** (машинное имя `dynamic_page_cache`) — стандартный модуль кеширования страниц с динамическим содержимым.

> [!TIP]
> Данный модуль включён по умолчанию в [стандартном профиле](../distributions/standard/index.md).

> [!NOTE]
> Не путайте данный модуль с [Internal Page Cache](../page-cache/index.md)

## Введение

Internal Dynamic Page Cache — отвечает за кеширование страниц, которые имеют динамическое содержание.

При обработке ответа сайта на запрос, данный модуль формирует специальный рендер массив без данных, к которому добавляются все кеш метаданные полученные в процессе подготовки ответа, а также свои собственные. Также в данный рендер массив сохраняется текущий экземпляр объекта ответа на текущий запрос. Эти данные объединяются и сохраняются в cache bin `dynamic_page_cache`.

При обработке входящего запроса, он заново формирует свой рендер массив с кеш метаданными и пытается получить для него результат из кеша. Если такой есть — он загружает его, и отвечает тем экземпляром ответа, что был сохранён на момент генерации.

### Преимущества

* Работает с любым динамическим содержимым на сайте. Он всегда отвечает с актуальными данными.
* Он кеширует «сырой результат». Плейсхолдеры типа `<head-placeholder>`, `<css-placeholder>` и `<js-placeholder>` заменяются при каждом таком ответе индивидуально. Это позволяет динамически управлять тем, какие библиотеки подключаются для одного и того же кеша и какие будут заголовки.
* С ним работают любые ленивые строители, так как данный кеш будет хранить плейсхолдеры, вместо результатов. Это позволяет иметь совершенно разные ответы на каждый запрос и при этом отдавать его из кеша.
* С ним работает стандартный модуль BigPipe.
* Благодаря тому что он не хранит в кеше динамические части, размер кеша получается достаточно небольшой.

### Недостатки

* Из-за того что он хранит «сырой результат», часть обработчиков Drupal запускаются на такие ответы и если они медленные или не оптимизированы, это скажется на времени ответа с данным кешированием.

## Политики кеширования запросов и ответов

Данный модуль предоставляет политики запросов и ответов, которые позволяют отключить данный тип кеша при определённых условиях. Для этого используются сервисы с метками `dynamic_page_cache_request_policy` и `dynamic_page_cache_response_policy`.

**Политика кеширования запроса** отвечает за то, необходимо ли предпринимать попытки поиска кеша.

**Политика кеширования ответа** отвечает за то, необходимо ли создавать кеш для данного запроса и ответа.

Подобные сервисы создаются только для описания ситуаций, которые требуют отключения кеша, во всех остальных случаях кеш по умолчанию создаётся и производится попытка поиска и для этого ничего не нужно делать.

### Стандартные политики кеширования запросов

По умолчанию предоставляется одна политика кеширования запросов `\Drupal\dynamic_page_cache\PageCache\RequestPolicy\DefaultRequestPolicy`.

Данная политика отключает поиск кеша для запроса в следующих ситуациях:

* Запрос выполняется в режиме CLI.
* Запрос произведён с не кешируемым методом: любой метод отличный от `GET` и `POST`.

### Стандартные политики кеширования ответов

По умолчанию предоставляется одна политика кеширования ответов `\Drupal\dynamic_page_cache\PageCache\ResponsePolicy\DenyAdminRoutes`.

Даная политика отключает сохранение ответа в кеш если ответ формируется для [маршрута](../routing) с опцией `_admin_route`. Иными словами, данный кеш не работает на административных страницах.

### Создание политики кеширования запроса

Для того чтобы создать политику кеширования запроса, вам необходимо создать класс реализующий `\Drupal\Core\PageCache\RequestPolicyInterface` и объявить его как [сервис](../services/index.md) с меткой `dynamic_page_cache_request_policy`.

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
 * Provides cache policy to disable dynamic page cache for UTM requests.
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
  example.utm_query_dynamic_page_cache_request_policy:
    class: Drupal\example\PageCache\RequestPolicy\UtmQueryRequestPolicy
    tags:
      - { name: dynamic_page_cache_request_policy }
```

Пример выше будет блокировать поиск кеша для запросов где присутствует хоть одна перечисленная UTM метка в query запросе.

### Создание политики кеширования ответа

Для того чтобы создать политику кеширования ответа, вам необходимо создать класс реализующий `\Drupal\Core\PageCache\ResponsePolicyInterface` и объявить его как [сервис](../services/index.md) с меткой `dynamic_page_cache_response_policy`.

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
 * Provides cache policy to disable dynamic page cache for UTM requests.
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
      - { name: dynamic_page_cache_response_policy }
```

Пример выше будет блокировать сохранение кеша для запросов где присутствует хоть одна перечисленная UTM метка в query запросе.

## Дополнительная информация

### Заголовок X-Drupal-Dynamic-Cache

Когда данный модуль включен, он всегда добавляет к ответам заголовок `X-Drupal-Dynamic-Cache`. Данный заголовок может принимать следующие значения:

* `HIT`: Ответ сформирован из кеша.
* `UNCACHEABLE`: Если ответ на запрос не является кешируемым.
* `MISS`: Ответ сформирован без участия кеша, но он успешно сохранён для будущих запросов — холодный кеш.

### Не кешируемые ответы

Модуль не будет кешировать ответы, которые имеют кеш-теги с высокой частотой инвалидации, очень общие кеш-контексты или `max-age` ниже определённой отметки.

Подобные значения берутся из параметра `renderer.config:auto_placeholder_conditions` файла `default.services.yml` и имеют следующие настройки по умолчанию:

* `max-age` равный 0 — ответы у которых max-age равен этому значению или ниже, не будут кешироваться данным модулем.
* `contexts` равный `session` и `user` — ответы у которых есть один из этих контекстов не будут кешироваться данным модулем.
* `tags` с пустым массивом — по умолчанию теги никак не будут влиять на поведение модуля.

> [!TIP]
> Данный файл и параметры могут быть изменены и быть разными для разных окружений.

Если одна из проверок сработает, заголовок `X-Drupal-Dynamic-Cache` будет равен `UNCACHEABLE`.

## Смотрите также

* [Internal Page Cache](../page-cache/index.md) — статическое кеширование ответов для анонимных пользователей.
