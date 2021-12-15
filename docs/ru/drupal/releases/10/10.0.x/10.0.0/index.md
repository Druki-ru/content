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

## Минимальная версия PHP — 8.0

* [#3252088](https://www.drupal.org/node/3252088) 

Минимальная версия PHP для Drupal 10 — 8.0. Drupal 10 не будет работать на PHP 7 и более ранних версиях.

## Composer

* [#3252010](https://www.drupal.org/node/3252010) Версия PHPUnit зафиксирована на релизе 9.5.

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

## Тестирование

* [#3182103](https://www.drupal.org/node/3182103) Различные методы PHPUnit, которые переопределяют базовые тестовые классы ядра приведены в соответствие с текущими сигнатурами оригинальных методов.

## Прочие изменения

* [#3251936](https://www.drupal.org/node/3251936) (отменено) DrupalCI теперь поддерживает Drupal 10.
* [#3104353](https://www.drupal.org/node/3104353) Guzzle обновлён до 7 версии.