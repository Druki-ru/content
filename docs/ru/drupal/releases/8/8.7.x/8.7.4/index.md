---
title: 'Drupal 8.7.4'
slug: wiki/drupal/releases/8.7.4
core: 8
metatags:
  title: 'Drupal 8.7.4: Список изменений'
  description: 'В данном релизе исправлены различные ошибки и внесены улучшения.'
authors:
  - Niklan
category:
  area: 'Drupal 8.7.x'
  title: Drupal 8.7.4
  order: 4
---

**Дата релиза**: 4 июля 2019 г.

Это патч-релиз Drupal 8, готовый к использованию на продакшен сайтах.

## Media

- [#3059022](https://www.drupal.org/node/3059022) Теперь тесты не подключаются к внешним сервисам, таким как Vimeo и YouTube. Вместо этого, происходит эмуляция соединения. Это позволяет исключить ошибки в тестах из-за недоступности данных сервисов.

## Media Library

- [#3038254](https://www.drupal.org/node/3038254) Добавлена возможность запрещать доступ к Media Library на основе того, откуда пришел запрос.
- [#3035408](https://www.drupal.org/node/3035408) Улучшена разметка для навигационных вкладок выбора типа медиа файла.
- [#2990664](https://www.drupal.org/node/2990664) Исправлена ошибка, из-за которой Media Library виджет не работал если Drupal установлен в подкаталог.
- [#3035446](https://www.drupal.org/node/3035446) Улучшено поведение формы для скрин ридеров. Элемент отображающий кол-во медиа доступных до добавления теперь не расценивается как активный элемент.
- [#3058943](https://www.drupal.org/node/3058943) Название модуля теперь "Media Library", а не "Media library".
- [#3038350](https://www.drupal.org/node/3038350) Доступ к виджету Media Library по умолчанию будет запрещен, если при запросе не передан валидный объект состояния.

## Migrate API

- [#3042223](https://www.drupal.org/node/3042223) Форматтер поля `text_plain` теперь обрабатывается как `string`.
- [#3049895](https://www.drupal.org/node/3049895) `setupMigrations()` перемещен из `FormBase` в `CredentialForm`.
- [#3060603](https://www.drupal.org/node/3060603) Исправлена ошибка, при попытке показать превью для media_library в модуле Views, когда недостаточно аргументов для обработки виджета.
- [#2979966](https://www.drupal.org/node/2979966) Добавлена поддержка миграции языка для терминов таксономии из D7 где используется модуль i18n_taxonomy.
- [#2993927](https://www.drupal.org/node/2993927) Исправлена ошибка при миграции настроек для терминов таксономии D7, где в качестве форматтера поля выбран формат отрендеренной сущности.
- [#2763637](https://www.drupal.org/node/2763637) Доработана миграция полей связи терминов таксономии. Теперь также мигрирует настройка доступных словарей для выбора терминов в поле.
- [#2968170](https://www.drupal.org/node/2968170) Исправлена ошибка миграции ссылок меню, у которых родитель содержит внешний URI.

## Image

- [#2829990](https://www.drupal.org/node/2829990) В шаблоне форматера изображения теперь используется Twig функция `link()`, вместо разметки ссылки.

## Workspaces

- [#3054582](https://www.drupal.org/node/3054582) Добавлена вкладка "Управление полями" для Workspace сущности.
- [#3027598](https://www.drupal.org/node/3027598) presave и predelete хуки запускаются только для внутренних сущностей Workspaces.
- [#3021247](https://www.drupal.org/node/3021247) Пользователи с пермишеном `bypass entity access own workspace` теперь могут добавлять и редактировать содержимое в рабочих областях, к которым у них есть доступ.

## Quickedit

- [#3045384](https://www.drupal.org/node/3045384) Quickedit теперь дополнительно проверяет, является ли сущность ревизионной, прежде чем вызывать методы, которые доступны только данному типу сущностей.

## Views

- [#3060081](https://www.drupal.org/node/3060081) Теперь не производится пересчет данных кеша в момент установки модуля.

## Demo Umami

- [#3050577](https://www.drupal.org/node/3050577) Убрано скрытый вывод названия сайта в брендированном блоке.
- [#2991577](https://www.drupal.org/node/2991577) Название категорий теперь во множественном числе.
- [#3044366](https://www.drupal.org/node/3044366) Улучшено отображение элементов управления Layout Builder.

## JSON:API

- [#3039730](https://www.drupal.org/node/3039730) Определение путей происходит теперь один раз на один тип и бандл сущности. Должно сказаться на производительности при большом количестве разнообразных сущностей, которые ссылаются на медиа сущности.

## Node

- [#3056616](https://www.drupal.org/node/3056616) Удален неиспользуемый код в `NodeTypeForm::actions()`.

## Прочие изменения

- [#3055760](https://www.drupal.org/node/3055760) Улучшены стили для вертикальных вкладок в RTL версии.
- [#3056454](https://www.drupal.org/node/3056454) Исправлена ошибка валидации HEX цветов в `Drupal\Component\Utility\Color::validateHex()`, которая возвращала `TRUE`, если строка содержит несколько `#`.
- [#2979044](https://www.drupal.org/node/2979044) Исправлена ошибка в `\Drupal\Core\Render\Element\Weight::processWeight()` приводящая к рендеру нерабочего элемента формы, если превышено значение `system.site.weight_select_max`.
- [#3062556](https://www.drupal.org/node/3062556) `\Drupal\FunctionalTests\Update\UpdatePathTestBase` теперь имеет поддержку свойства `$configSchemaCheckerExclusions`.
- [#3061564](https://www.drupal.org/node/3061564) В комментарии для возвращаемого типа `\Drupal::getContainer()` убран `NULL`, так как метод всегда возвращает `\Symfony\Component\DependencyInjection\ContainerInterface` или бросает исключение.
- [#2944552](https://www.drupal.org/node/2944552) Исправлена документация для `#default_value` `Date` элемента формы.
- [#3056414](https://www.drupal.org/node/3056414) Исправлена документация для `Drupal\Core\Site\Settings`.
- [#3060983](https://www.drupal.org/node/3060983) Обновлена версия **chokidar** для совместимости с актуальными node/npm/yarn версиями.
- [#3056008](https://www.drupal.org/node/3056008) Для `WebDriverTestBase` задано разрешение экрана по умолчанию в 1024x768.
- [#3016064](https://www.drupal.org/node/3016064) Улучшена документация для свойства `#date_timezone` - `Detetime` и `Datelist` элементов.
- [#3059564](https://www.drupal.org/node/3059564) Исправлена ошибка в описании устаревшего метода `EntityInterface::link()`.

## Ссылки

- [Drupal 8.7.4](https://www.drupal.org/project/drupal/releases/8.7.4) (англ.), drupal.org, 4 июля 2019
