---
title: 'Drupal 10.0.0' 
slug: wiki/drupal/releases/10.0.0 
core: 10
metatags:
  title: 'Drupal 10.0.0: Список изменений'
  description: 'Список изменений Drupal 10.0.0.'
---

> [!WARNING]
> Данная версия находится в разработке. Список изменений может меняться и дополняться, даты могут смещаться.

**Дата релиза**: июнь 2022\
**Активная поддержка до**: ноябрь 2022\
**Поддержка безопасности до**: июнь 2023

> [!NOTE]
> Drupal 10 содержит изменения вошедшие в релиз [Drupal 9.4.0](../../../9/9.4.x/9.4.0/index.md).

## Минимальная версия PHP — 8.0.2

* [#3252088](https://www.drupal.org/node/3252088), [#3255271](https://www.drupal.org/node/3255271), [#3255350](https://www.drupal.org/node/3255350) 

Минимальная версия PHP для Drupal 10 — 8.0.2. Drupal 10 не будет работать на PHP 7 и более ранних версиях.

Весь код, который был связан с PHP 7, включая настройки для PHP в `.htaccess` файле и различные упоминания — удалены из кодовой базы.

## Прекращена поддержка Internet Explorer

* [#3253148](https://www.drupal.org/node/3253148) Браузер Internet Explorer удалён из browserlist. Все ассеты были пересобраны без его поддержки.

## Аргумент $context больше не передаётся hook_entity_view_mode_alter()

* [#3194165](https://www.drupal.org/node/3194165) 

Начиная с версии [Drupal 9.2.0](../../../9/9.2.x/9.2.0/index.md), `hook_entity_view_mode_alter()` стал получать пустой массив в качестве `$context` аргумента. Это было сделано для обратной совместимости.

Начиная с Drupal 10, обратная совместимость удалена и аргумент не передаётся в данный хук.

Ранее:

```php
function hook_entity_view_mode_alter(&$view_mode, Drupal\Core\Entity\EntityInterface $entity, $context) {
  if ($entity->getEntityTypeId() == 'node' && $view_mode == 'teaser') {
    $view_mode = 'my_custom_view_mode';
  }
}
```

После изменения:

```php
function hook_entity_view_mode_alter(&$view_mode, Drupal\Core\Entity\EntityInterface $entity) {
  if ($entity->getEntityTypeId() == 'node' && $view_mode == 'teaser') {
    $view_mode = 'my_custom_view_mode';
  }
}
```

## Прекращение поддержки PHPUnit 8

* [#3252257](https://www.drupal.org/node/3252257) 

Начиная с Drupal 10 поддерживается только PHPUnit 9. Весь код, относящийся к PHPUnit 8 был удалён.

## Для совместимости с PSR-17, `laminas/diactoros` заменён `guzzlehttp/psr7`

* [#3255243](https://www.drupal.org/node/3255243) 

Использование библиотеки Laminas Diactoros заменено на Guzzle PSR 7. Если вы используете фабрики запросов с PSR-7 или PSR-17, вам не нужно вносить никаких изменений. Объекты запросов будут соответствовать обеим PSR стандартам.

Если же вы используете классы `Laminas\Diactoros` напрямую, вам необходимо добавить данную библиотеку в свой `composer.json` как зависимость, либо обновить использовать классы `GuzzleHttp\Psr7`.

## Twig обновлён до 3 версии

* [#3094493](https://www.drupal.org/node/3094493)

Drupal 10 теперь использует Twig 3, вместо Twig 2.

Если вы всё еще не обновили ваши шаблоны на Drupal 9 до синтаксиса Twig 2, рекомендуется сделать это первым делом, так как это позволит обновиться до Twig 3.

Twig 3 имеет свои изменения, но их можно будет внести и на Drupal 10. Пара изменений, что ломают совместимость и требуют внимания перед обновлением до Drupal 10 представлены ниже.

Например, тег `spaceless` в Twig 2 объявлен устаревшим в пользу фильтра и удалён в Twig 3.

**Ранее:**

```twig
{% spaceless %}
  …
{% endspaceless %}
```

**Сейчас:**

```twig
{% apply spaceless %}
  …
{% endapply %}
```

Также, удалена поддержка логического оператора `if` внутри конструкции `for`:

**Ранее:**

```twig
{% for i in 1..depth-1 if depth > 1 %}
  …
{% endfor %}
```

**Сейчас:**

```twig
{% if depth > 1 %}
  {% for i in 1..depth-1 %}
    …
  {% endfor %}
{% endif %}
```

**Полезные ссылки:**

* [Twig 1 deprecated](https://twig.symfony.com/doc/1.x/deprecated.html) (англ.), twig.symfony.com
* [Twig 2 deprecated](https://twig.symfony.com/doc/2.x/deprecated.html) (англ.), twig.symfony.com
* [Twig 2 CHANGELOG](https://github.com/twigphp/Twig/blob/2.x/CHANGELOG) (англ.), github.com
* [Twig 3 CHANGELOG](https://github.com/twigphp/Twig/blob/3.x/CHANGELOG) (англ.), github.com
* [Preparing for use of Twig 2 in Drupal 9](https://www.drupal.org/docs/upgrading-drupal/how-to-prepare-your-drupal-7-or-8-site-for-drupal-9/preparing-for-use-of-twig) (
  англ.), drupal.org

## Функция `db_installer_object()` объявлена устаревшей

* [#3256687](https://www.drupal.org/node/3256687) 

Функция `db_installer_object()` объявлена устаревшей и будет удалено до релиза Drupal 11. Замены данной функции не предоставлено.

В Drupal 9, расположение объекта установщика могло находиться в различных местах, в зависимости от используемого драйвера баз данных. В Drupal 10 и новее, все драйверы баз данных являются модулями, а следовательно, имеют общий паттерн для пространства имён, что позволяет без проблем инициализировать объект установщика конкретной БД.

```php
// Ранее.
$installer = db_installer_object($driver, $namespace);

// Сейчас.
$installer_class = $namespace . "\\Install\\Tasks";
$installer = new $installer_class();
```

## Подключение файла `install.inc` удалено из `KernelTest` и `UpdatePathTest`

* [#3256687](https://www.drupal.org/node/3256687)

До Drupal 10, следующие два базовых класса подключали файл `install.inc`:

* `core/tests/Drupal/KernelTests/KernelTestBase.php`
* `core/tests/Drupal/FunctionalTests/Update/UpdatePathTestBase.php`

Это позволяло Kernel-тестам и `UpdatePath` тесту использовать процедурные функции из файла `install.inc`. Начиная с Drupal 10, эти функции больше недоступны.

Если в своих тестах вы используете функции из `install.inc`, вам необходимо самостоятельно запрашивать данный файл.

**Ранее:**

```php
class ExampleTest extends KernelTestBase {

  public function testFunction() {
    // Call procedure function from install.inc
    $databases = drupal_detect_database_types();
  }

}
```

**Сейчас:**

```php
class ExampleTest extends KernelTestBase {

  public function testFunction() {
    // Перед использованием процедурной функции, запрашиваем install.inc.
    // Убедитесь что вы указали корректный путь до файла.
    require_once '/includes/install.inc';

    // Вызываем функцию объявленную в install.inc.
    $databases = drupal_detect_database_types();
  }

}
```

Список **функций** объявленных в данном файле:

* `drupal_load_updates()`
* `drupal_install_profile_distribution_name()`
* `drupal_install_profile_distribution_version()`
* `drupal_detect_database_types()`
* `_drupal_rewrite_settings_is_simple()`
* `_drupal_rewrite_settings_is_array_index()`
* `_drupal_rewrite_settings_global()`
* `_drupal_rewrite_settings_dump()`
* `_drupal_rewrite_settings_dump_one()`
* `drupal_verify_profile()`
* `drupal_install_system()`
* `drupal_verify_install_file()`
* `drupal_install_mkdir()`
* `drupal_install_fix_file()`
* `install_goto()`
* `drupal_current_script_url()`
* `drupal_requirements_url()`
* `drupal_check_profile()`
* `drupal_requirements_severity()`
* `drupal_check_module()`
* `install_profile_info()`
* `db_installer_object()`

Список **констант** объявленных в файле:

* `REQUIREMENT_INFO`
* `REQUIREMENT_OK`
* `REQUIREMENT_WARNING`
* `REQUIREMENT_ERROR`
* `FILE_EXIST`
* `FILE_READABLE`
* `FILE_WRITABLE`
* `FILE_EXECUTABLE`
* `FILE_NOT_EXIST`
* `FILE_NOT_READABLE`
* `FILE_NOT_WRITABLE`
* `FILE_NOT_EXECUTABLE`

## Composer

* [#3252010](https://www.drupal.org/node/3252010) Версия PHPUnit зафиксирована на релизе 9.5.
* [#3253093](https://www.drupal.org/node/3253093) Удалена зависимость `symfony-cmf/routing`.
* [#3220220](https://www.drupal.org/node/3220220) Зависимость `guzzlehttp/psr7` обновлена до версии 2.1.0.
* [#3253092](https://www.drupal.org/node/3253092) Удалена зависимость `doctrine/reflection`.
* [#3128982](https://www.drupal.org/node/3128982) Зависимость `asm89/stack-cors` обновлена до `^2.0`.
* [#3197482](https://www.drupal.org/node/3197482) Drupal ядро теперь использует компоненты Symfony `^5.4` вместо `^4.4`. 

## Database System

* [#3210310](https://www.drupal.org/node/3210310) Удалён код объявленный устаревшим в Drupal 9.

## Olivero

* [#3255119](https://www.drupal.org/node/3255119) Для переменных, используемых для сеток, теперь используются CSS-переменные.
* [#3255180](https://www.drupal.org/node/3255180) Внесены улучшения в стили для Views-сеток. Теперь они используют CSS-переменные.

## Symfony 6

* [#3232063](https://www.drupal.org/node/3232063) Методу `Drupal\serialization\Normalizer\NormalizerBase::supportsNormalization()` и его переопределениям добавлен тайпхинт `bool`.
* [#3236243](https://www.drupal.org/node/3236243) Методам, переопределяющие `Symfony\Component\HttpKernel\Controller\ControllerResolverInterface::getController()`, добавлен тайпхинт `callable|FALSE`.
* [#3236563](https://www.drupal.org/node/3236563) Методам, переопределяющие `Symfony\Component\HttpFoundation\Response::sendContent()`, добавлен тайпхинт `static`.
* [#3233456](https://www.drupal.org/node/3233456) Методам, переопределяющие `Symfony\Component\Console\Command\Command::execute()`, добавлен тайпхинт `int`.
* [#3232892](https://www.drupal.org/node/3232892) Методам, переопределяющие `Drupal\Core\Routing\RequestContext::fromRequest()`, добавлен тайпхинт `static`.
* [#3232832](https://www.drupal.org/node/3232832) Следующим методам добавлены тайпхинты: `Drupal\serialization\Encoder\JsonEncoder::supportsEncoding()` и `::
  supportsDecoding()`, а также для `Drupal\serialization\Encoder\XmlEncoder::supportsEncoding()`, `::encode()`, `::decode()` и `::supportsDecoding()`.
* [#3232113](https://www.drupal.org/node/3232113) Методам, переопределяющие `Drupal\Component\HttpFoundation\SecuredRedirectResponse::setTargetUrl()`, добавлен тайпхинт `static`.
* [#3232092](https://www.drupal.org/node/3232092) Методам, переопределяющие `Drupal\Component\DependencyInjection\Dumper\OptimizedPhpArrayDumper::dump()`, добавлен тайпхинт `string|array`.
* [#3232080](https://www.drupal.org/node/3232080) Методам, переопределяющие `Normalizer::denormalize()`, добавлен тайпхинт `mixed`.
* [#3233474](https://www.drupal.org/node/3233474) Методам, переопределяющие `Symfony\Component\Validator\Validator\ContextualValidatorInterface::atPath()`, `::getViolations()`, `::validateProperty()` и `::validatePropertyValue()`, добавлены тайпхинты.
* [#3238485](https://www.drupal.org/node/3238485) Методам, переопределяющие методы `Drupal\Component\DependencyInjection\Container`, добавлены тайпхинты.
* [#3233479](https://www.drupal.org/node/3233479) Методы, переопределяющие `Symfony\Component\Validator\Violation\ConstraintViolationBuilderInterface::atPath()`, `::setParameter()`, `::setParameters()`, `::setTranslationDomain()`, `::setInvalidValue()`, `::setPlural()`, `::setCode()` и `::setCause()` добавлены тайпхинты.
* [#3254331](https://www.drupal.org/node/3254331) Методам, переопределяющие `Normalizer::normalize()`, добавлен тайпхинт `array|string|int|float|bool|\ArrayObject|null`.
* [#3232097](https://www.drupal.org/node/3232097) Методам, переопределяющие `Symfony\Component\EventDispatcher\EventSubscriberInterface::getSubscribedEvents()`, добавлен тайпхинт `array`.
* [#3233031](https://www.drupal.org/node/3233031) Методам, переопределяющие `Symfony\Component\Routing\RequestContextAwareInterface::getContext()`, добавлен тайпхинт `RequestContext`.
* [#3254250](https://www.drupal.org/node/3254250) Внесены улучшения в код, который ожидает что метод `Symfony\Component\HttpFoundation\InputBag::get()` может вернуть значение отличное от строки.
* [#3231688](https://www.drupal.org/node/3231688) Методам, реализующих `ExecutionContextInterface::getViolations()`, `::getValidator()`, `::getRoot()`, `::getValue()`, `::isConstraintValidated()`, `::isGroupValidated()` и `::isObjectInitialized()` добавлены тайпхинты.

## Тестирование

* [#3182103](https://www.drupal.org/node/3182103) Различные методы PHPUnit, которые переопределяют базовые тестовые классы ядра приведены в соответствие с текущими сигнатурами оригинальных методов.
* [#3222004](https://www.drupal.org/node/3222004) Обновлена схема в файле `phpunit.xml.dist`.

## Прочие изменения

* [#3251936](https://www.drupal.org/node/3251936) (отменено) DrupalCI теперь поддерживает Drupal 10.
* [#3104353](https://www.drupal.org/node/3104353) Guzzle обновлён до 7 версии.
