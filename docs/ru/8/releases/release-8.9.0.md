---
id: release-8.9.0
title: 'Drupal 8.9.0'
path: /8/releases/8.9.0
core: 8
metatags:
  title: 'Drupal 8.9.0: Список изменений'
  description: 'Список изменений Drupal 8.9.0.'
---

**Дата релиза**: 3 июня 2020 г.

Данный релиз является переходным с [Drupal 8](../drupal-8.md) на [Drupal 9](../../9/drupal-9.md).

**Drupal 8.9.0** API совместим с [**Drupal 9.0.0**](../../9/releases/release-9.0.0.md). Главное отличие между версиями — Drupal 9.0.0 не содержит код, который помечен `@deprecated` в Drupal 8.9.0.

Изменения в данной версии минимальны, по причине того, что он разрабатывается одновременно с Drupal 9 и все силы направлены на чистку устаревшего кода Drupal 8.

## hook_install, hook_uninstall, hook_modules_installed и hook_modules_uninstalled теперь получают параметр $is_syncing

- [#3098920](https://www.drupal.org/node/3098920)

Теперь в хуки `hook_install()`, `hook_uninstall()`, `hook_modules_installed()` и `hook_modules_uninstalled()` передаётся новый параметр `$is_syncing`.

Параметр `$is_syncing` будет `TRUE` когда модуль устанавливается или удаляется как часть процесса обновления конфигураций. Это позволит корректировать поведение модуля с учётом данного значения. Например, если происходит синхронизация конфигураций, вы не должны производить никаких изменений в конфигурационных объектах и сущностях.

**Раньше**

```php
/**
 * Implements hook_install().
 */
function demo_umami_content_install() {
  if (!\Drupal::service('config.installer')->isSyncing()) {
    \Drupal::classResolver(InstallHelper::class)->importContent();
  }
}
```

**Теперь**

```php
/**
 * Implements hook_install().
 */
function demo_umami_content_install($is_syncing) {
  if (!$is_syncing) {
    \Drupal::classResolver(InstallHelper::class)->importContent();
  }
}
```

## Фильтр для Twig `without()` теперь может принимать массив

- [#3102853](https://www.drupal.org/node/3102853)

Иногда требуется перенести большое количество полей из `{{ content }}` массива и отрендерить их в ручном режиме.

Вы не можете не рендерить `{{ content }}` и просто вызывать поля напрямую, так как это может нарушить целостность кэша.

До текущего изменения `without()` принимал лишь аргументы в качестве строк чтобы вы могли контролировать поведение, теперь он может принимать массив из строк.

**Ранее**

```twig
<article>
  <div class="fancy">
    {{ content.field_fancy_1 }}
    {{ content.field_fancy_2 }}
    {{ content.field_fancy_3 }}
    {{ content.field_fancy_4 }}
  </div>
  <div class="important">
    {{ content.field_important_1 }}
    {{ content.field_important_2 }}
    {{ content.field_important_3 }}
    {{ content.field_important_4 }}
  </div>
  <div class="content">
    {{ content|without('field_fancy_1', 'field_fancy_2', 'field_fancy_3', 'field_fancy_4', 'field_important_1', 'field_important_2', 'field_important_3', 'field_important_4', 'field_special') }}
  </div>
  <div class="special">
    {{ content.field_special }}
  </div>
</article>
```

**Теперь**

```twig
{%
  set fancy_fields = [
    'field_fancy_1',
    'field_fancy_2',
    'field_fancy_3',
    'field_fancy_4',
  ]
%}
{%
  set important_fields = [
    'field_important_1',
    'field_important_2',
    'field_important_3',
    'field_important_4',
  ]
%}
<article>
  <div class="important">
    {% for field in important_fields %}
      {{ content[field] }}
    {% endfor %}
  </div>
  <div class="fancy">
    {% for field in fancy_fields %}
      {{ content[field] }}
    {% endfor %}
  </div>
  <div class="content">
    {{ content|without(fancy_fields, important_fields, 'field_special') }}
  </div>
  <div class="special">
      {{ content.field_special }}
  </div>
</article>
```

## Оформление и темизация

- [#3040758](https://www.drupal.org/node/3040758) В теме оформления Classy на поля с меткой «в строку» теперь добавляется класс `clearfix`. Если вызовет проблемы в вашей теме, вы можете переопределить данное поведение перезаписав шаблон `field.html.twig` в своей теме.

## Views

- [#3092185](https://www.drupal.org/node/3092185) Добавлен `rel="nofollow"` для заголовков сортировки таблиц.

## Composer

- [#3090416](https://www.drupal.org/node/3090416) Представлен Composer плагин Core Project Message.

## Color

- [#3082742](https://www.drupal.org/node/3082742) Тип данных возвращаемый функцией `color_valid_hexadecimal_string()` был изменён. До изменения возвращаемый тип был числом (0 или 1), теперь возвращается булево значение (`TRUE`, `FALSE`).

## Тестирование

- [#3105980](https://www.drupal.org/node/3105980) Метод `\Drupal\migrate_drupal\Tests\StubTestTrait::createStub` был переименован в `createEntityStub`.

## См. также

- [Drupal 9](../../9/drupal-9.md)