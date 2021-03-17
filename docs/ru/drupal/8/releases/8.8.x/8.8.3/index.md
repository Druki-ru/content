---
title: 'Drupal 8.8.3'
slug: 8/releases/8.8.3
core: 8
metatags:
  title: 'Drupal 8.8.3: Список изменений'
  description: 'Минорный релиз Drupal 8.8.3, список изменений.'
---

**Дата релиза**: 4 марта 2020 г.

## Добавлен новый объект AttributeHelper

- [#3090145](https://www.drupal.org/project/drupal/issues/3090145)

Добавлен новый объект `Drupal\Core\Template\AttributeHelper` который позволит управлять атрибутами независимо от того, объявлены они как массив, или как `Attribute`.

Класс предоставляет два статических метода:

- `::attributeExists`: Проверяет на наличие атрибута.
- `::mergeCollections`: Объединяет два значения, независимо, массивы они или объекты, или два разных значения. Если первое значение является объектом `Attribute`, то результатом будет тоже объект, если массив ­— то будет возвращён массив.

Также для класса `Attribute` было добавлено два новых метода:

- `::hasAttribute`: Проверяет на наличие атрибута.
- `::merge`: Объединяет свои значения со значениями другого `Attribute` объекта.

## Оформление и темизация

- [#3111729](https://www.drupal.org/node/3111729) Шаблоны и CSS Stable [темы оформления](../../../themes/index.md) приведённые к их оригиналу и улучшена документация.

## Composer

- [#3103918](https://www.drupal.org/node/3103918) Composer плагины Drupal теперь являются внутренним API.

## Claro

- [#3111390](https://www.drupal.org/node/3111390) Исправлена ошибка в рендер массиве из-за которой не происходило добавление класса `media-library-add-form--oembed`.

## Entity API

- [#3056539](https://www.drupal.org/node/3056539) Исправлена неполадка приводящая к ошибке когда происходит обновление не ревизионной сущности в ревизионную, но при этом у сущности есть не ревизионные поля хранимые в отдельных таблицах.

## Editor

- [#3111658](https://www.drupal.org/node/3111658) Исправлен комментарий для `editor_field_formatter_info_alter()` который по ошибке указывал что это реализация `hook_form_FORM_ID_alter()`.

## Field

- [#3116553](https://www.drupal.org/node/3116553) Для `PluginSettingsInterface` убрана метка что интерфейс устаревший, так как он активно используется в ядре. Рекомендуется не использовать данный интерфейс в своих модулях, а вместо этого использовать `\Drupal\Component\Plugin\ConfigurableInterface`. В будущем `PluginSettingsInterface` всё равно будет удалён.

## Layout Builder

- [#3108640](https://www.drupal.org/node/3108640) Исправлена неполадка, при которой, если доступа к связанной сущности блока нет, не собирались кеш метаданные.
- [#3088077](https://www.drupal.org/node/3088077) Исправлена неполадка, при которой, если блок возвращает пустой результат, не собирались кеш метаданные.

## Menu Link Content

- [#3067576](https://www.drupal.org/node/3067576) Улучшена миграция `<nolink>` ссылок меню.

## Migration

- [#3095922](https://www.drupal.org/node/3095922) Улучшена миграция комментариев из Drupal 7 для которых не указан язык.
- [#3100046](https://www.drupal.org/node/3100046) Улучшена миграция поля с опциями из Drupal 7 для которых включена поддержка i18n.

## Path alias

- [#3114366](https://www.drupal.org/node/3098718) Теперь при сохранении сущности синонима будет вызываться ошибка о несоответствии схемы БД если до сих пор не были запущены обновления синонимов из [Drupal 8.8.0](../8.8.0/index.md).  Для того чтобы отключить данное поведение, вы можете добавить настройку `$settings['system.path_alias_schema_check'] = FALSE;` в [settings.php](../../../settings-php/index.md).

## Taxonomy

- [#3059387](https://www.drupal.org/node/3059387) Исправлена неполадка, из-за которой предпросмотр нового термина (например в Layout Builder) приводил к исключению `EntityMalformedException`.

## Views

- [#3065720](https://www.drupal.org/node/3065720) Добавлена обработка двойного слэша (`//`) в начале пути. Теперь не важно в каком формате введён путь, он будет иметь лишь один.
- [#2989609](https://www.drupal.org/node/2989609) Исправлена опечатка в переменной `$filter_hander` `ExposedFormPluginBase`.

## Update

- [#3116198](https://www.drupal.org/node/3116198) Класс `ProjectCoreCompatibility` помечен финальным.
- [#3111929](https://www.drupal.org/node/3111929) Исправлена неполадка, из-за которой могли рекомендоваться dev, alpha, beta и rc если не обнаружен более свежий стабильный релиз.
- [#3102724](https://www.drupal.org/node/3102724) Улучшен вывод информации о совместимости модуля с ядром. Теперь показываются ограничения, с какой по какую версию совместим модуль, а также, до какой версии необходимо обновить ядро, если текущая версия несовместима.
- [#3074993](https://www.drupal.org/node/3074993) URL для проверки обновлений изменён с текущей версии ядра получаемой из `\Drupal::CORE_COMPATIBILITY` (`https://updates.drupal.org/release-history/paragraphs/8.x`) на `current` (`https://updates.drupal.org/release-history/paragraphs/current`). Данные адреса возвращают разный XML, в связи с этим весь код и тесты были исправлены на новый вариант.
- [#3096078](https://www.drupal.org/project/drupal/issues/3096078), [#3102724](https://www.drupal.org/project/drupal/issues/3102724), [#3101547](https://www.drupal.org/node/3101547) Обновлён шаблон `update-version.html.twig` для поддержки `core_compatibility_details`.

## Workspaces

- [#3098427](https://www.drupal.org/node/3098427) Удаление `workspace_association` сущности перенесено из `workspaces_update_8803()` в `workspaces_post_update_remove_association_schema_data()` для избежания конфликтов в процессе обновления и сохранения обратной совместимости.

## Тестирование

- [#3113292](https://www.drupal.org/node/3113292) Добавлены тесты для проверки поведения при обновлении модуля со статусами: `UpdateManagerInterface::NOT_SECURE`, `UpdateManagerInterface::REVOKED`, `UpdateManagerInterface::NOT_SUPPORTED`.
- [#3104372](https://www.drupal.org/node/3104372) Сообщения об устаревшем коде в `Drupal\FunctionalTests\AssertLegacyTrait` отформатированны в соответствии с требованиями стандарта.
- [#3113509](https://www.drupal.org/node/3113509) Аннотации `@expectedException*` заменены на соответствующие методы, для будущей совместимости с PHPUnit 9.
- [#2871374](https://www.drupal.org/node/2871374) Улучшен тест `SelectTest::testVulnerableComment`.
- [#3106215](https://www.drupal.org/node/3106215) Удалён приватный метод `UserBlocksTest::updateAccess` который нигде не используется.
- [#3086644](https://www.drupal.org/node/3086644) Добавлены тесты для сборки Composer шаблонов Drupal.
- [#3063912](https://www.drupal.org/node/3063912) `UpdatePathTestBase::runUpdates` перемещён в трейт `UpdatePathTestTrait`.
- [#3090017](https://www.drupal.org/node/3090017) Добавлен трейт `RdfParsingTrait`.

## Прочие изменения

- [#3098718](https://www.drupal.org/project/drupal/issues/3098718) Теперь при сохранении сущности синонима будет вызываться ошибка о несоответствии схемы БД если до сих пор не были запущены обновления синонимов из [Drupal 8.8.0](../8.8.0/index.md).  Для того чтобы отключить данное поведение, вы можете добавить настройку `$settings['system.path_alias_schema_check'] = FALSE;` в [settings.php](../../../settings-php/index.md).
- [#3101299](https://www.drupal.org/node/3101299) Исправлена неполадка, из-за которой было невозможно устанавливать модули из .zip архивов.
- [#3105327](https://www.drupal.org/node/3105327) CKEditor обновлён до версии 4.13.1.
- [#2991207](https://www.drupal.org/node/2991207) Улучшен отчёт о состоянии текущей версии Drupal. Теперь указывается, поддерживается ли текущая версия сайта и до какой версии. Для старых версий выводится предупреждение о необходимости обновления и до какой версии.
- [#3110186](https://www.drupal.org/node/3110186) На странице с отчётами улучшены описания для новых версий Drupal.
- [#3114545](https://www.drupal.org/node/3114545) Исправлена документация для `RoleInterface::ANONYMOUS_ID` и `RoleInterface::AUTHENTICATED_ID`. В них было старое упоминание таблицы `role`, но сейчас у нас для ролей используются сущности.
- [#3104071](https://www.drupal.org/node/3104071) Исправлена документация для `DrupalDateTime::$formatTranslationCache`. Было неверное указание типа в виде строки, исправлено на массив и добавлены примеры.
- [#3105095](https://www.drupal.org/node/3105095) Исправлена документация для `ElementInfoManagerInterface::getInfoProperty`.
- [#3114116](https://www.drupal.org/node/3114116) Устаревшие переменные и функции в `ajax.es6.js` помеченные на удаление в Drupal 9 продлены до Drupal 10.
- [#3113236](https://www.drupal.org/node/3113236) Исправлена некорректная ссылка на документацию в Help Topics.
- [#3113476](https://www.drupal.org/node/3113476) Улучшено получение времени через `Time::getRequestTime`. Если в момент обращения ещё не получен объект запроса, то производится попытка получить значение из `$_SERVER['REQUEST_TIME']`, если и то значение недоступно, получается системное время.
- [#3113284](https://www.drupal.org/node/3113284) Устаревшая константа `REQUEST_TIME` будет удалена в Drupal 10, а не Drupal 9, как планировалось ранее.
- [#3110104](https://www.drupal.org/node/3110104) Исправлена документация для `RouteProvider::getRouteByName`
- [#3112829](https://www.drupal.org/node/3112829) Использование `};` исправлено на `}` в соответствии с требованиями стандартов.
- [#3108540](https://www.drupal.org/node/3108540) Комментарии об устаревшем коде в `database.inc` приведены в соответствии со стандартом.
- [#2809237](https://www.drupal.org/node/2809237) Для всех методов трейта `AllowedTagsXssTrait`, который помечен как устаревший, добавлены вызовы ошибок с информацией об устаревшем коде.
- [#3112263](https://www.drupal.org/node/3112263) Константы `REGIONS_*` больше не признаны устаревшими так как используются не только в модуле Block.
- [#2738879](https://www.drupal.org/node/2738879) Исправлена неполадка из-за которой некоторые `hook_update_N()` функции могли быть не обнаружены и не вызывались.
- [#3086374](https://www.drupal.org/node/3086374) Drupal ядро теперь полностью совместимо с PHP 7.4.
- [#3111504](https://www.drupal.org/node/3111504) `AccessResult::cacheUntilEntityChanges` теперь вызывает ошибку об устаревшем методе.
- [#3091518](https://www.drupal.org/node/3091518) webchick убрана из MAINTAINERS.txt как координатор Admin UI.

## Ссылки

- [Drupal 8.8.3](https://www.drupal.org/project/drupal/releases/8.8.3)  (англ.), drupal.org, 4 марта 2020