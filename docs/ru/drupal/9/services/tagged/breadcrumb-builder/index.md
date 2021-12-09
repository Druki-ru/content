---
title: Breadcrumb Builder
slug: wiki/9/breadcrumb-builder
core: 9
search-keywords:
  - хлебные крошки
  - установка хлебных крошек
  - изменение хлебных крошек
  - создание хлебных крошек
  - breadcrumb_builder
  - breadcrumbs
metatags:
  title: 'Drupal 9: Breadcrumb Builder (Строитель хлебных крошек)'
  description: 'Сервисы с меткой breadcrumb_builder позволяют программно устанавливать хлебные крошки.'
---

**Breadcrumb Builder** (строитель хлебных крошек) — [сервисы](../../index.md) с меткой `breadcrumb_builder` позволяющие программно задавать хлебные крошки на сайте.

## Введение

**Breadcrumb Builder** сервисы используются Drupal для формирования хлебных крошек на странице.

Задача Breadcrumb Builder — сформировать массив из ссылок для хлебных крошек в том порядке, в котором они должны быть отображены.

При помощи данных сервисов вы можете контролировать то, какие ссылки, в каком порядке и как будут отображены.

## Стандартные Breadcrumb Builder

Drupal ядро предоставляет следующие строители хлебных крошек:

- `system.breadcrumb.default`: Строитель хлебных крошек от модуля `system`. Используется по умолчанию где не задействованы прочие Breadcrumb Builder. Устанавливает хлебные крошки на основе пути страницы и её иерархии.
- `book.breadcrumb`: Строитель крошек от модуля `book`. Устанавливает хлебные крошки для страниц с подшивками на основе иерархии подшивки текущего материала.
- `comment.breadcrumb`: Строитель крошек от модуля `comment`. Устанавливает хлебные крошки для страниц «ответ на комментарий».
- `forum.breadcrumb.node`: Строитель крошек от модуля `forum`. Устанавливает хлебные крошки для страницы топика форума.
- `forum.breadcrumb.listing`: Строитель крошек от модуля `forum`. Устанавливает хлебные крошки на страницах листингов топиков форума.
- `help.breadcrumb`: Строитель крошек от модуля `help_topics`. Устанавливает хлебные крошки на страницах с документацией.
- `taxonomy_term.breadcrumb`: Строитель крошек от модуля `taxonomy`. Устанавливает хлебные крошки на страницах терминах учитывая иерархию термина в словаре.

### system.breadcrumb.default

Стандартный строитель хлебных крошек, который будет применяться всегда, если не задан иной с более высоким приоритетом для конкретного запроса. Данный строитель хлебных крошек собирает их на основе пути, по которому обратились к сайту.

Например, если было обращение к новости с адресом `/foo/bar/news-title`, то он разобьет данный путь на 2 части:

- `/foo/bar`
- `/foo`

Далее, он будет проверять каждый из этих путей, и если они валидны и существуют, будет получать для них заголовки. Например, если `/foo` является страницей с заголовком «Foo», а `/foo/bar` странице с заголовком «Bar», то Drupal посмотрит следующий массив хлебных крошек:

- Главная (/)
- Foo (/foo)
- Bar (/foo/bar)

Таким образом, Drupal может автоматически собирать хлебные крошки на основе иерархии в пути.

> [!NOTE]
> Обратите внимание, что текущий путь не добавляется в хлебные крошки по умолчанию.

## Создание Breadcrumb Builder

Строитель хлебных крошек состоит из класса реализующего `Drupal\Core\Breadcrumb\BreadcrumbBuilderInterface`, который объявлен как сервис с меткой `breadcrumb_builder`.

От строителя требуется объявить два метода:

- `::applies()`: Должен вернуть логическое значение, применим ли в текущий момент данный строитель или нет.
- `::build()`: Возвращает объект `Drupal\Core\Breadcrumb\Breadcrumb` содержащий в себе массив из ссылок хлебных крошек.

Если несколько строителей хлебных крошек подходят для одного запроса, будет выбран тот, у которого выше приоритет сервиса.

Пример построителя хлебных крошек, который для содержимого типа `news` будет задавать хлебные состоящие из ссылки на главную страницу и ссылку на текущую страницу.

```php
<?php

namespace Drupal\example;

use Drupal\Core\Breadcrumb\Breadcrumb;
use Drupal\Core\Breadcrumb\BreadcrumbBuilderInterface;
use Drupal\Core\Link;
use Drupal\Core\Routing\RouteMatchInterface;
use Drupal\node\NodeInterface;
use Drupal\Core\StringTranslation\TranslatableMarkup;

/**
 * Provides breadcrumb builder for news content.
 */
class NewsBreadcrumbBuilder implements BreadcrumbBuilderInterface {

  /**
   * {@inheritdoc}
   */
  public function applies(RouteMatchInterface $route_match) {
    $node = $route_match->getParameter('node');
    // Only for "news" content types.
    return $node instanceof NodeInterface && $node->bundle() == 'news';
  }

  /**
   * {@inheritdoc}
   */
  public function build(RouteMatchInterface $route_match) {
    $breadcrumb = new Breadcrumb();
    // News entity.
    $node = $route_match->getParameter('node');
    $links = [];
    // Frontpage link.
    $links[] = Link::createFromRoute(new TranslatableMarkup('Home'), '<front>');
    // News page link.
    $links[] = $node->toLink();
    // Set cache contexts so each page has different set of breadcrumbs.
    $breadcrumb->addCacheContexts(['url.path']);
    return $breadcrumb->setLinks($links);
  }

}
```

## Сторонние решения

- [Easy Breadcrumb](https://www.drupal.org/project/easy_breadcrumb) — расширяет стандартный `system.breadcrumb.default` и собирает хлебные крошки на основе текущего пути. В отличие от стандартного, добавляет страницу настроек, где можно настроить поведение сборки крошек, например, добавлять крошку на текущую страницу, скрывать крошку на главную или поменять её текст и т.д.

## Ссылки

- [Drupal 8: Программное создание хлебных крошек](https://niklan.net/blog/129), Niklan, 2016.
