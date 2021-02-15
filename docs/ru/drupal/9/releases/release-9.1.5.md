---
id: release-9.1.5
title: 'Drupal 9.1.5'
path: /9/releases/9.1.5
core: 9
metatags:
  title: 'Drupal 9.1.5: Список изменений'
  description: 'Список изменений Drupal 9.1.5.'
---

**Дата релиза**: 3 марта 2021

> [!WARNING]
> Данная версия находится в разработке, актуальная версия [Drupal 9.1.4](release-9.1.4.md).

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

## Configuration System

- [#3196050](https://www.drupal.org/project/drupal/issues/3196050) Исправлена документация для `StorageConfigBase::validateValue()`.

## Editor

- [#2857444](https://www.drupal.org/project/drupal/issues/2857444) Улучшено отслеживание файлов в полях отличных от `text`, `text_long` и `text_with_summary`. Теперь данные файлы отслеживаются для всех полей что расширяют `TextItemBase`.

## Entity System

- [#2693485](https://www.drupal.org/project/drupal/issues/2693485) Пункты добавления материалов (`/node/add`) теперь сортируются по метке, а не по машинному имени.

## Form System

- [#997826](https://www.drupal.org/project/drupal/issues/997826) Исправлена работа свойства `#state` для элемента формы `text_format`.

## Migration System

- [#2558857](https://www.drupal.org/project/drupal/issues/2558857) Оптимизирована очистка памяти после загрузки сущностей.
- [#3165944](https://www.drupal.org/project/drupal/issues/3165944) Миграция `d7_shortcut` больше не указывает в качестве зависимости миграцию `d7_menu_links`.
- [#3189476](https://www.drupal.org/project/drupal/issues/3189476) Миграция `node_translation_menu_links` теперь имеет опциональные зависимости `d6_menu` и `d7_menu`.
- [#3196177](https://www.drupal.org/project/drupal/issues/3196177) Добавлена документация для плагинов источников `VariableMultiRow`, `Variable`, `d6/VariableTranslation` и `d7/VariableTranslation`.
- [#3187415](https://www.drupal.org/project/drupal/issues/3187415) Миграции переводов для системных настроек теперь зависят от базовых миграций этих настроек.
- [#3176394](https://www.drupal.org/project/drupal/issues/3176394) Типы комментариев больше не мигрируют если в источнике отключен модуль `comment`.
- [#2954982](https://www.drupal.org/project/drupal/issues/2954982) Исправлена неполадка в `EntityContentBase::processStubRow()` приводящая к ошибкам в некоторых ситуациях.
- [#3178966](https://www.drupal.org/project/drupal/issues/3178966) Миграции Drupal 2 Drupal теперь будут выдавать ошибки при попытке мигрировать комментарии если на сайте-источнике они отключены. 

## Olivero

- [#3182711](https://www.drupal.org/project/drupal/issues/3182711) Блок "помощи" теперь располагается в регионе `content_above` вместо несуществующего `help`.

## Тестирование

- [#2571475](https://www.drupal.org/project/drupal/issues/2571475) `KernelTestBase` теперь могу производить внешние HTTP запросы.
- [#3187309](https://www.drupal.org/project/drupal/issues/3187309) Сравнения с использованием XPath для `<select>` и `<option>` заменены на современные методы.
- [#3189607](https://www.drupal.org/project/drupal/issues/3189607) Сравнения с использованием XPath для `<input type="checkbox">` заменены на современные методы.

## Прочие изменения

- [#3195951](https://www.drupal.org/project/drupal/issues/3195951) Из `MAINTAINERS.txt` удалён пустой раздел "Provisional membership".
- [#3195277](https://www.drupal.org/project/drupal/issues/3195277) Из `MAINTAINERS.txt` удалены упоминания Drupal 8.
- [#3196391](https://www.drupal.org/project/drupal/issues/3196391) Упоминание "Drupal Core" в `MAINTAINERS.txt` приведено к единому стилю.
- [#3196433](https://www.drupal.org/project/drupal/issues/3196433) Исправлена ссылка ведущая на документацию форматов времени PHP.net.
- [#3197135](https://www.drupal.org/project/drupal/issues/3197135) Исправлена ошибка в примере `hook_validation_constraint_alter()`.