---
title: Маршрутизация
slug: wiki/9/routing
core: 9
search-keywords:
  - роуты контроллеры маршруты роутинг
  - как добавить создать собственную свою кастомную страницу
  - доступ защита маршрутов контроллеров права доступа
  - структура свойства маршрутов маршрута
  - какие параметры можно задать маршруту маршрутам контроллеру контроллерам
metatags:
  title: 'Drupal 9: Маршрутизация'
  description: 'Как создавать свои собственные страницы, с уникальным адресом, требованиями и поведением.'
---

## Введение
Маршрутизация в Drupal 9 базируется на системе маршрутизации Symfony. Они имеют идентичный синтаксис объявления маршрутов, с некоторыми дополнительными возможностями специально для Drupal.

Маршрут или роут — путь, который возвращает какое-то определенное содержимое или результат. Когда вы обращаетесь к страницам, Drupal пытается найти подходящий маршрут для вашего запроса, затем выполнить его, а его результат вернуть клиенту, если он не смог подобрать подходящий маршрут, то будет возвращён HTTP 404.

Примерная схема работы маршрутов и контроллеров:

![Drupal 9 routing](https://www.drupal.org/files/Drupal8Routing.png)

## Маршруты и контроллеры
Маршрутизация состоит из двух частей:

1. Роут (маршрут) - содержит информацию о будущей странице, её путь, какой контроллер использовать, заголовок, требования и т.д.
1. Контроллер - объект, отвечающий за формирование результата по запрошенному маршруту.

### Маршруты

Маршруты задаются в специальном файле `*.routing.yml`, который должен находиться в корне модуля рядом с `*.info.yml`. Если ваш модуль имеет название `foo`, в таком случае файл будет называться `foo.routing.yml`.

В данном файле вы объявляете все необходимые вашему модулю маршруты и описываете их при помощи YAML формата. Например:

```yaml
foo.content:
  path: '/my-custom-page'
  defaults:
    _controller: '\Drupal\foo\Controller\FooController::content'
    _title: 'Hello World'
  requirements:
    _permission: 'access content'
```

> [!NOTE]
> В Drupal принято, чтобы маршруты имели название в следующем формате `modulename.route_name`.

Данный пример объясняет Drupal, что маршрут с названием `foo.content` должен быть обработан по адресу `/my-custom-page`. Когда пользователь обратится по данному маршруту, для него будет произведена проверка, имеет ли он доступ `access content`. Если не имеет, то пользователь получит ответ HTTP 403 "Недостаточно прав", если имеет, тогда результат, который возвращает контроллер. В качестве заголовка страницы будет использована строка "Hello World".

### Контроллеры

Drupal 9 использует компонент Symfony - [HTTP Kernel](https://symfony.com/doc/current/components/http_kernel.html), который получает запрос и опрашивает другие системы в надежде получить от них результат, который необходимо вернуть пользователю. Результатом является один из объектов, наследующих `Symfony\Component\HttpFoundation\Response` или непосредственно данный объект. В случае с Drupal, мы также имеем возможность вернуть render array в качестве результата. Drupal обнаружит его, проведет все необходимые обработки и операции с ним, а затем вернет корректный ответ при помощи `Response`.

Например, если мы хотим создать контроллер для примера из маршрутизации выше, то результат может быть примерно таким:

```php
<?php

namespace Drupal\foo\Controller;

use Drupal\Core\Controller\ControllerBase;

/**
 * An foo controller.
 */
class FooController extends ControllerBase {

  /**
   * Returns a render-able array for a test page.
   */
  public function content() {
    $build = [
      '#markup' => $this->t('Hello World!'),
    ];
    return $build;
  }

}
```

В результате чего, Drupal вернет в качестве контента страницы строку "Hello World" на языке пользователя.

> [!NOTE]
> При помощи [Drush](../../../../drush/index.md), вы можете быстро генерировать код для контроллеров, для этого воспользуйтесь командой `drush generate controller`.

## Структура маршрутов
Маршруты, объявляемые в `*.routing.yml` могут содержать следующие свойства:

- **path**: (обязательно) Путь маршрута, должен начинаться со слеша `path: '/example'`. Понимает динамически части, заключенные в фигурные скобки (например: `path: '/foo/{argument}/bar'`).
- **defaults**:
  - **Обязательный параметр.** Должен быть один из перечисленных:
    - **_controller**: Значение типа [callable (callback-функция)](https://www.php.net/manual/en/language.types.callable.php):
      - **Class:method**: В формате `\Drupal\[module name]\Controller\[ClassName]::[method]`, например `_controller: '\Drupal\foo\Controller\FooController::build'`.

        В данном случае Drupal вызовет метод `build()` класса `FooController` из неймспейса `\Drupal\foo\Controller`.

      - **[Сервис](../../services/index.md)**: Позволяет формировать значение из сервиса. Значение задаётся в формате `mymodule.service_name:[method]`, где `mymodule.service_name` - название сервиса, а `[method]` - название метода сервиса, который необходимо вызвать. Например `_controller: 'mymodule.service_name:build'`.
    - **_form**: Класс, реализующий `Drupal\Core\Form\FormInterface`.
    - **_entity_view**: Значение в формате `[entity_type].[view_mode]`. Маршрут найдёт сущность в пути и отрендерит её в указанном формате отображения. Например `_entity_view: node.teaser`.
    - **_entity_list**: Значение в формате `[entity_type]`. Выводит список сущностей указанного типа при помощи `EntityListController` данной сущности. Например `_entity_list: user` выведет список всех пользователей на сайте.
    - **_entity_form**: Значение в формате `[entity_type].[form_view_mode]`. Выведет форму редактирования сущности указанного типа, в определенном формате отображения формы. Например `_entity_form: node.default` выведет форму редактирования материала в стандартном режиме отображения формы.
    - **_route**: Название маршрута, чьим алиасом вы хотите сделать данный маршрут. Например `_route: views.files.page_1`.
  - **Опциональные параметры.**
    - **_title**: Заголовок страницы на **английском** языке. Данное значение будет обработано `t()`.
    - **_title_arguments**: Дополнительные аргументы, передаваемые с заголовком в `t()`.
    - **_title_context**: Контекст, передаваемый с заголовком в `t()`.
    - **_title_callback**: Значение типа callable, которое возвращает значение для заголовка. В данном случае, перевод значения ложится на callback-функцию.
- **methods**: (опционально) В дополнение к пути, вы можете ограничить типы запросов, на который будет реагировать маршрут. Значение должно быть в виде массива с перечислением всех допустимых типов запроса. Например `methods: [GET, POST, UPDATE, DELETE]`.
- **requirements**: (обязательно) Определяет, какие условия должны быть удовлетворены, для того чтобы получить доступ к маршруту. Должно быть одно или более значений (все они должны пройти успешно).
  - **_permission**: Строковое значение прав доступа, например `_permission: 'access content'`. Может принимать несколько значений  разделяемых запятой `,` (логический оператор OR) (например: `_permissions: 'access content,access user profiles`) или плюсом `+` (логический оператор AND) (например: `_permissions: 'acess content+access user'`).
  - **_role**: Строковое значение роли, например `_role: 'administrator'`. Вы также можете использовать указание нескольких ролей, разделяя их запятой `,` (логический оператор OR), или плюсом `+` (логический оператор AND). Рекомендуется использовать `_permission`, вместо `_role`, так как на различных сайтах роли могут иметь различные настройки и смыслы.
  - **_access**: Установите значение равное `'TRUE'`, чтобы не иметь никаких проверок и результат отдавался при любых условиях.
  - **_entity_access**: Значение в формате `[entity_type].[operation]`. Доступ будет предоставлен только если пользователь проходит проверки определенных прав, конкретного типа сущности. Например `_entity_access: menu.edit`.
  - **_format**: Производит проверку типа запроса. Например, указав `_format: json`, маршрут будет обрабатываться, только если указать `?_format=json` при обращении по пути маршрута.
  - **_content_type_format**: Производит проверку по значению заголовка запроса `Content-Type`. Например, если зададите `_content_type_format: json`, то маршрут отработает только если при запросе был передан заголовок `Content-Type: application/json`.
  - **_custom_access**: Позволяет задать метод, который будет вызван для проверки прав доступа. Например `_custom_access: '\Drupal\foo\Controller\FooController::access'`
  - **_module_dependencies**: Производит проверку по модулю. Вы также можете использовать указание нескольких модулей, разделяя их запятой `,` (логический оператор OR), или плюсом `+` (логический оператор AND). Если указанный модуль отключен, то маршрут будет недоступен. Если модуль имеет зависимости, они также автоматически подразумевают проверку, поэтому нет никакого смысла указывать все зависимости.
  - **_csrf_token**: Значение равное `'TRUE'` произведет проверку `X-CSRF-Token` из заголовка запроса, для проверки, является ли запрос валидным. Используйте, для того чтобы убедиться что запрос валидный и не изменен.
  - Любой другой существующий Access Check сервис. Для более подробной информации обратитесь к разделу [контроля доступа маршрутов](../access-control/index.md).
- **options**: (опционально) Дополнительные настройки, с которыми маршрут должен взаимодействовать. Распространенные:
  - **_admin_route**: Помечает данный маршрут как часть административного интерфейса. Для всех маршрутов начинающихся с `/admin`, данное значение автоматически устанавливается в `'TRUE'`.
  - **_auth**: Доступные способы авторизации для маршрута. По умолчанию доступны все варианты авторизации с значением `global`. Данное значение ограничивает варианты авторизации только для указанных. Например `_auth: ['basic_auth', 'cookie']`. Разработчики могут объявлять свои собственные способы авторизации при помощи [Authentication API](https://niklan.net/blog/166).
  - **_maintenance_access**: Значение равное `'TRUE'` позволяет отрабатывать данному маршруту в режиме обслуживания.
  - **no_cache**: Значение равное `'TRUE'` отключает кеширование ответа по этому маршруту.
  - **parameters**: TODO.

### Примеры

#### Комплексный пример маршрутов

**foo.routing.yml**

```yaml
foo.render:
  path: '/book'
  defaults:
    _controller: '\Drupal\foo\Controller\FooController::fooRender'
  requirements:
    _permission: 'access content'

foo.export:
  path: '/foo/export/{type}/{node}'
  defaults:
    _controller: '\Drupal\foo\Controller\FooController::fooExport'
  requirements:
    _permission: 'access printer-friendly version'
    _entity_access: 'node.view'
  options:
    _admin_route: TRUE
```

#### Передача аргументов в контроллер

```yaml
foo.content:
  path: '/example'
  defaults:
    _controller: '\Drupal\foo\Controller\FooController::content'
    custom_arg: 17
  requirements:
    _permission: 'access content'
```

Данное значение будет передано методу в качестве аргумента.


```php
  public function content(Request $request, $custom_arg) {
    // $custom_arg == 17.
  }
```

## Контроль доступа
Каждый маршрут в Drupal должен иметь, по крайней мере одну, проверку доступа. Данные требования указываются в `requirements` разделе маршрута.

В данном разделе маршрута указываются какие Access Check сервисы будут использованы для проверки доступа. Ядро предоставляет достаточное количество данных проверок, ознакомиться с  ними вы можете в материале [структура маршрутов](../structure-of-routes/index.md).

Из предоставленного списка, можно выделить `_custom_access`, который позволяет указать в качестве проверки доступа необходимый объект и его метод. Например: `_custom_access: '\Drupal\foo\Controller\FooController::access'`.

Также вы можете объявлять свои access check сервисы и использовать их в маршрутах.

### Использование _custom_access

Данная проверка доступа подразумевает что вы напишите собственную логику для маршрута. Вы должны указать в качестве значения полный путь, включая неймспейс, до объекта и его метода, который будет вызван для проверки прав доступа к конкретному маршруту.

Пример использования:

```yaml
foo.content:
  path: '/my-custom-page'
  defaults:
    _controller: '\Drupal\foo\Controller\FooController::content'
    _title: 'Hello World'
  requirements:
    _custom_acess: '\Drupal\foo\Controller\FooController::access'
```

В качестве результата данный метод должен возвращать экземпляр объекта `Drupal\Core\Access\AccessResultInterface`.

Пример проверки прав доступа:

```php
<?php

namespace Drupal\foo\Controller;

use Drupal\Core\Controller\ControllerBase;
use Drupal\Core\Access\AccessResult;
use Drupal\Core\Session\AccountInterface;

/**
 * An foo controller.
 */
class FooController extends ControllerBase {

  /**
   * Returns a render-able array for a test page.
   */
  public function content() {
    $build = [
      '#markup' => $this->t('Hello World!'),
    ];
    return $build;
  }

  /**
   * Returns access result for controller.
   */
  public function access(AccountInterface $account) {
    return AccessResult::allowedIf(
      $account->hasPermission('do example things')
      && $account->hasPermission('do other things')
    );
  }

}
```

### Написание Access Check сервиса

Разработчики могут описывать свои access check сервисы. Например `_custom_access` выше тоже является access check сервисом.

Отличие access check от `_custom_access` в том, что собственный сервис вы можете применять к любым маршрутам. Он должен быть максимально универсальным в пределах тех маршрутов, к которым предназначен, тогда как в `_custom_access`, скорее всего, вы напишите индивидуальную проверку доступа, которая касается только данного маршрута и больше никакого.

Access check является [сервисом](../../services/index.md) и поддерживает все его возможности. Объекты, используемые для access check сервисов рекомендуется создавать в `src/Access`.

Пример объекта с проверкой доступа:

```php
<?php
namespace Drupal\foo\Access;

use Drupal\Core\Session\AccountInterface;
use Drupal\Core\Access\AccessResult;
use Drupal\Core\Routing\Access\AccessInterface;

/**
 * Checks access for displaying configuration translation page.
 */
class FooAccessCheck implements AccessInterface {

  /**
   * A custom access check.
   *
   * @param \Drupal\Core\Session\AccountInterface $account
   *   Run access checks for this account.
   *
   * @return \Drupal\Core\Access\AccessResultInterface
   *   The access result.
   */
  public function access(AccountInterface $account) {
    // Check permissions and combine that with any custom access checking needed. Pass forward
    // parameters from the route and/or request as needed.
    return ($account->hasPermission('do example things') && $this->someOtherCustomCondition()) ? AccessResult::allowed() : AccessResult::forbidden();
  }

}
```

Метод должен возвращаться экземпляр объекта `Drupal\Core\Access\AccessResultInterface` в качестве результата проверки прав доступа.

В качестве аргументов метод проверки доступа принимает:

- Первым делом все динамические части маршрута `{name}`. Они должны идти в том же порядке что и в `path` и теми же названиями.
- Любой из данных объектов в любом порядке:
  - `\Symfony\Component\HttpFoundation\Request $request`.
  - `\Symfony\Component\Routing\Route $route`
  - `\Drupal\Core\Routing\RouteMatch $route_match`
  - `\Drupal\Core\Session\AccountInterface $account`

Далее вам необходимо объявить данный объект как сервис, например:

```yaml
services:
  foo.access_checker:
    class: Drupal\foo\Access\FooAccessCheck
    tags:
      - { name: access_check, applies_to: _foo_access_check }
```

После чего вы можете применять его к любым маршрутам на сайте:

```yaml
foo.content:
  path: '/my-custom-page'
  defaults:
    _controller: '\Drupal\foo\Controller\FooController::content'
    _title: 'Hello World'
  requirements:
    _foo_access_check: 'TRUE'
```

## Изменения в релизах

- **[Drupal 9.2.0](../../releases/9/9.2.x/9.2.0/index.md) (02.06.2021):** Параметр `_entity_bundles` помечен устаревшим.

## Ссылки

- [Routing](https://symfony.com/doc/current/routing.html) (англ.), Symfony
- [Drupal 8 Hello World: Пишем свой первый модуль](https://niklan.net/blog/66), Niklan, 2014
