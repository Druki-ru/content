---
id: inbound-outbound-path-processor
title: Inbound и Outbound обработчики
core: 9
search-keywords:
  - чпу
  - алиас
  - алиасы
metatags:
  title: 'Drupal 9: Inbound и Outbound обработчики'
  description: 'Сервисы с меткой path_processor_inbound и path_processor_outbound позволяют обрабатывать входящие и исходящие пути сайта.'
---

[Сервисы](../services.md) с меткой `path_processor_inbound` и `path_processor_outbound` позволяют программно обрабатывать входящие и исходящие пути сайта.

## Outbound Processor

**Outbound Processor** (`path_processor_outbound`) — обработчик исходящих путей сайта. Задача данного обработчика, конвертировать системный путь в более человекопонятный. Например путь `/node/123` превратить в `/about`. Таким образом, все ссылки ведущие на `/node/123` будут автоматически конвертироваться в `/about`.

Для реализации Outbound Processor необходимо чтобы класс сервиса реализовывал `Drupal\Core\PathProcessor\OutboundPathProcessorInterface`. Данный интерфейс требует реализовать метод `::processOutbound` со следующими параметрами:

- `$path`: Текущий путь который проходит обработку. Из примера это `/node/123`.
- `$options`: Массив опций для URL.
    - `'query'`: Массив параметров для пути (query).
    - `'fragment'`: Фрагмент пути — якорь (`#foo-bar`).
    - `'absolute'`: Булевый индикатор должен ли сгенерированный путь быть абсолютным. По умолчанию `FALSE` если не заданого иного.
    - `'language'`: Объект с языком для которого генерируется путь. Если данное значение отсутствует, значит путь генерируется для текущего активного языка.
    - `'https'`: Булевый индикатор, должен ли путь быть принудительно с HTTPS протоколом. По умолчанию используется текущий протокол.
    - `'base_url'`: Базовый URL сайта — домен (example.com). Позволяет поменять домен. Например если для разных языков используются разные домены.
    - `'prefix'`: Префикс базового URL сайта — поддомен (**foo**.example.com). Позволяет скорректировать префикс пути. Например если для разных языков используются разные поддомены.
    - `'route'`: Объект [маршрута](../../routing/routing.md) для текущего `$path`.
- `$request`: Объект текущего запроса.
- `$bubbleable_metadata`: Объект для сбора кэш-метаданных.

> [!TIP]
> Благодаря возможности модификации `$options['query']` у вас появляется возможность конвертировать пути типа `/catalog?price-from=1000&price-to=2000` в `/catalog/price-1000-2000`.

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

## Inbound Processor

**Inbound Processor** (`path_processor_inbound`) — обработчик входящих путей сайта. Задача данного обработчика, конвертировать входящий человекопонятный путь в системный. Например путь `/about` превратить в `/about`. Таким образом, все входящие запросы на `/about` «внутри» будут трактованы как `/node/123`, система маршрутизации найдёт системный путь `/node/{nid}` и корректно вызовет все обработчики для страницы. Это позволяет бэкенд логике не опираться на нестабильные пути, а всегда работать с системными.

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

Drupal предоставляет стандартный модуль Path Alias (`path_alias`), который реализует систему алиасов и ЧПУ используя средсвом сервиса `path_alias.path_processor`.

Данный модуль создаёт специальную [сущность](../../entities.md) в которой хранит соответствия системного пути и алиаса для него. Затем, при входящих и исходящих запросах он, на основе данных соответствий конвертирует пути в обе стороны.

Так, задав материалу `/node/123`, через административный интерфейс, синоним `/aboout`, он будет сохранён и проходить через эти обработки.

## Ссылки

- [Drupal 8: Inbound и Outbound Processor](https://niklan.net/blog/183), Niklan, 2018.
- [Программная реализация ЧПУ](http://xandeadx.ru/blog/drupal/965), xandeadx, 2020.
