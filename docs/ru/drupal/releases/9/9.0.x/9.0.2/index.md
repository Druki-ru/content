---
title: 'Drupal 9.0.2'
slug: wiki/drupal/releases/9.0.2
core: 9
metatags:
  title: 'Drupal 9.0.2: Список изменений'
  description: 'Список изменений Drupal 9.0.2.'
authors:
  - Niklan
category:
  area: 'Drupal 9.0.x'
  title: Drupal 9.0.2
  order: 2
---

**Дата релиза**: 9 июля 2020

<Aside>

Данный релиз также содержит изменения внесенные в [Drupal 8.9.2](../../../8/8.9.x/8.9.2/index.md).

</Aside>

## Claro

- [#3023311](https://www.drupal.org/project/drupal/issues/3023311) Оформление модального окна приведено в соответствии с Drupal Design System.

## Content Moderation

- [#3020387](https://www.drupal.org/project/drupal/issues/3020387) Исправлена неполадка, из-за которой состояние применялось ко всем переводам материала, а не конкретному.

## JavaScript

- [#3016427](https://www.drupal.org/project/drupal/issues/3016427) Улучшено определение часового пояса по умолчанию при установке Drupal.

## Link

- [#3151047](https://www.drupal.org/project/drupal/issues/3151047) Добавлен новый тест `LinkItemUrlValidationTest` для покрытия различных вариантов ввода ссылок.

## Theme System

- [#3146567](https://www.drupal.org/project/drupal/issues/3146567) Исправлено неверное название (`base_theme`) параметра в исключении. 

## Тестирование

- [#3143604](https://www.drupal.org/project/drupal/issues/3143604) Исправлена неполадка, из-за которой PHPStan не мог найти файлы.
- [#3135077](https://www.drupal.org/project/drupal/issues/3135077) Удалено использование `AssertLegacyTrait::pass` из трейтов.
- [#3144331](https://www.drupal.org/project/drupal/issues/3144331) Обновлён комментарий для метода `Drupal\Tests\RandomGeneratorTrait::randomStringValidate`, который содержал упоминание несуществующего метода `Drupal\simpletest\WebTestBase::assertLink`.
- [#3153722](https://www.drupal.org/project/drupal/issues/3153722) Свойство `$modules` в `DuplicateContextualLinksTest`, `NoMultilingualReviewPageTest` и `MenuActiveTrail403Test` является приватным.

## Прочие изменения

- [#3143173](https://www.drupal.org/project/drupal/issues/3143173) `ProxyBuilder` теперь совместим с Symfony 5.
- [#3135305](https://www.drupal.org/project/drupal/issues/3135305) Удалён слой совместимости с Symfony 4.1 из `EmailConstraint`.
- [#3143482](https://www.drupal.org/project/drupal/issues/3143482) Ссылки в `README.txt` файле обновлены на алисы, которые ведут на актуальную информацию по текущей актуальной версии Drupal.

## Ссылки

- [Drupal 9.0.2](https://www.drupal.org/project/drupal/releases/9.0.2) (англ.), drupal.org, 9 июля 2020