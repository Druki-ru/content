---
title: Параметры маршрутов
slug: wiki/9/routing/parameters
core: 9
search-keywords:
  - параметры роутов
  - route parameters
  - структура параметров роута
metatags:
  title: 'Drupal 9: Параметры маршрутов'
  description: 'Как передавать параметры методу контроллера.'
authors:
  - a-kovrigin
---
Параметры маршрутов используются для передачи дынных контроллеру по URI.

Параметр должен быть указан в фигурных скобках в пути маршрута (например, маршрут с путем `/node/{node}/edit` передаст контроллеру объект класса `Node` в качестве аргумента).

## Пример использования
```yaml
foo.user:
  path: '/foo/{user}'
  defaults:
    _controller: '\Drupal\foo\Controller\FooController::greet'
  requirements:
    _permission: 'access content'
  options:
    parameters:
      user:
        type: entity:user
```
Элемент `{user}` пути маршрута (обычно называется `slug`) станет обязательным аргументом `$user` в методе контроллера.
>Важно: название переменной в методе контроллера должно совпадать с именем параметра. Таким образом, в маршруте описанном в примере выше переменная должна называться именно `$user`, так как именно такое имя параметра описано в разделе `options`, в противном случае контроллер вернёт исключение о том, что значение для аргумента не передано.

Если предполагается, что параметр должен являться объектом сущности, следует определить тип параметра в разделе options описания маршрута (`type: entity:user` из примера выше) для того, чтобы Drupal смог проверить существования сущности с переданным ID. Это нужно для того, чтобы Drupal мог отдать ответ "не найдено", если сущность с переданным ID не существует.

Если сущность с переданным ID существует, параметр будет автоматически конвертирован в объект объявленного типа сущности. В методе контроллера следует объявить параметр в качестве аргумента:
```php
<?php

namespace Drupal\foo\Controller;

use Drupal\Core\Session\AccountInterface;
use Drupal\Core\StringTranslation\TranslatableMarkup;
use Symfony\Component\HttpFoundation\Request;

/**
 * An example controller.
 */
class FooController {

  /**
   * Shows greeting message.
   */
  public function greet(AccountInterface $user, Request $request) {
    $build = [
      '#markup' => new TranslatableMarkup('Hello, @name!', [
        '@name' => $user->getAccountName(),
      ]),
    ];

    return $build;
  }

}
```
Как это работает:
1. Аргумент метода `$user` является экземпляром класса `Drupal\user\Entity\User`, который реализует интерфейс `AccountInterface`. В данном случае `$user` также может быть определён как `Drupal\user\UserInterface` или `Drupal\Core\Entity\EntityInterface` (подходит любой интерфейс, который реализует класс `User`). Использование класса `User` для определения данного аргумента также подходит, но рекомендуется использовать интерфейсы.
2. При определении аргумента типа `Request`, аргумент автоматически становится объектом запроса (в данном случае название переменной не важно).
> Порядок переменных в методе контроллера не важен.
3. Если в URL будет передан ID несуществующего пользователя, будет возвращён ответ "не найдено" (404).

## Параметры в контроллерах форм
Контроллеры, возвращающие формы также могут иметь параметры. В примере ниже форма получает объект пользователя по ID из URL:
```yml
foo.user_form:
  path: '/foo/form/{user}'
  defaults:
    _form: '\Drupal\foo\Form\UserForm'
  requirements:
    _permission: 'access content'
```
```php
<?php

namespace Drupal\foo\Form;

use Drupal\Core\Form\FormBase;
use Drupal\Core\Form\FormStateInterface;
use Drupal\Core\Session\AccountInterface;
use Drupal\Core\StringTranslation\TranslatableMarkup;

/**
 * Provides example user form.
 */
class UserForm extends FormBase {

  /**
   * {@inheritdoc}
   */
  public function getFormId() {
    return 'user_form';
  }

  /**
   * {@inheritdoc}
   */
  public function buildForm(array $form, FormStateInterface $form_state, AccountInterface $user = NULL) {
    $form['user_email'] = [
      '#type' => 'textfield',
      '#title' => new TranslatableMarkup('User email'),
      '#default_value' => $user ?? $user->getEmail(),
    ];

    $form['actions'] = [
      '#type' => 'actions',
    ];

    $form['actions']['submit'] = [
      '#type' => 'submit',
      '#value' => new TranslatableMarkup('Save'),
      '#button_type' => 'primary',
    ];
  }

  /**
   * {@inheritdoc}
   */
  public function submitForm(array &$form, FormStateInterface $form_state) {
    $this->messenger()->addStatus($form_state->getValue('user_email'));
  }

}
```
> Такой способ передачи сущностей в качестве параметров маршрута не используется в CRUD-формах сущностей.

