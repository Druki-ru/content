---
title: 'Drupal 9.2.7'
slug: drupal/releases/9.2.7
core: 9
metatags:
  title: 'Drupal 9.2.7: Список изменений'
  description: 'Список изменений Drupal 9.2.7.'
---

**Дата релиза**: 6 октября 2021

## Claro

* [#3229172](https://www.drupal.org/node/3229172) Исправлена неполадка, при которой подчёркнутый текст в CKEditor не имел подчёркивания в теме.
* [#3194669](https://www.drupal.org/node/3194669) Внесены улучшения в оформление высокой контрастности.

## CKeditor

* [#2556069](https://www.drupal.org/node/2556069) Исправлена неполадка приводящая к JS ошибке при использовании фильтра `filtered_html`.

## Filter

* [#2016739](https://www.drupal.org/node/2016739) Исправлена неполадка при которой URL-адреса содержащие `@` конвертировались в email-адреса даже если не имели домена после символа.

## HAL

* [#2928882](https://www.drupal.org/node/2928882) Исправлена неполадка из-за которой генерировались некорректные ссылки при использовании разных доменов, протоколов или портов при мультисайтинге.

## JSON:API

* [#3225034](https://www.drupal.org/node/3225034) Произведён небольшой рефакторинг кода в `ResourceTypeRepository::all()` для упрощения кода.
* [#3214675](https://www.drupal.org/node/3214675) Исправлена неполадка приводящая к `422 Unprocessable Entity` при загрузке файлов в корень публичной директории.

## Layout Builder

* [#3104980](https://www.drupal.org/node/3104980) `layout_builder_system_breadcrumb_alter()` теперь проверяет что объект маршрута существует.

## Media System

* [#3230772](https://www.drupal.org/node/3230772) `OembedMediaController` теперь корректно собирает кеш-метаданные.
* [#3230547](https://www.drupal.org/node/3230547) `\Drupal\media\Controller\OEmbedIframeController::render()` теперь задаёт заголовок `Content-Type` в своём ответе.

## Migration System

* [#3085192](https://www.drupal.org/node/3085192) Добавлен индекс на `source_ids_hash` для таблиц `migrate_message_*`.
* [#3200534](https://www.drupal.org/node/3200534) `ContentEntityTest` теперь использует `@dataprovider`.
* [#3229734](https://www.drupal.org/node/3229734) Внесены улучшения и комментарии к тесту `ContentEntityTest`.

## Olivero

* [#3211616](https://www.drupal.org/node/3211616) Изменён цвет для основной кнопки при наведении, чтобы он был более контрастным.
* [#3229094](https://www.drupal.org/node/3229094) Заголовки материалов теперь обтекают изображение если недостаточно места для удобства чтения заголовка.
* [#3190537](https://www.drupal.org/node/3190537) Исправлена неполадка, из-за которой поиск в режиме планшета не обрабатывал нажатия клавиш в IE11.
* [#3211395](https://www.drupal.org/node/3211395) Улучшен фон и контрастность кнопки отправки комментария.
* [#3228801](https://www.drupal.org/node/3228801) Исправлена неполадка, из-за которой у выпадающих пунктов меню при наведении не было индикации.
* [#3226010](https://www.drupal.org/node/3226010) Улучшена контрастность ссылки «Пропустить».
* [#3211622](https://www.drupal.org/node/3211622) Исправлена неполадка, при которой текст в блоке брендирования мог быть обрезан если он более двух строк.

## Render System

* [#2938969](https://www.drupal.org/node/2938969) Упоминания функции `drupal_render()` в документации заменены на `RendererInterface` и соответствующие методы.

## User

* [#3132145](https://www.drupal.org/node/3132145) Исправлена неполадка из-за которой контекстуальный фильтр Views «allow multiple» не работал для фильтрации пользовательских ролей.

## Views

* [#3230690](https://www.drupal.org/node/3230690) Исправлена документация для `Drupal\views\Plugin\views\display::viewExposedFormBlocks()`.

## Тестирование

* [#3139409](https://www.drupal.org/node/3139409) Использование устаревшего `AssertLegacyTrait::assertRaw()` заменено на современные аналоги.
* [#3130606](https://www.drupal.org/node/3130606) Использование `MockBuilder::setMethods()` заменено на современные аналоги, так как метод помечен устаревшим.

## Прочие изменения

- [#3231263](https://www.drupal.org/node/3231263) Добавлено три новых мейнтенера ядра: [Cristina Chumillas (ckrina)](https://www.drupal.org/u/ckrina), [Ben Mullins (bnjmnm)](https://www.drupal.org/u/bnjmnm), [Victoria Spagnolo (quietone)](https://www.drupal.org/u/quietone).