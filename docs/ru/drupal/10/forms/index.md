---
title: Формы
slug: wiki/10/forms
core: 10
search-keywords:
  - как создать добавить объявить форму
  - собственные кастомные формы
metatags:
  title: 'Drupal 10: Формы'
  description: 'Form API позволяет создавать, валидировать и обрабатывать отправки форм на сайте.'
authors:
  - Niklan
---

**Form API** позволяет создавать, валидировать и обрабатывать отправки форм на сайте.

## Введение

Формы — неотъемлемая часть множества сайтов, независимо от его размера и назначения. Формы создаются на странице с использованием одноимённого HTML-тега `<from>`, после чего, данные внутри формы отправляются на обработку.

Если вы разрабатываете простую HTML-страницу или же используете небольшой фреймворк, то вы, вероятнее всего, будете использовать HTML-тег `<form>` и свои контроллеры для обработки отправленных данных.

В случае Drupal, где всё должно быть легко модифицируемым, заменяемым и дополняемым, классический подход не работает по ряду причин:

- Работать с HTML в PHP намного сложнее чем с PHP массивами.
- В случае, если форма обслуживается сразу несколькими [модулями](../modules/index.md), появляется конфликт того, кто и как должен отвечать за обработку данных формы, ведь `action` аттрибут может содержать лишь одно значение. Если оригинальный обработчик формы это никак не предусматривает, это может стать серьезной проблемой.

Для решения данных проблем в Drupal используются рендер-массивы, а Form API активно полагается на них. Это позволяет работать с формой на PHP, как с обычным массивом, что сразу решает все проблемы разом и добавляет новые возможности:

- Стабильная и предсказуемая разметка для всех форм на сайте, поддающаяся модификации при помощи [тем оформления](../themes/index.md).
- Все формы в Drupal работают через единый API, поэтому не важно, где и какой оригинальный обработчик у формы, так как это определяется на уровне Form API, а Drupal сам передаёт данные на валидацию и обработку в необходимом порядке всем желающим.
- Формы, объявленные одним модулем, могут быть легко изменены или дополнены другим модулем при помощи [хуков](../hooks/index.md), без необходимости производить сложные операции или что-либо хакать.
- Комплексные элементы форм, например, загрузка файлов или виджет рейтинга, могут быть объявлены как компоненты и использоваться в любых формах и любом количестве, автоматически со всей необходимой логикой обработки данных полученных в форме, предоставляя готовый результат.

## Состояние формы

Для того чтобы все этапы формы могли «общаться» между собой, а также все сторонние обработчики могли без проблем получать, редактировать данные или вносить новые — используется хранилище состояния формы.

Состояние формы в Drupal представлено в виде класса `Drupal\Core\Form\FormState` и передаётся на все этапы формы и всем его обработчикам в качестве аргумента.

Для каждой формы, на момент инициализации создаётся экземпляр данного класса, в котором хранится вся необходимая информация о форме:

- Данные, отправленные с использованием формы. Как сырые, так и санитизированные.
- Временное хранилище, через которое можно передавать дополнительную информацию между этапами и обработчиками формы.
- Информация о всех ошибках формы и возможность их добавления \ очистки.
- Системная информация о состоянии формы.

### Работа со значениями формы

Для работы со значениями формы используются следующие методы:

- `&getValues()`: Возвращает массив значений формы.
- `&getValue($key, $default = NULL)`: Получает конкретное значение формы с возможностью задать значение по умолчанию. `$key` может быть либо строкой с конкретным ключом, либо массивом с ключами иерархии значений. Например, если значение находится в `$values['foo']['bar']`, и вы хотите получить только данное значение, вы можете запросить его по `getValue(['foo', 'bar'])`.
- `setValues(array $values)`: Устанавливает массив в качестве значений формы, полностью заменяя те что имеются.
- `setValue($key, $value)`: Устанавливает конкретное значение формы. `$key` имеет поведение как у `getValue()`.
- `unsetValue($key)`: Удаляет конкретное значение формы. `$key` имеет поведение как у `getValue()`.
- `hasValue($key)`: Проверяет при помощи `isset()`, имеется ли значение в форме. `$key` имеет поведение как у `getValue()`.
- `isValueEmpty($key)`: Проверяет при помощи `empty()`, имеется ли значение в форме. `$key` имеет поведение как у `getValue()`.

### Работа с ошибками формы

Для работы с ошибками формы используются следующие методы:

- `hasAnyErrors()`: Возвращает логическое значение, есть ли в форме на текущей момент ошибки или нет.
- `setErrorByName($name, $message = '')`: Устанавливает ошибку на элемент по его названию.
- `setError(array &$element, $message = '')`: Устанавливает ошибку на элемент по его ссылке.
- `clearErrors()`: Очищает информацию о всех ошибках на данный момент.
- `getErrors()`: Возвращает ассоциативный массив с информацией о всех ошибках на данный момент.
- `getError(array $element)`: Получает информацию об ошибке для конкретного элемента.

