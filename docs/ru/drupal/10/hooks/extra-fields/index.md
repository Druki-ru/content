---
title: Псевдо-поля (экстра-поля)
slug: wiki/10/extra-fields
core: 10
search-keywords:
  - псевдо поля
  - экстра поля
  - extra fields
  - pesudo fields
metatags:
  title: 'Drupal 10: Псевдо-поля (экстра-поля)'
  description: 'Виртуальный поля позволяющие добавлять свою разметку или элементы в форму материалов сущностей.'
authors:
  - Niklan
  - chesn0k
---

**Extra fields** (также известны как Pseudo Fields, псевдо-поля, экстра-поля) — дополнительные поля используемые в «Управлении отображением» той или иной сущности, позволяющие добавлять любую дополнительную информацию в процессе рендера.

По умолчанию в Drupal все добавленные поля к [сущности](../../entities/index.md) имеют возможность настройки отображения и их рендера. Псевдо-поля позволяют генерировать своё содержимое на лету, и являются частью процесса рендера. Они могут быть подключены как к рендеру результата, так и к форме создания/редактирования сущности.

Псевдо-поля не имеют ни настроек, ни поддержки виджетов, как и не могут их добавлять. То что будет выведено, полностью зависит от самого псевдо-поля на основе предоставленнымх ему данных. Если вы хотите сделать виртуальное поле без хранения данных в БД, но с поддержкой виджетов, то вам следует обратить своё внимание на вычисляемые поля.

Управлять отображением псевдо поля можно через стандартный административный интерфейс, но для модификации будет доступен вес и видимость поля.

## hook_entity_extra_field_info()

Для того чтобы объявить своё псевдо-поле, необходимо реализовать [хук](../index.md) `hook_entity_extra_field_info()`. Хук должен вернуть массив из значений, которые могут содержать следующие ключи:

- `label`: Человекопонятная метка для данного поля. Отображается в административном интерфейсе.
- `description`: Описание поля, по умолчанию нигде не отображается.
- `weight`: Вес псевдо-поля в «Управлении отображением» по умолчанию.
- `visible`: (опционально) Логическое значение, будет ли данное поле по умолчанию отображаться или скрыто. Значение по умолчанию `TRUE` — поле будет сразу выводиться.
- `edit`: (опционально) Позволяет задать разметку, которая будет отображаться на странице «Управление содержимым». Работает только для контекста `form`.
- `delete`: (опционально) Позволяет задать разметку, которая будет отображаться на странице «Управление содержимым». Работает только для контекста `form`.

Значения псевдо-полей должны находиться в структуре массива по следующему паттерну: `$fields[{entity_type_id}][{bundle_id}][{extra_field_type}][{extra_field_name}]`, где:

- `{entity_type_id}`: Машинное имя [сущности](../../entities/index.md) для которого добавляется данное псевдо-поле.
- `{bundle_id}`: Подтип сущности для которого добавляется данное псевдо-поле.
- `{extra_field_type}`: Тип псевдо-поля, может быть одним из двух значений:
    - `display`: Данные псевдо-поля доступны при выводе материала.
    - `form`: Данные псевдо-поля доступны в формах редактирования материала.
- `{extra_field_name}`: Машинное имя псевдо-поля которое будет использовано в качестве ключа для хранения в отображении.

<Aside>

Убедитесь что значение `{extra_field_name}` не пересекается с другими ключами рендер массива сущности. Например, называя псевдо-поле с префиксом `field_*`, вы рискуете что оно пересечётся с названием уже существующего, реального поля, и приведёт к тому что вывод будет некорректным.

</Aside>

### Пример псевдо-поля типа display

Псевдо-поля типа `display` выводятся в момент рендера материала. Для их вставки используется [хук](../index.md) `hook_view()` и его аналоги.

Пример добавления псевдо-поля с обычной строкой к «Типу содержимого» (сущность node) «Новость» (news):

```php
/**
 * Implements hook_entity_extra_field_info().
 */
function example_entity_extra_field_info() {
  $extra = [];

  $extra['node']['news']['display']['extra_foo_bar'] = [
    'label' => new TranslatableMarkup('Foo bar field'),
    'description' => new TranslatableMarkup('Prints "foo bar" string.'),
    'weight' => 3,
  ];

  return $extra;
}

/**
 * Implements hook_ENTITY_TYPE_view().
 */
function example_node_view(array &$build, EntityInterface $entity, EntityViewDisplayInterface $display, $view_mode) {
  if ($display->getComponent('extra_foo_bar')) {
    $build['extra_foo_bar'] = [
      '#type' => 'markup',
      '#markup' => 'foo bar',
    ];
  }
}
```

### Пример псевдо-поля типа form

Псевдо-поля типа `form` выводятся в форме редактирования/создания материала. Для их вставки используется [хук](../index.md) `hook_form_alter()` и его аналоги.

Пример добавления псевдо-поля с валидацией его значения к «Типу содержимого» (сущность node) «Новость» (news). Данное поле будет ожидать что в него введут `foo bar`, иначе оно будет запрещать отправку формы.

```php
/**
 * Implements hook_entity_extra_field_info().
 */
function example_entity_extra_field_info() {
  $extra = [];

  $extra['node']['news']['form']['extra_foo_bar'] = [
    'label' => new TranslatableMarkup('Foo bar field'),
    'description' => new TranslatableMarkup('Enter "foo bar" to continue.'),
    'weight' => 3,
  ];

  return $extra;
}

/**
 * Implements hook_form_BASE_FORM_ID_alter().
 */
function example_form_node_form_alter(&$form, FormStateInterface $form_state, $form_id) {
  $storage = $form_state->getStorage();
  if (!empty($storage['form_display']) && $storage['form_display'] instanceof EntityFormDisplay) {
    $form_display = $storage['form_display'];
    if ($component = $form_display->getComponent('extra_foo_bar')) {
      $form['extra_foo_bar'] = [
        '#type' => 'textfield',
        '#title' => new TranslatableMarkup('Foo bar field'),
        '#default_value' => new TranslatableMarkup('Enter "foo bar" to continue.'),
        '#required' => TRUE,
        '#weight' => $component['weight'],
      ];
      $form['#validate'][] = 'example_foo_bar_validation';
    }
  }
}

/**
 * Validates 'extra_foo_bar' extra field.
 */
function example_foo_bar_validation(&$form, FormStateInterface $form_state) {
  if ($form_state->getValue('extra_foo_bar') != 'foo bar') {
    $form_state->setError($form['extra_foo_bar'], new TranslatableMarkup('Enter "foo bar" to proceed.'));
  }
}
```

## Расширения

- [Extra Field](https://www.drupal.org/project/extra_field) — добавляет возможность объявлять псевдо-поля типа `display` при помощи [плагинов](../../plugins/index.md). Рекомендуется к использованию вместо хуков, так как позволяет поддерживать код в чистоте и более организованно.

## Ссылки

- [Drupal 8: Создание псевдо-полей (Extra Fields)](https://niklan.net/blog/177), Niklan, 2018
- [Сравнение производительности различных способов вывода псевдо полей](http://xandeadx.ru/blog/drupal/1003), xandeadx.ru, 2021