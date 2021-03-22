---
title: Стратегия заполнителя
slug: 9/placeholder-strategy
metatags:
  title: 'Drupal 9: Стратегия заполнителя'
  description: 'Placeholder Strategy определяет как будут обрабатываться заполнители созданные ленивым строителем.'
---

**Стратегия заполнителя** (англ. «Placeholder Strategy») — логика, определяющая как должны обрабатываться заполнители созданные [ленивым строителем](../lazy-builder/index.md).

## Введение

При помощи ленивых строителей вы можете делать специальную разметку для тех участков кода, которые являются медленными или могут оказаться медленными. Ленивые строители пропускают выполнение основного кода и заменяют их на заполнители.

Стратегия заполнителя должна заменить все подходящие для неё заполнители на значения. Эти стратегии могут быть как простыми, где значение сразу заменятся на значение, так и более комплексными разделёнными на несколько этапов и процессов.

Ядро предоставляет модуль BigPipe, который предоставляет одноимённую стратегию.

## Стратегия заполнителя

Стратегия заполнителя — это класс, реализующий `\Drupal\Core\Render\Placeholder\PlaceholderStrategyInterface` и объявленный как [сервис](../services/index.md) с меткой `placeholder_strategy`.

Пример класса:

```php
<?php

namespace Drupal\example\Render\Placeholder;

use Drupal\Core\Render\Placeholder\PlaceholderStrategyInterface;

/**
 * Provides placeholder strategy.
 */
final class ExamplePlaceholderStrategy implements PlaceholderStrategyInterface {

  /**
   * {@inheritdoc}
   */
  public function processPlaceholders(array $placeholders): array {
    return $placeholders;
  }

}
```

Пример объявления сервиса:

```yaml
services:
  placeholder_strategy.example:
    class: Drupal\example\Render\Placeholder\ExamplePlaceholderStrategy
    tags:
      - { name: placeholder_strategy }
```

## PlaceholderStrategyInterface

Интерфейс `PlaceholderStrategyInterface` требует реализации единственного метода `::processPlaceholders()`.

В качестве аргумента, метод получает массив `$placeholders`, который содержит в себе все заполнители, созданные Drupal в процессе рендера результата.

Ключами у данного массива являются заполнители, которые Drupal вставил в текущий вариант ответа. Значениями являются рендер массивы с тем, на что заменить заполнители на следующем цикле рендера.

Пример такого массива:

```php
$placeholders = [
  '<drupal-render-placeholder callback="foo_b_lazy" arguments="" token="TOKEN"></drupal-render-placeholder>' => [
    '#lazy_builder' => [
      'foo_b_lazy', [],
    ],
  ],
  '<drupal-render-placeholder callback="FooBar::baz" arguments="0" token="_HAdUpwWmet0TOTe2PSiJuMntExoshbm1kh2wQzzzAA"></drupal-render-placeholder>' => [
    '#lazy_builder' => [
      'FooBar::baz', [0],
    ],
  ],
];
```

Результатом должен быть этот же самый массив, в котором разрешено заменять рендер массивы заполнителей.

Например, если наша стратегия хочет чтобы заполнитель `<drupal-render-placeholder callback="FooBar::baz" arguments="0" token="_HAdUpwWmet0TOTe2PSiJuMntExoshbm1kh2wQzzzAA"></drupal-render-placeholder>` был заменён на строку «Hello World!», она должна вернуть следующий результат:

```php
$placeholders = [
  '<drupal-render-placeholder callback="foo_b_lazy" arguments="" token="TOKEN"></drupal-render-placeholder>' => [
    '#lazy_builder' => [
      'foo_b_lazy', [],
    ],
  ],
  '<drupal-render-placeholder callback="FooBar::baz" arguments="0" token="_HAdUpwWmet0TOTe2PSiJuMntExoshbm1kh2wQzzzAA"></drupal-render-placeholder>' => [
    '#markup' => 'Hello World!',
  ],
];
```

Вы можете использовать все возможности рендер массивов, например, дополнительно подключать необходимые для стратегии [библиотеки](../libraries/index.md).

Если вы не желаете ничего делать с тем или иным заполнителем — не меняйте его рендер массив. Так он передастся следующим стратегиям, как минимум стандартной `SingleFlushStrategy`.

## Стандартные стратегии

Drupal предоставляет две стратегии по умолчанию:

- `placeholder_strategy.single_flush`: Стратегия, которая вызывается всегда, если отсутствуют другие или они не заменили рендер массивы, так как имеет вес -1000. Данная стратегия возвращает заполнители без изменений, из-за чего, при следующем цикле рендера будут вызваны указанные функции обратного вызова и заменены на значения.
- `placeholder_strategy.big_pipe`: Стратегия предоставляемая стандартным модулем BigPipe. Данная стратегия возвращает ответ на запрос с заполнителями, но не закрывает запрос и вызывает функции обратного вызова. По мере готовности результата, данные значения добавляются в ответ.

## Ссылки

- [Drupal 8, 9: Placeholder Strategy](https://niklan.net/blog/213), Niklan 2020
- [Drupal 8: Рендер массивы и их рендеринг](https://niklan.net/blog/210), Niklan, 2020