## Класс формы

Каждая форма в Drupal является классом, расширяющим `Drupal\Core\Form\FormBase`. Данный класс предоставляет базовый функциона для описания своей формы.

В классе описываются все необходимые этапы, сама форма и её поведение. Drupal также содержит ряд уже более высокоуровневых заготовок для форм, которые вы можете использовать в качестве основы для своей формы.

<Aside type="note">

Ниже представлены основные, но не все, абстрактные классы для создания форм.

</Aside>

### Абстракция `Drupal\Core\Form\FormBase`

`Drupal\Core\Form\FormBase` — основной абстрактный класс, который необходимо расширять при создании своей формы. Используется для любого типа форм.

### Абстракция `Drupal\Core\Form\ConfigFormBase`

`Drupal\Core\Form\ConfigFormBase` — данный абстрактный класс рекомендуется использовать если вы создаёте форму, данные которой загружаются или сохраняются в конфигруации. Как правило, это формы с какими-либо настройками.

Данная абстракция предоставляет дополнительные методы для быстрой работы с нужной конфигурацией и доступа к её данными и записи в неё.

### Абстракция `Drupal\Core\Form\ConfirmFormBase`

`Drupal\Core\Form\ConfirmFormBase` — абстрактный класс, используемый для создания простых форм для подтверждения тех или иных действий пользователя. Например, при удалении материалов.

Данная абстракция по сути уже готовая, вам лишь необходимо вернуть сообщения с информацией о том, что подтверждает или не подтверждает пользователь, а так же обработку успешного подтверждения.

## Этапы формы

Все формы имеют три этапа: **подготовка формы**, **валидация формы** и **обработка формы**.

### Построение формы

**Построение формы** — этап, на котором описывается структура формы, её элементы и начальные требования к ним.

Построение формы производится при помощи метода `::buildForm()`. Задача данного метода, вернуть рендер массив с формой.

```php
  /**
   * {@inheritdoc}
   */
  public function buildForm(array $form, FormStateInterface $form_state): array {
    $form['message'] = [
      '#type' => 'textarea',
      '#title' => $this->t('Message'),
      '#required' => TRUE,
      '#default_value' => $this->t('Hello World'),
    ];

    $form['actions']['#type'] = 'actions';
    $form['actions']['submit'] = [
      '#type' => 'submit',
      '#value' => $this->t('Send'),
    ];

    return $form;
  }
```

### Валидация формы

**Валидация формы** — этап, который запускается после отправки формы. В данном этапе уже доступны данные отправленные с формой, как в сыром, так и очищенном виде.

**Цель валидации** — предотвратить дальнейшую обработку формы в случае обнаружения ошибок. Любая ошибка в форме обнаруженная в момент валидации, прекращает обработку, и возвращает пользователю ошибку(и) и подсвечивает проблемные элементы.

На данном этапе не должно быть никакой логики, которая не имеет прямого отношения к валидации. Вы должны установить ошибки на все проблемные элементы с указанием ошибок.

```php
  /**
   * {@inheritdoc}
   */
  public function validateForm(array &$form, FormStateInterface $form_state): void {
    if (mb_strlen($form_state->getValue('message')) < 10) {
      $form_state->setErrorByName('name', $this->t('Message should be at least 10 characters.'));
    }
  }
```

### Обработка формы

**Обработка формы** — финальный этап, который запускается в случае успешной отправки формы прошедшей этап валидации. На данном этапе конечная обработка отправленных данных.

```php
  /**
   * {@inheritdoc}
   */
  public function submitForm(array &$form, FormStateInterface $form_state): void {
    $this->messenger()->addStatus($this->t('The message has been sent.'));
    $form_state->setRedirect('<front>');
  }
```

## Создание формы

Для того чтобы объявить собственную форму, вам необходимо создать класс расширяющий один из абстрактных классов для создания форм. В данном классе вы обязательно должны объявить ID формы и необходимые этапы формы.

Классы форм могут быть объявлены в [модулях](../modules/index.md). В Drupal обычно формы описываются в пространстве имён `Drupal\modulename\Form` (`modulename/src/Form`).

Каждая форма должна содержать (при наследовании от `Drupal\Core\Form\FormBase`):

- **ID формы:** Уникальный ID формы в видео строки. ID формы может содержать только латинские символы в нижнем регистре и нижнее подчёркивание. Данное значение используется сторонними модулями для обращения к вашей форме и подключения к ней.
- **Построение формы:** Форма без элементов — бесполезна, поэтому она должна содержать описание элементов.
- **Обработка формы:** Вы должны описать, что необходимо делать с данными формы.

