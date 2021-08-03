---
title: 'Drupal 9.2.3'
slug: drupal/releases/9.2.3
core: 9
metatags:
  title: 'Drupal 9.2.3: Список изменений'
  description: 'Список изменений Drupal 9.2.3.'
---

**Дата релиза**: 3 августа 2021 г.

## Database

* [#1479220](https://www.drupal.org/project/drupal/issues/1479220) Для метода `Merge::execute()` добавлена отсутствующая документация.
* [#3185400](https://www.drupal.org/project/drupal/issues/3185400) Внесены улучшения в `UpsertTest`.

## File

* [#2834958](https://www.drupal.org/project/drupal/issues/2834958) Внесено улучшение в функцию `file_validate_extensions()` — теперь для управляемых (managed) файлов получение расширения производится по его URI, а не по названию файла, которое можно менять и может не содержать расширения.

## Install System

* [#3222577](https://www.drupal.org/project/drupal/issues/3222577) Теперь значение `DrupalKernel::$configStorage` сбрасывается в `DrupalKernel::shutdown()`. Это гарантирует, что при пересборке ядра в `drupal_install_system()` будет указано корректное хранилище конфигураций.

## JSON:API

* [#3222980](https://www.drupal.org/project/drupal/issues/3222980) Из метода `ResourceTestBase::getEntityDuplicate()` удалено бесполезное присвоение `$id_key`.

## Help Topics

* [#3192585](https://www.drupal.org/project/drupal/issues/3192585) Вызовы `url()` в Help Topics заменены на `help_topic_link()`.

## Media System

* [#3080666](https://www.drupal.org/project/drupal/issues/3080666) Исправлена неполадка, из-за которой oEmbed не мог получить миниатюру если в URL файла отсутствовало расширение файла. Теперь если расширение файла отсутствует, будет предпринята попытка определить его по MIME-типу.
* [#3186415](https://www.drupal.org/project/drupal/issues/3186415) Теперь oEmbed обработчик, прежде чем выбрасывать исключение о некорректном `Content-Type` в ответе ресурса, предпримет попытку распарсить ответ как JSON. Это вызвано тем, что некоторые ресурсы отдают JSON с `Content-Type` равным `text/html`.
* [#3160238](https://www.drupal.org/project/drupal/issues/3160238) Виджет Media Library теперь имеет красную рамку в случае ошибки.

## Migration System

* [#3184184](https://www.drupal.org/project/drupal/issues/3184184) Добавлен тест `d7/FollowUpMigrationsTest.php`, который убеждается что связи на ноды из других типов сущностей мигрируют корректно.

## Node System

* [#2935654](https://www.drupal.org/project/drupal/issues/2935654) В методе `NodeListBuilder::buildRow()` улучшена подготовка ссылки для многоязычного материала.

## Olivero

* [#3222313](https://www.drupal.org/project/drupal/issues/3222313) Файл `scripts.js` переименован в `navigation-utils.js` для того чтобы отражать своё назначение.
* [#3223270](https://www.drupal.org/project/drupal/issues/3223270) Исправлена неполадка из-за которой текст «Закрыть» у системного сообщения было не видно на IE11, Edge и Firefox в режиме высокой контрастности.

## Path

* [#3221966](https://www.drupal.org/project/drupal/issues/3221966) Исправлено значение по умолчанию для параметра `$message` в методе `PathAliasTestTrait::assertPathAliasNotExists()`.

## Text

* [#2750925](https://www.drupal.org/project/drupal/issues/2750925) Исправлена неполадка в генерации образцов значений при помощи `generateSampleValue()` для `TextItem` поля если максимальная длина поля менее 3 символов.

## Tour

* [#3224861](https://www.drupal.org/project/drupal/issues/3224861) Исправлены нарушения Drupal Coding Standards в `TourViewBuilder`.

## Views

* [#3221933](https://www.drupal.org/project/drupal/issues/3221933) Исправлена неполадка приводящая к PHP Notice: «Notice: Undefined index: left_field in Drupal\views\Plugin\views\join\JoinPluginBase->__construct()».

## Тестирование

* [#3222783](https://www.drupal.org/project/drupal/issues/3222783) Результат вызова метода `PHPUnit\Framework\Assert::assertEquals()` больше не используется как результат теста.
* [#3223267](https://www.drupal.org/project/drupal/issues/3223267) Вызовы функции `drupal_flush_all_caches()` в тестах заменены на более корректные и точечные варианты чистки кеша.
* [#3225351](https://www.drupal.org/project/drupal/issues/3225351) В `tests/bootstrap.php` исправлено некорректное пространство имён дла трейтов.
* [#3221312](https://www.drupal.org/project/drupal/issues/3221312) Часть сравнений в текстах были заменены на `::willReturn*()`.
* [#3153469](https://www.drupal.org/project/drupal/issues/3153469) Функция `t()` больше не используется при вызове `::clickLink()`. 

## Прочие изменения

* [#3220379](https://www.drupal.org/project/drupal/issues/3220379) Пример с кодом для `NullCoalesce` теперь корректно отформатирован.
* [#3207111](https://www.drupal.org/project/drupal/issues/3207111) Улучшена документация для `ScaffoldFilePath::__construct()`.
* [#3219198](https://www.drupal.org/project/drupal/issues/3219198) Исправлено некорректное пространство имён в документации `\Drupal\Core\Entity\Query\QueryInterface::condition()`.
* [#3224583](https://www.drupal.org/project/drupal/issues/3224583) Улучшен скрипт `commit-code-check.sh`. Теперь он проверяет все файлы при помощи PHPCS даже если `phpcs.xml.dist` был изменён.
* [#3221062](https://www.drupal.org/project/drupal/issues/3221062) В документацию к методу `EntityDefinitionUpdateManagerInterface::getEntityType()` добавлен `NULL` тип в качестве возможного результата.
* [#2784203](https://www.drupal.org/project/drupal/issues/2784203) В документацию к `QueryInterface::currentRevision()` добавлено объяснение, что такое «текущая версия».