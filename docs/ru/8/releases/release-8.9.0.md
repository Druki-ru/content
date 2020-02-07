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

## Twig фильтр `without()` теперь может принимать массив

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

## Теперь к листингам добавляется новый кэш тег [entity_type]_list:[bundle]

- [#3107058](https://www.drupal.org/node/3107058)

Теперь для листингов возвращается дополнительный кэш тег `[entity_type]_list:[bundle]`, если у сущности имеется поддержка бандлов.

Ранее, сущность возвращала кэш тег для списков `[entity_type]_list` (`node_list`), который автоматически устанавливался на выводы со списком конкретной сущности. Например это делает Views, если вы выводите сущности.

Это приводило к ситуациям, когда обновление одной сущности инвалидировало множество других, не имеющих отношения к текущей. Например, у вас есть типы материалов «Новость» и «Отзыв», а для них созданы страницы с листингом `/news` и `/reviews`, соответственно. На оба листинга ранее устанавливался кэш тег `node_list`, и когда вы правили новость, он инвалидировался и очищал соответствующие кэш данные, но это также задевало листинг отзывов, что совершенно ненужно. Теперь, изменяя новость, будут инвалидированы только новости, а другие типы содержимого останутся в кэше.

Данное изменение позволит снизить частоту обращения и инвалидации кэша без необходимости.

## Добавлена поддержка поиска библиотек в папке сайта и установочных профилях

- [#3099614](https://www.drupal.org/node/3099614)

Некоторые модули используют модуль Libraries, для того чтобы хранить сторонние библиотеки в отличных от `/libraries` директориях. Данное поведение и возможность перенесена в ядро.

Все [библиотеки](../libraries/libraries.md), путь которых начинается с `/libraries` теперь ищутся в нескольких директориях в соответствующем порядке:

1. `/PATH/TO/SITE/libraries` (например `/sites/default/libraries`)
1. `/libraries`
1. `/PATH/TO/INSTALL_PROFILE` (например `/core/profiles/demo_umami/libraries`)

Первый файл что будет обнаружен и будет использован для библиотеки.

Дополнительно в ядро добавлен новый [сервис](../services/services.md) `library.libraries_directory_file_finder`, метод `find()` которого полная альтернатива для `libraries_get_path()`.

## Разработчики теперь могут создавать собственные Session Bag

- [#3109877](https://www.drupal.org/node/3109877)

Теперь разработчики могут создавать свои собственные «сессионные мешки» для хранения информации конкретного модуля внутри сессии.

Сессионный мешок должен реализовывать `\Symfony\Component\HttpFoundation\Session\SessionBagInterface` и быть объявлен как [сервис с меткой](../services/services.md). Получить сессионный мешок можно через сессию из запроса.

**Пример объявления сервиса:**

```yaml
example.session_bag:
  class: Drupal\example\Session\ExampleSessionBag
  tags:
    - { name: session_bag }
```

**Пример получения сессионного мешка:**

```php
$bag = $request->getSession()->getBag(ExampleSessionBag::BAG_NAME);
$bag->setFlag();
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

## Прочие изменения

- [#3083486](https://www.drupal.org/node/3083486) Конфигурация `action.settings.recursion_limit` и её схема была удалена. Drupal ядро не использовало данную конфигурацию.

## См. также

- [Drupal 9](../../9/drupal-9.md)