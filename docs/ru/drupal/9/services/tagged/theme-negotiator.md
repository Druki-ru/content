---
id: theme-negotiator
title: Theme Negotiator
core: 9
search-keywords:
  - theme_negotiator
  - theme negotiator
  - программное переключение темы
  - программная смена темы
  - поменять тему
  - сменить тему
  - переключить тему
metatags:
  title: 'Drupal 9: Theme Negotiator'
  description: 'Сервисы с меткой theme_negotiator позволяют программно переключать темы оформления.'
---

**Theme Negotiator** — [сервисы](../services.md) с меткой `theme_negotiator` позволяющие программно переключать [тему оформления](../../themes/themes.md).

## Введение

Theme Negotiator используется Drupal для определения темы оформления, которая должна быть использована для отрисовки страницы в рамках текущего запроса.

В пределах одного запроса могут сработать сразу несколько Theme Negotiator, в таком случае будет выбрана тема возвращаемая сервисом с самым большим приоритетом.

## Стандартные Theme Negotiator

Theme Negotiator сервисы, поставляемые с Drupal:

- **Drupal Core**:
    - `theme.negotiator.default`: Сервис по умолчанию. Применяется ко всем страницам. В качестве темы оформления выбирает ту, что указана в конфигурации `system.theme:default`.
    - `theme.negotiator.ajax_base_page`: Сервис для AJAX запросов. Контролирует чтобы AJAX запросы отвечали с темой оформления, которая используется на странице-инициатора данного запроса.
- **Block**:
    - `theme.negotiator.block.admin_demo`: Используется для включения нужной темы оформления и демонстрации регионов данной темы.
- **System**:
    - `theme.negotiator.system.batch`: Устанавливает тему оформления для страниц выполнения [Batch операций](../../batches/batches.md). В качестве темы старается использовать значение [$settings['maintenance_theme']](../../settings-php.md).
    - `theme.negotiator.system.db_update`: Устанавливает тему оформления для страниц обновления Базы Данных. Как и `theme.negotiator.system.batch`, отдаёт приоритет `$settings['maintenance_theme']`.
- **User**:
    - `theme.negotiator.admin_theme`: Включает административную тему оформления для административных [маршрутов](../../routing/routing.md) и если пользователь имеет разрешение `view the administration theme`. Значение берётся из конфигурации `system.theme:admin`, если оно не задано, используется текущая тема оформления.

## Создание Theme Negotiator

Класс Theme Negotiator должен реализовывать `Drupal\Core\Theme\ThemeNegotiatorInterface` и быть объявлен как сервис с меткой.

Класс должен реализовывать два обязательных метода:

- `applies()`: Должен вернуть логическое значение `TRUE`, если данный Theme Negotiator подходит для текущего запроса, `FALSE`, если нет.
- `determineActiveTheme()`: Должен вернуть машинное имя темы оформления, которая будет использоваться для отрисовки страницы.

Пример Theme Negotiator, который включает тему оформления `special` на основе куки `use_special_theme`:

```php
<?php

namespace Drupal\example\Theme;

use Drupal\Core\Routing\RouteMatchInterface;
use Drupal\Core\Theme\ThemeNegotiatorInterface;
use Drupal\Core\Path\PathMatcherInterface;
use Symfony\Component\HttpFoundation\RequestStack;

/**
 * Provides theme negotiator for 'special' theme.
 */
final class SpecialTheme implements ThemeNegotiatorInterface {

  /**
   * The request stack.
   * 
   * @var \Symfony\Component\HttpFoundation\RequestStack
   */
  protected $requestStack;

  /**
   * Constructs the RequestStack object.
   * 
   * @param \Symfony\Component\HttpFoundation\RequestStack $request_stack
   *   The request stack.
   */
  public function __construct(RequestStack $request_stack) {
    $this->requestStack = $request_stack;
  }

  /**
   * {@inheritdoc}
   */
  public function applies(RouteMatchInterface $routeMatch) {
    return $this->requestStack->getCurrentRequest()->cookies->has('use_special_theme');
  }

  /**
   * {@inheritdoc}
   */
  public function determineActiveTheme(RouteMatchInterface $routeMatch) {
    return 'special';
  }

}
```

Затем необходимо объявить данный класс как сервис:

```yaml
services:
  theme.negotiator.special_theme:
    class: Drupal\example\Theme\SpecialTheme
    arguments: ['@request_stack']
    tags:
      - { name: theme_negotiator, priority: -40 }
```

Для нашего сервиса указан приоритет -40, так как у `theme.negotiator.default` приоритет -100. Таким образом, наш Theme Negotiator будет иметь больший вес чем определитель по умолчанию.

## Ссылки

- [Drupal 8: Theme Negotiator — программное переключение тем](https://niklan.net/blog/126), Niklan, 2016.
