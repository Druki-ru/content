---
title: 'Drupal 9.1.5'
slug: wiki/drupal/releases/9.1.5
core: 9
metatags:
  title: 'Drupal 9.1.5: Список изменений'
  description: 'Список изменений Drupal 9.1.5.'
authors:
  - Niklan
  - chesn0k
---

**Дата релиза**: 4 марта 2021

## Добавлен опциональный параметр для StatementInterface::fetchObject()

- [#3128548](https://www.drupal.org/project/drupal/issues/3128548)

Метод `Drupal\Core\Database\StatementPrefetch::fetchObject()` и его интерфейс `Drupal\Core\Database\StatementInterface::fetchObject()` мимикируют под реализацию `fertchObject()` [в PDO](https://www.php.net/manual/en/pdostatement.fetchobject.php). Для того чтобы они были идентичны добавлен второй параметр:

```php
  /**
   * Fetches the next row and returns it as an object.
   *
   * The object will be of the class specified by StatementInterface::setFetchMode()
   * or stdClass if not specified.
   *
   * @param string|null $class_name
   *   Name of the created class.
   * @param array $constructor_args
   *   Elements of this array are passed to the constructor.
   *
   * @return mixed
   *   The object of specified class or \stdClass if not specified. Returns
   *   FALSE or NULL if there is no next row.
   */
  public function fetchObject($class_name = NULL, $constructor_args = []);
```

## В миграцию ContentEntity добавлена возможность указывать ключ источника в котором хранится ID ревизии

- [#3184650](https://www.drupal.org/project/drupal/issues/3184650) 

Ранее, миграция `content_entity` (`Drupal\migrate_drupal\Plugin\migrate\source\ContentEntity`) использовалась в ситуациях, когда сайт источник был идентичен сайту назначения.

В [Drupal](../../../../index.md) [8.9](../../../8/8.9.x/8.9.0/index.md), [9.0](../../9.0.x/9.0.0/index.md) и [9.1](../9.1.0/index.md), плагин источника данных `content_entity` всегда включал ключ для ID ревизии, так, как если бы сущность источника поддерживала ревизии. (Пример: node, media, taxonomy_term, но не user, file).

В версиях [Drupal 8.8](../../../8/8.8.x/8.8.0/index.md) и ранее, ключ ID ревизии никогда не добавлялся.

Начиная с текущей версии вы можете указать, включать ли ID ревизии в миграцию или нет, используя настройку `revisions`. Допустимые значения `none` и `add key`.

Новая настройка опциональна, а значение по умолчанию `revisions: add key` - что равносильно поведение до текущего изменения.

> [!IMPORTANT]
> Значение по умолчанию может измениться в будущем. Рекомендуется указать данное значение своим миграциям даже если оно соответствует текущему.

**Ранее:**

```yaml
source:
  plugin: content_entity:node
```

**Начиная с данного релиза:**

Для того чтобы ID ревизии не использовался (поведение Drupal 8.8-):

```yaml
source:
  plugin: content_entity:node
  revisions: none
```

Для того чтобы ID ревизии добавлялся (поведение Drupal 8.9, 9.0, 9.1):

```yaml
source:
  plugin: content_entity:node
  revisions: add key
```

## Claro

- [#3116377](https://www.drupal.org/project/drupal/issues/3116377) Улучшено отображение элемента с автодополнением в раскрытых фильтрах Views.

## Configuration System

- [#3196050](https://www.drupal.org/project/drupal/issues/3196050) Исправлена документация для `StorageConfigBase::validateValue()`.

## Entity System

- [#2693485](https://www.drupal.org/project/drupal/issues/2693485) Пункты добавления материалов (`/node/add`) теперь сортируются по метке, а не по машинному имени.

## File

- [#3174349](https://www.drupal.org/project/drupal/issues/3174349) `file_url_transform_relative()` теперь поддерживает порты отличные от 80 и 443.

## Form System

- [#997826](https://www.drupal.org/project/drupal/issues/997826) Исправлена работа свойства `#state` для элемента формы `text_format`.

## Forum

- [#3197754](https://www.drupal.org/project/drupal/issues/3197754) Кнопка «Add new Form topic» больше не показывается на разделах форума.

## Layout Builder

- [#3144010](https://www.drupal.org/project/drupal/issues/3144010) Исправлена неполадка из-за которой было невозможно удалять [псевдо-поля](../../../../9/hooks/extra-fields/index.md).

## Migration System

- [#2558857](https://www.drupal.org/project/drupal/issues/2558857) Оптимизирована очистка памяти после загрузки сущностей.
- [#3165944](https://www.drupal.org/project/drupal/issues/3165944) Миграция `d7_shortcut` больше не указывает в качестве зависимости миграцию `d7_menu_links`.
- [#3189476](https://www.drupal.org/project/drupal/issues/3189476) Миграция `node_translation_menu_links` теперь имеет опциональные зависимости `d6_menu` и `d7_menu`.
- [#3196177](https://www.drupal.org/project/drupal/issues/3196177) Добавлена документация для плагинов источников `VariableMultiRow`, `Variable`, `d6/VariableTranslation` и `d7/VariableTranslation`.
- [#3187415](https://www.drupal.org/project/drupal/issues/3187415) Миграции переводов для системных настроек теперь зависят от базовых миграций этих настроек.
- [#3176394](https://www.drupal.org/project/drupal/issues/3176394) Типы комментариев больше не мигрируют если в источнике отключен модуль `comment`.
- [#2954982](https://www.drupal.org/project/drupal/issues/2954982) Исправлена неполадка в `EntityContentBase::processStubRow()` приводящая к ошибкам в некоторых ситуациях.
- [#3178966](https://www.drupal.org/project/drupal/issues/3178966) Миграции Drupal 2 Drupal теперь будут выдавать ошибки при попытке мигрировать комментарии если на сайте-источнике они отключены.
- [#3189587](https://www.drupal.org/project/drupal/issues/3189587) Добавлена документация для плагинов источников связанных с таксономией.
- [#3084477](https://www.drupal.org/project/drupal/issues/3084477) Исправлены неполадки в тестах для `migrate_drupal_ui`.
- [#3197749](https://www.drupal.org/project/drupal/issues/3197749) Улучшена документация и добавлен пример для плагина источника `empty`.
- [#3097312](https://www.drupal.org/project/drupal/issues/3097312) Миграции созданные при помощи дериватив и имеющие плагин `migration_lookup`, больше не указываются в качестве опциональных зависимостей. Ранее, подобные миграции имели зависимости друг на друга, хотя могли быть не связаны между собой.
- [#2579361](https://www.drupal.org/project/drupal/issues/2579361) Улучшена документация для `Row::setSourceProperty()`.

## Olivero

- [#3182711](https://www.drupal.org/project/drupal/issues/3182711) Блок "помощи" теперь располагается в регионе `content_above` вместо несуществующего `help`.
- [#3182134](https://www.drupal.org/project/drupal/issues/3182134) Обновлены установочные конфигурации для темы.
- [#3196425](https://www.drupal.org/project/drupal/issues/3196425) Удалены стили для `::selection`.
- [#3197721](https://www.drupal.org/project/drupal/issues/3197721) Добавлен прелоад Metropolis-Regular.
- [#3190140](https://www.drupal.org/project/drupal/issues/3190140) Описание для элемента раскрытия меню теперь содержит заголовок раскрываемого пункта.

## Views

- [#2470753](https://www.drupal.org/project/drupal/issues/2470753) Улучшена документация для `hook_views_analyze()`.

## Тестирование

- [#2571475](https://www.drupal.org/project/drupal/issues/2571475) `KernelTestBase` теперь могу производить внешние HTTP запросы.
- [#3187309](https://www.drupal.org/project/drupal/issues/3187309) Сравнения с использованием XPath для `<select>` и `<option>` заменены на современные методы.
- [#3189607](https://www.drupal.org/project/drupal/issues/3189607) Сравнения с использованием XPath для `<input type="checkbox">` заменены на современные методы.
- [#3201113](https://www.drupal.org/project/drupal/issues/3201113) Исправлена опечатка в `PhpUnitVersionDependentTestCompatibilityTrait`.

## Прочие изменения

- [#3195951](https://www.drupal.org/project/drupal/issues/3195951) Из `MAINTAINERS.txt` удалён пустой раздел "Provisional membership".
- [#3195277](https://www.drupal.org/project/drupal/issues/3195277) Из `MAINTAINERS.txt` удалены упоминания Drupal 8.
- [#3196391](https://www.drupal.org/project/drupal/issues/3196391) Упоминание "Drupal Core" в `MAINTAINERS.txt` приведено к единому стилю.
- [#3196433](https://www.drupal.org/project/drupal/issues/3196433) Исправлена ссылка ведущая на документацию форматов времени PHP.net.
- [#3197135](https://www.drupal.org/project/drupal/issues/3197135) Исправлена ошибка в примере `hook_validation_constraint_alter()`.
- [#2623718](https://www.drupal.org/project/drupal/issues/2623718) Исправлен код не соответствующий стандарту `Drupal.Commenting.HookComment`.
- [#3196392](https://www.drupal.org/project/drupal/issues/3196392) В `MAINTAINERS.txt` добавлено описание об инициативе «Topic» (Help Topics).
- [#3196430](https://www.drupal.org/project/drupal/issues/3196430) Удалены стили для `::selection` элемента `#drupal-off-canvas`.
- [#2533254](https://www.drupal.org/project/drupal/issues/2533254) `LanguageInterface` добавлен в группу документаций `i18n`.
- [#3199582](https://www.drupal.org/project/drupal/issues/3199582) Исправлен пример реализации `hook_field_storage_config_update_forbid()`.

## Ссылки

- [Drupal 9.1.5](https://www.drupal.org/project/drupal/releases/9.1.5) (англ.), drupal.org, 4 марта 2021 г.
