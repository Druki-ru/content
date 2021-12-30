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

## Composer

* [#3252010](https://www.drupal.org/node/3252010) Версия PHPUnit зафиксирована на релизе 9.5.
* [#3253093](https://www.drupal.org/node/3253093) Удалена зависимость `symfony-cmf/routing`.
* [#3220220](https://www.drupal.org/node/3220220) Зависимость `guzzlehttp/psr7` обновлена до версии 2.1.0.
* [#3253092](https://www.drupal.org/node/3253092) Удалена зависимость `doctrine/reflection`.

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

## Тестирование

* [#3182103](https://www.drupal.org/node/3182103) Различные методы PHPUnit, которые переопределяют базовые тестовые классы ядра приведены в соответствие с текущими сигнатурами оригинальных методов.
* [#3222004](https://www.drupal.org/node/3222004) Обновлена схема в файле `phpunit.xml.dist`.

## Прочие изменения

* [#3251936](https://www.drupal.org/node/3251936) (отменено) DrupalCI теперь поддерживает Drupal 10.
* [#3104353](https://www.drupal.org/node/3104353) Guzzle обновлён до 7 версии.