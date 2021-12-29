---
title: 'Drupal 9.4.0'
slug: wiki/drupal/releases/9.4.0
core: 9
metatags:
  title: 'Drupal 9.4.0: Список изменений'
  description: 'Список изменений Drupal 9.4.0.'
---

> [!WARNING]
> Данная версия находится в разработке. Список изменений может меняться и дополняться, даты могут смещаться.

**Дата релиза**: июнь 2022\
**Активная поддержка до**: декабрь 2022\
**Поддержка безопасности до**: июнь 2023

## Хук `THEME_ENGINE_init()` объявлен устаревшим

* [#3158289](https://www.drupal.org/node/3158289) 

Хук `THEME_ENGINE_init()` объявлен устаревшим и будет удалён в [Drupal 10](../../../../10/index.md). Замена данному хуку не предоставляется.

## Для сущностей с поддержкой ревизий, модуль Views теперь предоставляет связи между основной и ревизионной таблицами

* [#2652652](https://www.drupal.org/node/2652652)

Ранее, модуль Views не предоставлял для ревизионных сущностей связи между основной таблицой и таблицой ревизий. Для модулей `block_content` и `node` были «костыли» для решения данной проблемы, но остальным сущностям приходилось решать это вопрос самостоятельно.

Начиная с текущего релиза, Views предоставляет соответствующие связи между основной и ревизионной таблицами для всех типов сущностей что поддерживают ревизии.

## В .htaccess файлы добавлена поддержка настроек PHP 8

* [#3186524](https://www.drupal.org/node/3186524)

В `.htaccess` файлы добавлен новый раздел для настроек параметров PHP 8. Это изменение касается как основного файла `.htaccess`, так и всех прочих что генерируются Drupal для различных ситуаций, например, для `sites/default/files`.

В эти файлы добавлена настройка для PHP 8:

```text
# Имеющийся раздел для PHP 7.
<IfModule mod_php7.c>
  php_value assert.active   0
</IfModule>
# Новые настройки для PHP 8.
<IfModule mod_php.c>
  php_value assert.active   0
</IfModule>
```

Если вы переопределяете данные файлы или изменяете параметры PHP, для PHP 8 вам необходимо скопировать данные настройки и поправить под себя. Если вы используете стандартные настройки, файлы обновятся автоматически.

## Cache System

* [#2873732](https://www.drupal.org/node/2873732) Внесены улучшения в `CookiesCacheContext`, который мог приводить к ошибке «Array
  to string conversion in CacheContextsManager::convertTokensToKeys()».

## Claro

* [#3214170](https://www.drupal.org/node/3214170) Кнопка «Отмена» теперь отцентрована в off-canvas модальном окне.
* [#3247994](https://www.drupal.org/node/3247994) Исправлена неполадка, из-за которой мог некорректно работать элемент формы `password`.

## Composer

* [#3246595](https://www.drupal.org/node/3246595) Зависимости ядра обновлены на 01.11.21.

## Database System

* [#3213644](https://www.drupal.org/node/3213644) Внесены улучшения в тест `StatementWrapperLegacyTest::testClientStatementMethod()`.

## Datetime

* [#3251100](https://www.drupal.org/node/3251100) Исправлена неполадка, из-за которой `DateTimeWidgetBase` дважды устанавливал одну и ту же временную зону.

## Extension System

* [#3245622](https://www.drupal.org/node/3245622) Ссылкам на справку и права доступа конкретного модуля, в списке модулей, добавлено указание, для какого модуля они а также `title` аттрибуты.

## Field System

* [#3213023](https://www.drupal.org/node/3213023) `FieldConfig` теперь выводит более полезные и понятные сообщения об
  ошибках.

## Filter

* [#3227821](https://www.drupal.org/node/3227821) Исправлена неполадка в фильтре «Заменять переводы строк соответствующими HTML-тегами», которая могла приводить к поломке SVG элементов.

## Image

* [#3223233](https://www.drupal.org/node/3223233) Улучшены заголовки страниц для форм добавления и редактирования [стилей изображений](../../../../9/image/image-styles/index.md).

## JavaScript

* [#3238860](https://www.drupal.org/node/3238860) Использование `jQuery.map()` заменено на нативную `map()` функцию.
* [#3239500](https://www.drupal.org/node/3239500) Добавлен полифил для `Array.includes()`.

## JSON:API

* [#3199696](https://www.drupal.org/node/3199696) `ResourceObject` теперь учитывает текущий язык и корректно кеширует результаты для мультиязычных ресурсов.

## Layout Builder

* [#3253666](https://www.drupal.org/node/3253666) `LayoutTempstoreRouteEnhancer` теперь использует `
  Drupal\Core\Routing\RouteObjectInterface` вместо `Symfony\Cmf\Component\Routing\RouteObjectInterface`.

## Media Library

* [#3173770](https://www.drupal.org/node/3173770) `MediaLibraryFieldWidgetOpener` теперь позволяет использовать другие референс поля, расширяющее поле из ядра и использующие `EntityReferenceFieldItemList`.
* [#3248454](https://www.drupal.org/node/3248454) Внесены улучшения в `MediaLibraryStateTest` для совместимости с Symfony 5.4.

## Migration System

* [#3246053](https://www.drupal.org/node/3246053) Обновлено значение `filesize` файла `ds9.txt` в `file_managed`.
* [#3163663](https://www.drupal.org/node/3163663) Исправлена неполадка из-за которой плагин-обработчик `download` мог приводить к предупреждению «failed to open stream: Too many open file».
* [#3087332](https://www.drupal.org/node/3087332) Плагин-обработчик `d6_url_alias_language` объявлен устаревшим.

## Olivero

* [#3186992](https://www.drupal.org/node/3186992) Исправлена неполадка, из-за которой навигационные пункты меню могли выходить за рамки контейнера.

## Standard Profile

* [#3171149](https://www.drupal.org/node/3171149) Стандартный профиль теперь использует стиль изображения `wide` для
  публикаций вместо `large`.

## Update

* [#3253639](https://www.drupal.org/node/3253639) Добавлен новый класс `UpdateUploaderTestBase` с универсальной реализацией `::setUp()` для уменьшения дублирования кода в тестах модуля.

## User

* [#3198010](https://www.drupal.org/node/3198010) Улучшено отображение ошибки о неудачных попытках авторизации, чтобы снизить нагрузку на систему. `UserLoginForm` теперь принимает сервис `bare_html_page_renderer` в качестве аргумента.

## Views

* [#2569381](https://www.drupal.org/node/2569381) Из `Drupal\views\Plugin\views\area\Result` удалён лишний вызов `XSS::adminFilter()`.

## Symfony 6

* [#3232074](https://www.drupal.org/node/3232074) Для классов расширяющих `Normalizer` добавлен тайпхинт `array|string|int|float|bool|\ArrayObject|null` методу `::normalize()`.
* [#3232095](https://www.drupal.org/node/3232095) Произведён рефакторинг сервиса `update.root` для того чтобы он возвращал объект вместо строки.
* [#3232131](https://www.drupal.org/node/3232131) В `DebugClassLoader` добавлены тайпхинты.
* [#3250299](https://www.drupal.org/node/3250299) Внесены улучшения в констрейнты для совместимости с Symfony 6.
* [#3250442](https://www.drupal.org/node/3250442) Код с использованием Prophecy на методы с тайпхинтом на возвращаемое значение `static` заменены на моки для исправления проблем, пока поддержка не появится в Prophecy.

## Прочие изменения

* [#3038596](https://www.drupal.org/node/3038596) В `drupalci.yml` добавлено напоминание о том, что данный файл требуется в ручном изменении при создании ново ветки ядра.
* [#3245820](https://www.drupal.org/node/3245820) Из кода удалены упоминания удалённого `\Drupal\Core\Action\Plugin\Action\PublishAction`.
* [#3250263](https://www.drupal.org/node/3250263) Удалён неиспользуемый файл `core/scripts/test/test.script`.
* [#3251891](https://www.drupal.org/node/3251891) Исправлены неполадки, приводящие к проблемам на ветке Drupal 10.
* [#3251988](https://www.drupal.org/node/3251988) Обновлена документация о возвращаемых типах данных для `JSWebAssert::waitForText()`.