> Важно: каждому аргументу формы являющемуся параметром маршрута обязательно указывать значение по умолчанию (пример из класса формы: `AccountInterface $user = NULL`).

## Параметры с явно указанным типом (typehinting)
Перечисленные ниже специальные объекты могут передаваться методу контроллера. Чтобы это работало, в методе контроллера должны быть объявлены аргументы с соответствующим типом. Название таких аргументов не важно.
* `\Symfony\Component\HttpFoundation\Request`: необработанный объект запроса Symfony. Обычно используется если контроллеру необходимо получить значения параметров запроса. Глобальная переменная `$_GET` доступна в методе `$request->query` (возвращает `ParameterBag`, не массив), информация о сессии может быть получена с помощью метода `$request->getSession()` (возвращает `SessionInterface`).
* `\Drupal\Core\Routing\RouteMatchInterface`: в переменную будет записана информация `соответствия маршрутов` (данные о результате выполнения процесса роутинга). Например, чтобы получить объект текущего маршрута, можно использовать метод `$route_match->getRouteObject()`.
* `\Psr\Http\Message\ServerRequestInterface`: Необработанный запрос, представленный с использованием формата PSR-7 ServerRequest.

## Множественные параметры для маршрута
Если маршруту требуется несколько параметров, каждый из них должен быть описан в разделе options:
```yml
foo.route_with_two_nodes:
  path: '/foo/{node1}/{node2}'
  defaults:
    _controller: '\Drupal\foo\Controller\FooController::twoNodes'
  options:
    parameters:
      node1:
        type: entity:node
      node2:
        type: entity:node
```
```php
<?php

namespace Drupal\foo\Controller;

use Drupal\Core\DependencyInjection\ContainerInjectionInterface;
use Symfony\Component\DependencyInjection\ContainerInterface;
use Drupal\node\NodeInterface;

/**
 * An example controller.
 */
class FooController implements ContainerInjectionInterface {

  /**
   * The entity type manager.
   *
   * @var \Drupal\Core\Entity\EntityTypeManagerInterface
   */
  private $entityTypeManager;

  /**
   * {@inheritdoc}
   */
  public static function create(ContainerInterface $container) {
    $instance = new static();
    $instance->entityTypeManager = $container->get('entity_type.manager');
    return $instance;
  }

  /**
   * Renders two node teasers.
   *
   * @param \Drupal\node\NodeInterface $node1
   *   The first node entity.
   * @param \Drupal\node\NodeInterface $node2
   *   The second node entity.
   *
   * @return array
   *   The nodes render array.
   */
  public function twoNodes(NodeInterface $node1, NodeInterface $node2) : array {
    /** @var \Drupal\node\NodeViewBuilder $node_view_builder */
    $node_view_builder = $this->entityTypeManager->getViewBuilder('node');

    return $node_view_builder->viewMultiple([$node1, $node2], 'teaser');
  }

}
```
## Опциональные параметры
Параметры маршрутов могут быть исключены если для них предоставлено значение по умолчанию в методе контроллера.
Пример задачи: для контроллера с формой которая позволяет пользователям отправлять различные запросы администратору сайта (вопрос, сообщение об опечатке, коммерческий запрос), если пользователь не указал тип запроса, сообщение должно считаться вопросом. Это можно сделать, передав значение по умолчанию для параметра маршрута в разделе `defaults`:
```yml
foo.report_form:
  path: '/foo/report/{issue_type}'
  defaults:
    _controller: '\Drupal\foo\Controller\FooIssueController::report'
    issue_type: 'question'
  requirements:
    _permission: 'report issue'
```
Теперь по адресу '/report' параметр `$issue_type` будет равен `question`. Для коммерческого запроса обращение к форме будет по адресу `/foo/report/commercial-request`.

Значения по умолчанию для параметров могут использоваться для маршрутов с фиксированным (не динамическим) адресом, у которых метод контроллера ожидает аргумент.
Например, если бы потребовалось создать отдельную страницу для коммерческих обращений, мы могли бы воспользоваться тем же контроллером, но отдельным маршрутом так:
если пользователь не указал тип запроса, сообщение должно считаться вопросом. Это можно сделать, передав значение по умолчанию для параметра маршрута в разделе `defaults`:
```yml
foo.commercial_requests:
  path: '/commercial-requests'
  defaults:
    _controller: '\Drupal\foo\Controller\FooIssueController::report'
    issue_type: 'commercial_request'
  requirements:
    _permission: 'report issue'
```
## Ссылки
- [Using parameters in routes](https://www.drupal.org/docs/8/api/routing-system/parameters-in-routes/using-parameters-in-routes) (англ.), Drupal.org