Валидация формы является опциональной и используется только тогда, когда в ней есть потребность. Данный этап будет вызываться, но `Drupal\Core\Form\FormBase` имеет «заглушку» для него, которая ничего не делает.

### Создание обычной формы

Пример `/src/Form/FooForm`:

```php
<?php

namespace Drupal\foo\Form;

use Drupal\Core\Form\FormBase;
use Drupal\Core\Form\FormStateInterface;
use Drupal\Core\Session\AccountProxyInterface;
use Symfony\Component\DependencyInjection\ContainerInterface;

/**
 * Provides a Foo form.
 */
class FooForm extends FormBase {

  /**
   * The current user.
   *
   * @var \Drupal\Core\Session\AccountProxyInterface
   */
  protected $currentUser;

  /**
   * Constructs a new FooForm object.
   *
   * @param \Drupal\Core\Session\AccountProxyInterface $account_proxy
   *   The account proxy.
   */
  public function __construct(AccountProxyInterface $account_proxy) {
    $this->currentUser = $account_proxy;
  }

  /**
   * {@inheritdoc}
   */
  public static function create(ContainerInterface $container) {
    return new static(
      $container->get('current_user')
    );
  }

  /**
   * {@inheritdoc}
   */
  public function getFormId() {
    return 'foo';
  }

  /**
   * {@inheritdoc}
   */
  public function buildForm(array $form, FormStateInterface $form_state) {

    $form['message'] = [
      '#type' => 'textarea',
      '#title' => $this->t('Message'),
      '#required' => TRUE,
      '#default_value' => $this->currentUser->getAccountName(),
    ];

    $form['actions'] = [
      '#type' => 'actions',
    ];
    $form['actions']['submit'] = [
      '#type' => 'submit',
      '#value' => $this->t('Send'),
    ];

    return $form;
  }

  /**
   * {@inheritdoc}
   */
  public function validateForm(array &$form, FormStateInterface $form_state) {
    if (mb_strlen($form_state->getValue('message')) < 10) {
      $form_state->setErrorByName('name', $this->t('Message should be at least 10 characters.'));
    }
  }

  /**
   * {@inheritdoc}
   */
  public function submitForm(array &$form, FormStateInterface $form_state) {
    $this->messenger()->addStatus($this->t('The message has been sent.'));
    $form_state->setRedirect('<front>');
  }

}
```

<Aside type="tip">

Для генерации простой формы при помощи [Drush](../../../../drush/index.md) используйте команду `drush generate form-simple`.

</Aside>

### Создание конфигурационной формы

Конфигурационный формы расширяют `Drupal\Core\Form\ConfigFormBase`, который предоставляет ряд дополнительных возможностей поверх `Drupal\Core\Form\FormBase`.

Конфигурационные формы имеют дополнительный метод `getEditableConfigNames()`, который должен возвращать массив состоящий из идентификаторов конфигураций. В отличии от `FormBase::config()`, данные конфигурации будут загружены в режиме для чтения и записи, так, вы сможете без проблем записать туда необходимые данные в обработчике формы.

Пример `/src/Form/FooSettingsForm`:

```php
<?php

namespace Drupal\foo\Form;

use Drupal\Core\Form\ConfigFormBase;
use Drupal\Core\Form\FormStateInterface;

/**
 * Configure Foo settings for this site.
 */
class FooSettingsForm extends ConfigFormBase {

  /**
   * {@inheritdoc}
   */
  public function getFormId() {
    return 'foo_foo_settings';
  }

  /**
   * {@inheritdoc}
   */
  protected function getEditableConfigNames() {
    return ['foo.settings'];
  }

  /**
   * {@inheritdoc}
   */
  public function buildForm(array $form, FormStateInterface $form_state) {
    $form['example'] = [
      '#type' => 'textfield',
      '#title' => $this->t('Example'),
      '#default_value' => $this->config('foo.settings')->get('example'),
    ];
    return parent::buildForm($form, $form_state);
  }

  /**
   * {@inheritdoc}
   */
  public function validateForm(array &$form, FormStateInterface $form_state) {
    if ($form_state->getValue('example') != 'example') {
      $form_state->setErrorByName('example', $this->t('The value is not correct.'));
    }
    parent::validateForm($form, $form_state);
  }

  /**
   * {@inheritdoc}
   */
  public function submitForm(array &$form, FormStateInterface $form_state) {
    $this->config('foo.settings')
      ->set('example', $form_state->getValue('example'))
      ->save();
    parent::submitForm($form, $form_state);
  }

}
```

<Aside type="tip">

Для генерации конфигурационной формы при помощи [Drush](../../../../drush/index.md) используйте команду `drush generate form-config`.

</Aside>

## Ссылки

- [Drupal 8: Form API что изменилось и как использовать](https://niklan.net/blog/73), Niklan, 2015