---
title: Inbound и Outbound обработчики
slug: wiki/10/inbound-outbound-path-processor
core: 10
search-keywords:
  - чпу
  - алиас
  - алиасы
metatags:
  title: 'Drupal 10: Inbound и Outbound обработчики'
  description: 'Сервисы с меткой path_processor_inbound и path_processor_outbound позволяют обрабатывать входящие и исходящие пути сайта.'
authors:
  - Niklan
  - a-kovrigin
---

**Inbound Processor** и **Outbound Processor** — [сервисы](../../index.md) с меткой `path_processor_inbound` и `path_processor_outbound` позволяющие программно обрабатывать входящие и исходящие пути сайта.

## Введение

Inbound и Outbound обработчики используются Drupal для определения адреса, по которому обращается пользователь, а также, конвертирует исходящие системные адреса в необходимый вид.

Drupal и его API для маршрутизации всегда работают с внутренними системными путями. Но системные пути не всегда красивы и понятны, поэтому применяются ЧПУ или задаются синонимы путей. Inbound и Outbound обработчики отвечают за этот процесс.

Задача данных обработчиков - конвертировать исходящие системные пути в более понятные для пользователя и входящие в системные, которые понятны Drupal и коду.

![Схема работы Inbound и Outbound обработчиков](https://i.imgur.com/xpkOGxb.png)

Таким образом Drupal и код работают с системными адресами, независимо от того что видит в конечном итоге пользователь. Благодаря данным «посредникам», синонимы путей не конфликтуют с системными и предоставляется больше свободы для настройки путей.

## Outbound Processor — исходящие пути

**Outbound Processor** (`path_processor_outbound`) — обработчик исходящих путей сайта. Задача данного обработчика, конвертировать системный путь в более человекопонятный. Например путь `/node/123` превратить в `/about`. Таким образом, все ссылки ведущие на `/node/123` будут автоматически конвертироваться в `/about`.

### Создание Outbound Processor

Для реализации Outbound Processor необходимо чтобы класс сервиса реализовывал `Drupal\Core\PathProcessor\OutboundPathProcessorInterface`. Данный интерфейс требует реализовать метод `::processOutbound` со следующими параметрами:

- `$path`: Текущий путь который проходит обработку. Из примера это `/node/123`.
- `$options`: Массив опций для URL.
    - `'query'`: Массив параметров для пути (query).
    - `'fragment'`: Фрагмент пути — якорь (`#foo-bar`).
    - `'absolute'`: Логический индикатор должен ли сгенерированный путь быть абсолютным. По умолчанию `FALSE` если не заданого иного.
    - `'language'`: Объект с языком для которого генерируется путь. Если данное значение отсутствует, значит путь генерируется для текущего активного языка.
    - `'https'`: Логический индикатор, должен ли путь быть принудительно с HTTPS протоколом. По умолчанию используется текущий протокол.
    - `'base_url'`: Базовый URL сайта — домен (example.com). Позволяет поменять домен. Например если для разных языков используются разные домены.
    - `'prefix'`: Префикс базового URL сайта — поддомен (**foo**.example.com). Позволяет скорректировать префикс пути. Например если для разных языков используются разные поддомены.
    - `'route'`: Объект [маршрута](../../../routing/index.md) для текущего `$path`.
- `$request`: Объект текущего запроса.
- `$bubbleable_metadata`: Объект для сбора кеш-метаданных.

<Aside type="tip">

Благодаря возможности модификации `$options['query']` у вас появляется возможность конвертировать пути типа `/catalog?price-from=1000&price-to=2000` в `/catalog/price-1000-2000`.

</Aside>

Метод должен вернуть путь который будет использоваться в URL.

Пример конвертации `/node/123` в `/about`:

```yaml
services:
  example.example_outbound_processor:
    class: Drupal\example\PathProcessor\ExampleOutboundProcessor
    tags:
      - { name: path_processor_outbound }
```

```php
<?php

namespace Drupal\example\PathProcessor;

use Drupal\Core\PathProcessor\OutboundPathProcessorInterface;
use Drupal\Core\Render\BubbleableMetadata;
use Symfony\Component\HttpFoundation\Request;

/**
 * Provides custom outbound processor.
 */
final class ExampleOutboundProcessor implements OutboundPathProcessorInterface {

  /**
   * {@inheritdoc}
   */
  public function processOutbound($path, &$options = [], Request $request = NULL, BubbleableMetadata $bubbleable_metadata = NULL) {
    if ($path == '/node/123') {
      $path = '/about';
    }
    return $path;
  }

}
```

## Inbound Processor — входящие пути

**Inbound Processor** (`path_processor_inbound`) — обработчик входящих путей сайта. Задача данного обработчика, конвертировать входящий человекопонятный путь в системный. Например путь `/about` превратить в `/node/123`. Таким образом, все входящие запросы на `/about` «внутри» будут трактованы как `/node/123`, система маршрутизации найдёт системный путь `/node/{nid}` и корректно вызовет все обработчики для страницы. Это позволяет бэкенд логике не опираться на нестабильные пути, а всегда работать с системными.

### Создание Inbound Processor

Для реализации Inbound Processor необходимо чтобы класс сервиса реализовывал `Drupal\Core\PathProcessor\InboundPathProcessorInterface`. Данный интерфейс требует реализовать метод `::processInbound` со следующими параметрами:

- `$path`: Текущий путь который проходит обработку. Из примера это `/about`.
- `$request`: Объект текущего запроса.

Метод должен вернуть системный путь чтобы Drupal понял как его обрабатывать.

Пример конвертации `/about` в `/node/123`

```yaml
services:
  example.example_inbound_processor:
    class: Drupal\example\PathProcessor\ExampleInboundProcessor
    tags:
      - { name: path_processor_inbound }
```

```php
<?php

namespace Drupal\example\PathProcessor;

use Drupal\Core\PathProcessor\InboundPathProcessorInterface;
use Drupal\Core\Render\BubbleableMetadata;
use Symfony\Component\HttpFoundation\Request;

/**
 * Provides custom inbound processor.
 */
final class ExampleInboundProcessor implements InboundPathProcessorInterface {

  /**
   * {@inheritdoc}
   */
  public function processInbound($path, Request $request) {
    if ($path == '/about') {
      $path = '/node/123';
    }
    return $path;
  }

}
```

## Модуль Path Alias

Drupal предоставляет стандартный модуль Path Alias (`path_alias`), который реализует систему алиасов и ЧПУ (сервис `path_alias.path_processor`).

Данный модуль создаёт специальную [сущность](../../../entities/index.md) в которой хранит соответствия системного пути и алиаса для него. Затем, при входящих и исходящих запросах он, на основе данных соответствий конвертирует пути в обе стороны.

Так, задав материалу `/node/123`, через административный интерфейс, синоним `/about`, он будет сохранён и проходить через эти обработки.

## Ссылки

- [Drupal 8: Inbound и Outbound Processor](https://niklan.net/blog/183), Niklan, 2018.
- [Программная реализация ЧПУ](http://xandeadx.ru/blog/drupal/965), xandeadx, 2020.
