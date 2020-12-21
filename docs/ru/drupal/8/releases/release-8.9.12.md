---
id: release-8.9.12
title: 'Drupal 8.9.12'
path: /8/releases/8.9.12
core: 8
metatags:
  title: 'Drupal 8.9.12: Список изменений'
  description: 'Список изменений Drupal 8.9.12.'
---

**Дата релиза**: 6 января 2020

## Configuration System

- [#3181272](https://www.drupal.org/project/drupal/issues/3181272) Исправлена опечатка в сообщении об ошибке `FileStorage`.

## Database

- [#3162603](https://www.drupal.org/project/drupal/issues/3162603) Исправлена неполадка при которой некорректно работал `EntityStorageBase::loadByProperties()` для чувствительных к регистру свойств на PostgreSQL.

## File System

- [#3036494](https://www.drupal.org/project/drupal/issues/3036494) Исправлено состояние гонки для `ImageStyle::createDerivative()`.

## JavaScript

- [#3181870](https://www.drupal.org/project/drupal/issues/3181870) Исправлена опечатка «the the» в сообщении о [депрекации](../../../deprecation.md) «core/classList».

## Image System

- [#2644468](https://www.drupal.org/project/drupal/issues/2644468) Исправлена неполадка из-за которой при загрузке сразу нескольких картинок, значения высоты и ширины первой картинки применялись к остальным.

## Plugin System

- [#2916376](https://www.drupal.org/project/drupal/issues/2916376) Исправлена неполадка из-за которых было невозможно получить метку и описание для аннотации `@ContextDefinition`.

## Прочие изменения

- [#3180167](https://www.drupal.org/project/drupal/issues/3180167) Из списка мейнтенеров удалён «valthebald».
- [#3178066](https://www.drupal.org/project/drupal/issues/3178066) Улучшена документация для `ThirdPartySettingsInterface`.
- [#3188816](https://www.drupal.org/project/drupal/issues/3188816) Исправлены ссылки на документацию в файле `core.api.php`.