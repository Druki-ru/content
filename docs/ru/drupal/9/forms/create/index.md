---
title: Создание формы
slug: wiki/9/forms/create
core: 9
metatags:
  title: 'Drupal 9: Создание формы'
  description: 'Создание форм в Drupal 9.'
category:
  area: Формы
  title: Создание
  order: 3
authors:
  - Niklan
---

Формы являются PHP объектами, расширяющие `Drupal\Core\Form\FormBase`. Для их создания необходимо создать соответствующий объект в любом неймспейcе модуля в `/src` директории.

> [!TIP]
> Хорошей практикой является создание форм в `/src/Form`.

<Aside type="tip">

Вы можете использовать [Drush](../../../../drush/index.md) для генерации форм.

</Aside>

## Создание обычный формы

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

> [!TIP]
> Для генерации простой формы при помощи [Drush](../../../../drush/index.md) используйте команду `drush generate form-simple`.

## Создание конфигурационной формы

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

> [!TIP]
> Для генерации конфигурационной формы при помощи [Drush](../../../../drush/index.md) используйте команду `drush generate form-config`.

## Ссылки

- [Drupal 8: Form API что изменилось и как использовать](https://niklan.net/blog/73), Niklan, 2015
