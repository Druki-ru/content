---
title: 'Drupal 8.9.8'
slug: drupal/releases/8.9.8
core: 8
metatags:
  title: 'Drupal 8.9.8: Список изменений'
  description: 'Список изменений Drupal 8.9.8.'
---

**Дата релиза**: 5 ноября 2020

## DateTime

- [#3175395](https://www.drupal.org/project/drupal/issues/3175395) Удалено неиспользуемое свойство `#html` из `DateTimeFormatterBase::buildDateWithIsoAttribute`.

## Language System

- [#3179318](https://www.drupal.org/project/drupal/issues/3179318) Для загрузки переводов теперь принудительно используется HTTPS протокол.

## Libraries

- [#2716115](https://www.drupal.org/project/drupal/issues/2716115) Добавлена поддержка `attributes` для CSS библиотек.

## Typed Data System

- [#3177765](https://www.drupal.org/project/drupal/issues/3177765) Исправлена документация о возвращаемом типе для `ListInterface::first()`. 

## Update

- [#3132426](https://www.drupal.org/project/drupal/issues/3132426) Теперь при запросе обновлений, информация в кеше, полученная ранее, очищается принудительно.

## User

- [#3176036](https://www.drupal.org/project/drupal/issues/3176036) Исправлены грамматические ошибки в `ProfileFieldCheckRequirementsTest`.

## Тестирование

- [#3175112](https://www.drupal.org/project/drupal/issues/3175112) Исправлена ошибка в `hold_test` модуле, который создавал файлы в неположенном месте что приводило к различным случайным ошибкам.
- [#3178273](https://www.drupal.org/project/drupal/issues/3178273) Удалён мёртвый метод `BasicAuthTestTrait::basicAuthPostForm()`.

## Прочие изменения

- [#3177477](https://www.drupal.org/project/drupal/issues/3177477) Pamela Barone ([@pameeela](https://www.drupal.org/u/pameeela)) теперь полноценный коммитер-помощник.
- [#3157963](https://www.drupal.org/project/drupal/issues/3157963) Исправлены некорректные референсы в документации к `FormStateInterface::getResponse()`.
- [#3040274](https://www.drupal.org/project/drupal/issues/3040274) Исправлены грамматические ошибки, написание и стиль кода и комментариев для `FormBuilder::prepareForm()`.
- [#2937844](https://www.drupal.org/project/drupal/issues/2937844) Внесены исправления для соответствия стандарту кода `Squiz.PHP.NonExecutableCode`.
- [#3178039](https://www.drupal.org/project/drupal/issues/3178039) Исправлены опечатки «is has» в комментариях.
- [#3173004](https://www.drupal.org/project/drupal/issues/3173004) Внесены исправления в документацию для `FieldItemInterface::view()`.
- [#3014969](https://www.drupal.org/project/drupal/issues/3014969) Улучшен синтаксис документации для `ContextProviderInterface`, что позволяет корректно парсить её.

## Ссылки

- [Drupal 8.9.8](https://www.drupal.org/project/drupal/releases/8.9.8) (англ.), drupal.org, 5 ноября 2020