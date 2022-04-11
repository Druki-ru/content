---
title: 'Drupal 10.0.0' 
slug: wiki/drupal/releases/10.0.0 
core: 10
metatags:
  title: 'Drupal 10.0.0: Список изменений'
  description: 'Список изменений Drupal 10.0.0.'
authors:
  - Niklan
  - arraksis
---

**Дата релиза**: июнь 2022\
**Активная поддержка до**: ноябрь 2022\
**Поддержка безопасности до**: июнь 2023

<Aside type="warning">

Данная версия находится в разработке. Список изменений может меняться и дополняться, даты могут смещаться.

</Aside>

На данной странице представлен список изменений вошедший в версию Drupal 10.0.0. Данная версия также включает изменения вошедшие в [Drupal 9.4.0](../../../9/9.4.x/9.4.0/index.md).

## Введение

**10.0.0** — новая [мажорная версия](../../../../../semver/index.md) [Drupal](../../../../9/index.md). Это первый релиз для [Drupal 10](../../../../10/index.md).

Данная версия является «переходной» с [Drupal 9](../../../../9/index.md) и содержит минимальное количество нововведений. Все нововведения были добавлены в [Drupal 9.4.0](../../../9/9.4.x/9.4.0/index.md), а Drupal 10 будет получать новые возможности начиная с Drupal 10.1.0.

<Aside type="tip" header="Обновление Drupal 9 до Drupal 10">

Если ваш сайт работает на [Drupal 9](../../../../9/index.md), то вам может оказаться полезным [руководство по обновлению Drupal 9 до Drupal 10](../../../../10/upgrade-drupal-9-to-drupal-10/index.md).

</Aside>

## Минимальная версия PHP — 8.1

