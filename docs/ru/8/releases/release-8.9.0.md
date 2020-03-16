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
- [#3115005](https://www.drupal.org/node/3115005) Из тем оформления удалён фикс для fieldset в Firefox, так как ошибку в браузере исправили.

## Book

- [#3010378](https://www.drupal.org/node/3010378) `BookManager::buildItems` теперь не загружает сущности для генерации ссылки, а генерирует роут на основе ID. Это значительно повышает производительность на материалах с большим количество «подшивок».

## Database API

- [#3113403](https://www.drupal.org/node/3113403) (временно откачено) Все драйверы баз данных теперь должны переопределять `Drupal\Core\Database\Query\Condition`.

## Extension API

- [#3066801](https://www.drupal.org/node/3066801) Добавлен новый хук `hook_removed_post_updates()` который позволяет указать какие `hook_post_update_N()` реализации были удалены и в какой версии.

## Views

- [#3092185](https://www.drupal.org/node/3092185) Добавлен `rel="nofollow"` для заголовков сортировки таблиц.

## Composer

- [#3090416](https://www.drupal.org/node/3090416) Представлен Composer плагин Core Project Message.

## Color

- [#3082742](https://www.drupal.org/node/3082742) Тип данных возвращаемый функцией `color_valid_hexadecimal_string()` был изменён. До изменения возвращаемый тип был числом (0 или 1), теперь возвращается булево значение (`TRUE`, `FALSE`).

## Migrate

- [#2746541](https://www.drupal.org/node/2746541) Добавлена новая миграция `d*_node_complete.yml`, которая именуется как "complete node migration". Данная миграция переносить все ноды, их ревизии, переводы и переводы ревизий.
- [#2966856](https://www.drupal.org/node/2966856) Модуль `migrate_drupal_multilingual` удалён из зависимостей миграций и скрыт из интерфейса. Мультиязычные миграции теперь стабильны и данный модуль больше не требуется и будет удалён в Drupal 10.

## Update

- [#3098475](https://www.drupal.org/node/3098475) Теперь страница обновления БД будет показывать ожидающие обновления, зависимости и требования которых до сих пор не удовлетворены и они не могут быть применены.

## Views UI

- [#3113556](https://www.drupal.org/node/3113556) Views UI теперь использует библиотеку `core/drupal.dialog.ajax` вместо `core/jquery.ui.dialog`.

## Тестирование

- [#3105980](https://www.drupal.org/node/3105980) Метод `\Drupal\migrate_drupal\Tests\StubTestTrait::createStub` был переименован в `createEntityStub`.
- [#3118477](https://www.drupal.org/node/3118477) Реестр темы теперь создаётся через мок, для того чтобы избежать конфликта когда он создавался в двух разных местах `RegistryTest` и `RegistryLegacyTest`.
- [#3078671](https://www.drupal.org/node/3078671) Зависимость `behat/mink` обновлена до 1.8.0, а `behat/mink-selenium2-driver` до 1.4.0.

## Производительность

- [#3092180](https://www.drupal.org/node/3092180) Добавлен новый кэш контекст `protocol_version`, который позволяет иметь различные результаты для разных протоколов (HTTP/1.0, HTTP/1.1 или HTTP/2).

## Прочие изменения

- [#3083486](https://www.drupal.org/node/3083486) Конфигурация `action.settings.recursion_limit` и её схема была удалена. Drupal ядро не использовало данную конфигурацию.
- [#3111613](https://www.drupal.org/node/3111613) Удалены два `protected` метода у `Drupal\Core\Entity\Sql\SqlContentEntityStorageSchema`. Они не использовались ядром, а так как являются защищёнными, не относятся к публичному API.
- [#3113062](https://www.drupal.org/node/3113062) Функция `system_user_timezone()` помечена как `@internal` и будет удалена в [Drupal 9](../../9/releases/release-9.0.0.md).
- [#3054049](https://www.drupal.org/node/3054049) Добавлена новая [проверка доступа маршрута](../routing/route-access-control.md) `_entity_bundles`, при помощи которой можно создать [маршрут](../routing/routing.md) принимающий только конкретные бандлы сущности из аргумента. Например: `_entity_bundles: 'node:article|page'`.
- [#3114909](https://www.drupal.org/node/3114909) Документация для `EntityTypeInterface::hasHandlerClass` и `::getHandlerClass` приведены к одному виду.
- [#2999549](https://www.drupal.org/node/2999549) Добавлен новый служебный роут `<button>` для генерации ссылки в виде кнопки `route:<button>`.

## Смотрите также

- [Drupal 9](../../9/drupal-9.md)