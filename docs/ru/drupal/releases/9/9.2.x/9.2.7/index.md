---
title: 'Drupal 9.2.7'
slug: drupal/releases/9.2.7
core: 9
metatags:
  title: 'Drupal 9.2.7: Список изменений'
  description: 'Список изменений Drupal 9.2.7.'
---

> [!WARNING]
> Данная версия находится в разработке и не предназначена для использования.

## CKeditor

* [#2556069](https://www.drupal.org/node/2556069) Исправлена неполадка приводящая к JS ошибке при использовании фильтра `filtered_html`.

## Filter

* [#2016739](https://www.drupal.org/node/2016739) Исправлена неполадка при которой URL-адреса содержащие `@` конвертировались в email-адреса даже если не имели домена после символа.

## HAL

* [#2928882](https://www.drupal.org/node/2928882) Исправлена неполадка из-за которой генерировались некорректные ссылки при использовании разных доменов, протоколов или портов при мультисайтинге.

## JSON:API

* [#3225034](https://www.drupal.org/node/3225034) Произведён небольшой рефакторинг кода в `ResourceTypeRepository::all()` для упрощения кода.

## Layout Builder

* [#3104980](https://www.drupal.org/node/3104980) `layout_builder_system_breadcrumb_alter()` теперь проверяет что объект маршрута существует.

## Olivero

* [#3211616](https://www.drupal.org/node/3211616) Изменён цвет для основной кнопки при наведении, чтобы он был более контрастным.
* [#3229094](https://www.drupal.org/node/3229094) Заголовки материалов теперь обтекают изображение если недостаточно места для удобства чтения заголовка.
* [#3190537](https://www.drupal.org/node/3190537) Исправлена неполадка, из-за которой поиск в режиме планшета не обрабатывал нажатия клавиш в IE11.
* [#3211395](https://www.drupal.org/node/3211395) Улучшен фон и контрастность кнопки отправки комментария.

## Render System

* [#2938969](https://www.drupal.org/node/2938969) Упоминания функции `drupal_render()` в документации заменены на `RendererInterface` и соответствующие методы.

## Views

* [#3230690](https://www.drupal.org/node/3230690) Исправлена документация для `Drupal\views\Plugin\views\display::viewExposedFormBlocks()`.

## Тестирование

* [#3139409](https://www.drupal.org/node/3139409) Использование устаревшего `AssertLegacyTrait::assertRaw()` заменено на современные аналоги.

## Прочие изменения

- [#3231263](https://www.drupal.org/node/3231263) Добавлено три новых мейнтенера ядра: [Cristina Chumillas (ckrina)](https://www.drupal.org/u/ckrina), [Ben Mullins (bnjmnm)](https://www.drupal.org/u/bnjmnm), [Victoria Spagnolo (quietone)](https://www.drupal.org/u/quietone).