- [#3252088](https://www.drupal.org/node/3252088), [#3255271](https://www.drupal.org/node/3255271), [#3255350](https://www.drupal.org/node/3255350), [#3261357](https://www.drupal.org/node/3261357), [#3264819](https://www.drupal.org/node/3264819) 

Минимальная и рекомендуемая версия PHP для Drupal 10 — 8.1. Drupal 10 не будет работать на PHP 8.0, PHP 7 и более ранних версиях.

Весь код, который был связан с PHP 7, включая настройки для PHP в `.htaccess` файле и различные упоминания — удалены из кодовой базы.

## Компоненты Symfony обновлены до 6 версии

- [#3252757](https://www.drupal.org/node/3252757) 

Drupal 10 будет использовать компоненты Symfony 6.0. Единственный пакет, который временно останется на версии 5.4 — `symfony/console`, в связи с тем, что у 6.0 версии есть проблемы с [Composer](../../../../../composer/index.md), которые были исправлены в Comopser 2.3.

## Прекращена поддержка Internet Explorer

- [#3253148](https://www.drupal.org/node/3253148) Браузер Internet Explorer удалён из browserlist. Все ассеты были пересобраны без его поддержки.

## Аргумент $context больше не передаётся hook_entity_view_mode_alter()

- [#3194165](https://www.drupal.org/node/3194165) 

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

- [#3252257](https://www.drupal.org/node/3252257) 

Начиная с Drupal 10 поддерживается только PHPUnit 9. Весь код, относящийся к PHPUnit 8 был удалён.

## Для совместимости с PSR-17, `laminas/diactoros` заменён `guzzlehttp/psr7`

- [#3255243](https://www.drupal.org/node/3255243) 

Использование библиотеки Laminas Diactoros заменено на Guzzle PSR 7. Если вы используете фабрики запросов с PSR-7 или PSR-17, вам не нужно вносить никаких изменений. Объекты запросов будут соответствовать обеим PSR стандартам.

Если же вы используете классы `Laminas\Diactoros` напрямую, вам необходимо добавить данную библиотеку в свой `composer.json` как зависимость, либо обновить использовать классы `GuzzleHttp\Psr7`.

## Twig обновлён до 3 версии

- [#3094493](https://www.drupal.org/node/3094493)

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

- [#3256687](https://www.drupal.org/node/3256687) 

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

- [#3256687](https://www.drupal.org/node/3256687)

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

- `drupal_load_updates()`
- `drupal_install_profile_distribution_name()`
- `drupal_install_profile_distribution_version()`
- `drupal_detect_database_types()`
- `_drupal_rewrite_settings_is_simple()`
- `_drupal_rewrite_settings_is_array_index()`
- `_drupal_rewrite_settings_global()`
- `_drupal_rewrite_settings_dump()`
- `_drupal_rewrite_settings_dump_one()`
- `drupal_verify_profile()`
- `drupal_install_system()`
- `drupal_verify_install_file()`
- `drupal_install_mkdir()`
- `drupal_install_fix_file()`
- `install_goto()`
- `drupal_current_script_url()`
- `drupal_requirements_url()`
- `drupal_check_profile()`
- `drupal_requirements_severity()`
- `drupal_check_module()`
- `install_profile_info()`
- `db_installer_object()`

Список **констант** объявленных в файле:

- `REQUIREMENT_INFO`
- `REQUIREMENT_OK`
- `REQUIREMENT_WARNING`
- `REQUIREMENT_ERROR`
- `FILE_EXIST`
- `FILE_READABLE`
- `FILE_WRITABLE`
- `FILE_EXECUTABLE`
- `FILE_NOT_EXIST`
- `FILE_NOT_READABLE`
- `FILE_NOT_WRITABLE`
- `FILE_NOT_EXECUTABLE`

## Обновлён `z-index` для элементов модуля Tour

- [#3250357](https://www.drupal.org/node/3250357)

Внесены изменения в значения `z-index` следующих селекторов:

* `.shepherd-element` увеличен со 101 до 110.
* `.shepherd-modal-overlay-container` увеличен со 100 до 105.

## В Drupal ядре начали использовать статически анализатор кода PHPStan с уровнем проверки 0

- [#3178534](https://www.drupal.org/node/3178534)

Статический анализатор код [PHPStan](https://phpstan.org/) добавлен в ядро. На данный момент проверка будет проводиться на самом минимальном уровне (`0`). PHPStan позволяет находить общие неточности и ошибки в коде, без необходимости писать тесты. Использование PHPStan добавлено в `commit-code-check.sh` скрипт, который используется при тестировании Drupal.

Основная конфигурация расположена по пути `core/phpstan.neon.dist`, а файл с исключениями для игнорирования по пути `core/phpstan-baseline.neon`.

Применение:

```shell
# Запуск PHPStan для анализа Drupal ядра.
$ phpstan analyze --configuration=core/phpstan.neon.dist
# Сгенерировать новый файл с исключениями для игнорирования.
$ phpstan analyze --configuration=core/phpstan.neon.dist --generate-baseline ./core/phpstan-baseline.neon
```

<Aside type="tip">

Данные конфигурации, на данный момент, делают базовые проверки без учёта специфики и возможностей Drupal и игнорируют множество ошибок и предупреждений.

Если вы заинтересованы в использовании PHPStan на своём проекте, рекомендуется обратить внимание на проект [mglaman/phpstan-drupal](https://github.com/mglaman/phpstan-drupal), который предоставляет более глубокую интеграцию PHPStan в Drupal. Это позволит вам найти больше ошибок и не пропускать мелочь, которая игнорируется ядром на данный момент.

</Aside>

## Удалены обновления добавленные до Drupal 9.3.0

- [#3261486](https://www.drupal.org/node/3261486)

Обновления добавленные до [Drupal 9.3.0](../../../9/9.3.x/9.3.0/index.md) были удалены из [Drupal 10](../../../../10/index.md). Это означает что минимальная версия для обновления на Drupal 10 — 9.3.0+.

## `Drupal\Core\Http\RequestStack` объявлен устаревшим

- [#3265356](https://www.drupal.org/node/3265356)

Класс `Drupal\Core\Http\RequestStack`, предоставляемый Drupal в качестве слоя обратной совместимости с Symfony 4 `RequestStack`, объявлен устаревшим.

Drupal 10 перешел на Symfony 6, следовательно, данная прослойка больше не нужна и будет удалена в Drupal 11. Замена не предоставляется, вы можете использовать класс из Symfony напрямую.

## CKEditor 5

- [#3261585](https://www.drupal.org/node/3261585) Удалены предупреждения для Internet Explorer 11, так как Drupal 10 больше его не поддерживает.

## Composer

- [#3252010](https://www.drupal.org/node/3252010) Версия PHPUnit зафиксирована на релизе 9.5.
- [#3253093](https://www.drupal.org/node/3253093) Удалена зависимость `symfony-cmf/routing`.
- [#3220220](https://www.drupal.org/node/3220220) Зависимость `guzzlehttp/psr7` обновлена до версии 2.1.0.
- [#3253092](https://www.drupal.org/node/3253092) Удалена зависимость `doctrine/reflection`.
- [#3128982](https://www.drupal.org/node/3128982) Зависимость `asm89/stack-cors` обновлена до `^2.0`.
- [#3197482](https://www.drupal.org/node/3197482) Drupal ядро теперь использует компоненты Symfony `^5.4` вместо `^4.4`. 
- [#3255353](https://www.drupal.org/node/3255353) Зависимости ядра обновлены 17.01.22.
- [#3210486](https://www.drupal.org/node/3210486) Удалена зависимость `typo3/phar-stream-wrapper`.
- [#3258902](https://www.drupal.org/node/3258902) Удалена зависимость `stackphp/builder`, а его класс `Stack\StackedHttpKernel` добавлен в ядро как `
  Drupal\Core\DependencyInjection\Compiler\StackedKernelPass`.
- [#3254149](https://www.drupal.org/node/3254149) Из `composer.json` удалена настройка `config.autoloader-suffix`.
- [#3261743](https://www.drupal.org/node/3261743) Из Composer команды `drupal-phpunit-upgrade` удалена установка `phpspec/prophecy-phpunit`.
- [#3265094](https://www.drupal.org/node/3265094) Зависимости для разработки теперь имеют минимально требумые версии.
- [#3266017](https://www.drupal.org/node/3266017) Зависимость `psr/log` обновлена до 2 версии.
- [#3253059](https://www.drupal.org/node/3253059) Зависимость `composer/installers` обновлена до 2 версии.
- [#3270882](https://www.drupal.org/node/3270882) Зависимость `guzzlehttp/psr7` теперь явно прописана в `composer.json`. Ранее она была неявной зависимостью.
- [#3264918](https://www.drupal.org/node/3264918) Зависимость `symfony/console` обновлена до 6 версии.
- [#3264903](https://www.drupal.org/node/3264903) Зависимость `friends-of-behat/mink` заменена на `behat/mink`.

## Database System

- [#3210310](https://www.drupal.org/node/3210310) Удалён код объявленный устаревшим в Drupal 9.
- [#3203193](https://www.drupal.org/node/3203193) Установка Drupal 10 теперь прекращается, если БД не поддерживает тип данных JSON.

## Entity System

- [#3244802](https://www.drupal.org/node/3244802) Удалён слой [обратной совместимости](../../../../../backward-compatibility/index.md) и устаревший код из системы сущностей.

## Extensions System

- [#3077703](https://www.drupal.org/node/3077703) Удалены проверки используемые перед обновлением [Drupal 8.7.7](../../../8/8.7.x/8.7.7/index.md). Данные проверки являлись слоем обратной совместим для свойств `core` и `core_version_requirement`.

## JavaScript

- [#3268228](https://www.drupal.org/node/3268228) Удалена библиотека jQuery Joyride. Она была заменена на Popper.js добавленной в [Drupal 8.8.0](../../../8/8.8.x/8.8.0/index.md).

## Migrate

- [#2966859](https://www.drupal.org/node/2966859) Удалён устаревший модуль `migrate_drupal_multilingual`.

## Olivero

- [#3255119](https://www.drupal.org/node/3255119) Для переменных, используемых для сеток, теперь используются CSS-переменные.
- [#3255180](https://www.drupal.org/node/3255180) Внесены улучшения в стили для Views-сеток. Теперь они используют CSS-переменные.
- [#3217924](https://www.drupal.org/node/3217924) Синие и серые цвета конвертированы из HEX в HSL.
- [#3257583](https://www.drupal.org/node/3257583) Стили для вкладок были улучшены с использованием CSS-переменных.
- [#3217926](https://www.drupal.org/node/3217926) Название CSS переменных с цветами приведено к единому стилю, `blue` цвет заменён на `primary`.
- [#3262139](https://www.drupal.org/node/3262139) Из JavaScript файлов удалён слой обратной совместимости который поддерживал событие нажатия `Esc` для IE11.

## PosgreSQL DB driver

- [#3214922](https://www.drupal.org/node/3214922) PostgreSQL драйвер теперь проверяет наличие расширения `pg_tgrm` и выдаёт ошибку если он отсутствует.

## RDF

- [#3176468 ](https://www.drupal.org/node/3176468 ) Удалена поддержка Easy RDF 0.9. Поддержка Easy RDF 1.0 продолжит работать без изменений.

## Symfony 6

- [#3232063](https://www.drupal.org/node/3232063) Методу `Drupal\serialization\Normalizer\NormalizerBase::supportsNormalization()` и его переопределениям добавлен тайпхинт `bool`.
- [#3236243](https://www.drupal.org/node/3236243) Методам, переопределяющие `Symfony\Component\HttpKernel\Controller\ControllerResolverInterface::getController()`, добавлен тайпхинт `callable|FALSE`.
- [#3236563](https://www.drupal.org/node/3236563) Методам, переопределяющие `Symfony\Component\HttpFoundation\Response::sendContent()`, добавлен тайпхинт `static`.
- [#3233456](https://www.drupal.org/node/3233456) Методам, переопределяющие `Symfony\Component\Console\Command\Command::execute()`, добавлен тайпхинт `int`.
- [#3232892](https://www.drupal.org/node/3232892) Методам, переопределяющие `Drupal\Core\Routing\RequestContext::fromRequest()`, добавлен тайпхинт `static`.
- [#3232832](https://www.drupal.org/node/3232832) Следующим методам добавлены тайпхинты: `Drupal\serialization\Encoder\JsonEncoder::supportsEncoding()` и `::
  supportsDecoding()`, а также для `Drupal\serialization\Encoder\XmlEncoder::supportsEncoding()`, `::encode()`, `::decode()` и `::supportsDecoding()`.
- [#3232113](https://www.drupal.org/node/3232113) Методам, переопределяющие `Drupal\Component\HttpFoundation\SecuredRedirectResponse::setTargetUrl()`, добавлен тайпхинт `static`.
- [#3232092](https://www.drupal.org/node/3232092) Методам, переопределяющие `Drupal\Component\DependencyInjection\Dumper\OptimizedPhpArrayDumper::dump()`, добавлен тайпхинт `string|array`.
- [#3232080](https://www.drupal.org/node/3232080) Методам, переопределяющие `Normalizer::denormalize()`, добавлен тайпхинт `mixed`.
- [#3233474](https://www.drupal.org/node/3233474) Методам, переопределяющие `Symfony\Component\Validator\Validator\ContextualValidatorInterface::atPath()`, `::getViolations()`, `::validateProperty()` и `::validatePropertyValue()`, добавлены тайпхинты.
- [#3238485](https://www.drupal.org/node/3238485) Методам, переопределяющие методы `Drupal\Component\DependencyInjection\Container`, добавлены тайпхинты.
- [#3233479](https://www.drupal.org/node/3233479) Методы, переопределяющие `Symfony\Component\Validator\Violation\ConstraintViolationBuilderInterface::atPath()`, `::setParameter()`, `::setParameters()`, `::setTranslationDomain()`, `::setInvalidValue()`, `::setPlural()`, `::setCode()` и `::setCause()` добавлены тайпхинты.
- [#3254331](https://www.drupal.org/node/3254331) Методам, переопределяющие `Normalizer::normalize()`, добавлен тайпхинт `array|string|int|float|bool|\ArrayObject|null`.
- [#3232097](https://www.drupal.org/node/3232097) Методам, переопределяющие `Symfony\Component\EventDispatcher\EventSubscriberInterface::getSubscribedEvents()`, добавлен тайпхинт `array`.
- [#3233031](https://www.drupal.org/node/3233031) Методам, переопределяющие `Symfony\Component\Routing\RequestContextAwareInterface::getContext()`, добавлен тайпхинт `RequestContext`.
- [#3254250](https://www.drupal.org/node/3254250) Внесены улучшения в код, который ожидает что метод `Symfony\Component\HttpFoundation\InputBag::get()` может вернуть значение отличное от строки.
- [#3231688](https://www.drupal.org/node/3231688) Методам, реализующих `ExecutionContextInterface::getViolations()`, `::getValidator()`, `::getRoot()`, `::getValue()`, `::isConstraintValidated()`, `::isGroupValidated()` и `::isObjectInitialized()` добавлены тайпхинты.
- [#3259028](https://www.drupal.org/node/3259028) Методам, реализующих `Symfony\Component\HttpKernel\Controller\ArgumentValueResolverInterface::implementations()` добавлены тайпхинты.
- [#3259026](https://www.drupal.org/node/3259026) Методам, переопределяющие методы Symfony Console добавлены тайпхинты.
- [#3209723](https://www.drupal.org/node/3209723) Использование `HttpKernelInterface::MASTER_REQUEST` заменено на `
  HttpKernelInterface::MAIN_REQUEST`.
- [#3162981](https://www.drupal.org/node/3162981) Где возможно, использование `ParameterBag` заменено на `InputBag`.
- [#3259675](https://www.drupal.org/node/3259675) Внесены улучшения в `LazyRouteCollectionTest` для совместимости с тайпхинтом для `::all()` метода.
- [#3259169](https://www.drupal.org/node/3259169) `ControllerResolver` больше не расширяет `BaseControllerResolver`.
- [#3259674](https://www.drupal.org/node/3259674) Методу `Drupal\Core\Routing\Router::matchCollection()` добавлен тайпхинт.
- [#3262190](https://www.drupal.org/node/3262190) Для различных классов добавлены тайпхинты возвращаемых данных.

## System

- [#3264120](https://www.drupal.org/node/3264120) Обновлена фикстура с базой данных Drupal 9.3.0 отражающая удаление модуля Aggregator и изменения в драйвера баз данных Posgres/SQLite.

## Тестирование

- [#3182103](https://www.drupal.org/node/3182103) Различные методы PHPUnit, которые переопределяют базовые тестовые классы ядра приведены в соответствие с текущими сигнатурами оригинальных методов.
- [#3222004](https://www.drupal.org/node/3222004) Обновлена схема в файле `phpunit.xml.dist`.
- [#3227265](https://www.drupal.org/node/3227265) Удалены устаревшие трейты для сравнений.
- [#3259158](https://www.drupal.org/node/3259158) Исправлен некорректный путь до `simpletest` директории в тесте `ClassWriter`.
- [#3261262](https://www.drupal.org/node/3261262) Из `PhpUnitWarnings` удалены сообщения об устаревшем коде связанным PHPUnit 8-.
- [#3262227](https://www.drupal.org/node/3262227) В список для игнорирования сообщений об устаревшем коде `DeprecationListenerTrait` добавлены исключения связанные с Symfony 6.
- [#3262183](https://www.drupal.org/node/3262183) Удалён устаревший `DrupalKernelLegacyTest`.
- [#3254726](https://www.drupal.org/node/3254726) Удалена поддержка SimpleTest тестов.
- [#3254723](https://www.drupal.org/node/3254723) Удалён слушатель `SimpletestUiPrinter`.
- [#3264764](https://www.drupal.org/node/3264764) Исправлена неполадка, из-за которой тест `PhpUnitCliTest::testFunctionalTestDebugHtmlOutput()` проваливался при пустом значении `BROWSERTEST_OUTPUT_DIRECTORY`.
- [#3211131](https://www.drupal.org/node/3211131) Внесены улучшения в `DrupalStandardsListenerTrait`.

## Прочие изменения

- [#3251936](https://www.drupal.org/node/3251936) (отменено) DrupalCI теперь поддерживает Drupal 10.
- [#3104353](https://www.drupal.org/node/3104353) Guzzle обновлён до 7 версии.
- [#3197729](https://www.drupal.org/node/3197729) Удалён слой обратной совместимости для `Definition::setDeprecated()`.
- [#3259158](https://www.drupal.org/node/3259158) Исправлена неполадка в `commit-code-check.sh`, которая приводила к некорректным результатам PHPStan если в проверяемом коммите был удалён файл.
- [#3260243](https://www.drupal.org/node/3260243) Внесены улучшения в `ConfigEntityResourceTestBase`.
- [#3214211](https://www.drupal.org/node/3214211) Drupal больше не добавляет заголовок `X-UA-Compatible` в ответы из-за прекращения поддержки Internet Explorer.
- [#3260766](https://www.drupal.org/node/3260766) Удалён устаревший файл `includes/file.inc`.
- [#3260805](https://www.drupal.org/node/3260805) Удалён устаревший код из `core/lib/Drupal/Core/Routing`.
- [#3259024](https://www.drupal.org/node/3259024) Удалены устаревшие сервисы `app.root` и `site.path`.
- [#3260778](https://www.drupal.org/node/3260778) Удалён устаревший код из `includes/bootstrap.inc`.
- [#3260801](https://www.drupal.org/node/3260801) Удалён устаревший код из `core/lib/Drupal/Component/Utility`.
- [#3260806](https://www.drupal.org/node/3260806) Удалён устаревший код из `core/lib/Drupal/Core/Config`.
- [#3260780](https://www.drupal.org/node/3260780) Удалён устаревший код из `includes/common.inc`.
- [#3210931](https://www.drupal.org/node/3210931) Удалён устаревший код из `includes/update.inc`.
- [#3258918](https://www.drupal.org/node/3258918) Удалён устаревший сервис `cache.null`.
- [#3261539](https://www.drupal.org/node/3261539) В конфигурацию PHPStan добавлено игнорирование `*.php.gz` файлов.
- [#3261539](https://www.drupal.org/node/3261539) В конфигурацию PHPStan добавлено игнорирование `lib/Drupal/Component/Transliteration/data/*.php` файлов, содержащих данные для транслитерации.
- [#3259110](https://www.drupal.org/node/3259110) Исправлены проблемы с использованием необъявленных переменных.
- [#3258435](https://www.drupal.org/node/3258435) Удалены устаревшие сервисы `feed.reader.*` и `feed.writer.*`.
- [#3261264](https://www.drupal.org/node/3261264) Удалён устаревший код из `Drupal\Core\Cache\DatabaseCacheTagsChecksum`.
- [#3261250](https://www.drupal.org/node/3261250) Удалён устаревший `Drupal\update\ModuleVersion`.
- [#3261253](https://www.drupal.org/node/3261253) Удалён устаревший плагин-обработчик `d6_url_alias_language`.
- [#3261265](https://www.drupal.org/node/3261265) Удалён код для устаревшего `MimeTypeGuesser`.
- [#3260765](https://www.drupal.org/node/3260765) Удалён устаревший код из кода связанного с навигацией (`menu`).
- [#3260615](https://www.drupal.org/node/3260615) Удалён устаревший файл `includes/schema.inc`.
- [#3097889](https://www.drupal.org/node/3097889) Удалены устаревшие функции из `includes/theme.inc`.
- [#3260781](https://www.drupal.org/node/3260781) Удалены устаревшие функции из `includes/module.inc`.
- [#3262500](https://www.drupal.org/node/3262500) Удалена устаревшая функция `drupal_find_theme_functions()`.
- [#3261252](https://www.drupal.org/node/3261252) Удалены устаревшие функции из `system.module`.
- [#3261243](https://www.drupal.org/node/3261243) Удалены устаревшие функции модуля `comment`.
- [#3124382](https://www.drupal.org/node/3124382) Удалена поддержка префиксов конкретных таблиц. Поддержка префиксов для всех таблиц не удалена.
- [#3261241](https://www.drupal.org/node/3261241) Удалены устаревшие функции `field` модуля.
- [#3254726](https://www.drupal.org/node/3254726) Сгенерирован обновлённый `phpstan-baseline.neon` на 08.02.2022. 
- [#3262931](https://www.drupal.org/node/3262931) Удалена устаревшая функция `drupal_required_modules()`.
- [#3261251](https://www.drupal.org/node/3261251) Удалены устаревшие функции модуля `node`.
- [#3261239](https://www.drupal.org/node/3261239) Удалены устаревшие функции модуля `search`.
- [#3262853](https://www.drupal.org/node/3262853) Удалена поддержка передачи сервисов в качестве параметров контейнера.
- [#3262937](https://www.drupal.org/node/3262937) Добавлены исключения в коде для PHPStan, в тех местах, где код намеренно проверяет ситуацию, где вызывается ошибка.
- [#3263391](https://www.drupal.org/node/3263391) Удалены устаревшие функции модуля `book`.
- [#3264073](https://www.drupal.org/node/3264073) Удалён устаревший код в пространстве имён `Drupal\Core\Condition`.
- [#3264072](https://www.drupal.org/node/3264072) Удалён устаревший код в пространстве имён `Drupal\Core\Archiver`.
- [#3264062](https://www.drupal.org/node/3264062) Удалён устаревший код в модуле `editor`.
- [#3263395](https://www.drupal.org/node/3263395) Удалён устаревший код в пространстве имён `Drupal\Core\Asset`.
- [#3264057](https://www.drupal.org/node/3264057) Удалены устаревшие функции модуля `media`.
- [#3264067](https://www.drupal.org/node/3264067) Удалён устаревший код в пространстве имён `Drupal\Core\Session`.
- [#3264061](https://www.drupal.org/node/3264061) Удалён устаревший код в модуле `image`.
- [#2940025](https://www.drupal.org/node/2940025) Удалён устаревший код в модуле `file`.
- [#3188858](https://www.drupal.org/node/3188858) Удалён устаревший модуль `entity_reference`. Весь функционал попал в ядро и одноимённый тип полей не удалён.
- [#2966859](https://www.drupal.org/node/2966859) Удалён устаревший модуль `drupal/migrate_drupal_multilingual` из `composer.json` файла.
- [#3261244](https://www.drupal.org/node/3261244) Удалены устаревшие функции модуля `layout_builder`.
- [#3115308](https://www.drupal.org/node/3115308) Удалена устаревшая [проверка доступа к маршруту](../../../../10/services/tagged/access-check/index.md) `_access_rest_csrf`.
- [#3265120](https://www.drupal.org/node/3265120) Удалён класс для обратной совместимости с Symfony 4 — `DeprecatedServicePass`.
- [#3261240](https://www.drupal.org/node/3261240) Удалены устаревшие функции модуля `taxonomy`.
- [#3261004](https://www.drupal.org/node/3261004) Удалены устаревший код в модуле `migrate`.
- [#3265605](https://www.drupal.org/node/3265605) Удалёна обратная совместимость для аргументов конструкторов `ConfigInstaller` и `ConfigManager`.
- [#3264059](https://www.drupal.org/node/3264059) Удалены устаревшие функции модуля `jsonapi`.
- [#3049857](https://www.drupal.org/node/3049857) Удалён устаревший модуль HAL.
- [#3266525](https://www.drupal.org/node/3266525) Исправлена минимально поддерживаемая версия `Drupal::MINIMUM_SUPPORTED_PHP` с 8.0.2 до 8.1.0.
- [#3268550](https://www.drupal.org/node/3268550) Удалена устаревшая библиотека jQuery Once.
- [#3267791](https://www.drupal.org/node/3267791) Удалён устаревший шим `jquery.cookie`.
- [#3261248](https://www.drupal.org/node/3261248) Удалены устаревшие функции модуля `user`.
- [#3264120](https://www.drupal.org/node/3264120) Удалён устаревший модуль `aggregator`.
- [#3274016](https://www.drupal.org/node/3274016) Удалён устаревший метод `AssertMailTrait::verboseEmail()`.
- [#3260928](https://www.drupal.org/node/3260928) Добавлено больше правил в `phpstan-baseline.neon`.