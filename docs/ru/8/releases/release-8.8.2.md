---
id: release-8.8.2
title: 'Drupal 8.8.2'
path: /8/releases/8.8.2
core: 8
metatags:
  title: 'Drupal 8.8.2: Список изменений'
  description: 'Минорный релиз Drupal 8.8.2, список изменений.'
---

**Дата релиза**: 2 февраля 2020 г.

## Важная информация перед обновлением

- `ConfigEntityUpdater` теперь требует использовать только одну функция в момент обновления. Ранее, где проблема протекала «тихо», теперь будут вызывать исключение. Изучите [#3100978](https://www.drupal.org/node/3100978) для более полной информации.
- Это первый релиз Drupal 8 который использует новый автоматический режим сборки (`composer create-project`) для создания релиз пакетов.

> [!INFO]
> Файлы упакованные в tar и zip и загруженные в период 1 февраля 2020 23:20 (Москва) до 2 февраля 03:15 2020 (Москва) по ошибке содержали файлы [Drupal 8.8.1](release-8.8.1.md). Если вы загрузили архив в данное окно, вам необходимо обновить файлы из актуального архива.

## Важные исправления ошибок

- [#3099986](https://www.drupal.org/project/drupal/issues/3099986) Обновление для модуля Workspaces было переработано в `hook_update_N()` для избежания конфликтов с контриб модулями.

## Прочие изменения

- [#3059934](https://www.drupal.org/node/3059934) [cilefen](https://www.drupal.org/u/cilefen) удалён из списка мейнтейнеров ядра Drupal по собственному желанию из-за нехватки времени для выполнения своих обязательств.
- [#3108025](https://www.drupal.org/node/3108025) SQL запрос для теста `testNumericExpressionSubstitution` переработан на более корректный синтаксис.
- [#3087606](https://www.drupal.org/node/3087606) Исправлена неполадка в `Datetime::getInfo()` которая кэшировала временную зону пользователя, что в итоге приводило к непредсказуемым timestamp.
- [#3108287](https://www.drupal.org/node/3108287) Тест `UpdateTest::testPrimaryKeyUpdate()` был удалён по причине того что уже имеется более проработанный вариант `testMultiUpdate()`.
- [#3108021](https://www.drupal.org/node/3108021) Улучшен процесс определения [темы оформления](../themes/themes.md) по умолчанию для [установочных профилей](../distributions/distributions.md), теперь он корректно определяет тему при установке из `config/sync`.
- [#3109433](https://www.drupal.org/node/3109433) Исправлены дампы БД с фикстурами `core/modules/system/tests/fixtures/update/drupal-8.8.0.bare.standard.php.gz` и `core/modules/system/tests/fixtures/update/drupal-8.8.0.filled.standard.php.gz` которые содержали некорректный тип профиля `NULL`.
- [#2893804](https://www.drupal.org/node/2893804) Удалён слой обратной совместимости для Rest модуля.
- [#3098521](https://www.drupal.org/node/3098521) Исправлена некорректная ссылка на ишью об устаревании функции `drupal_installation_attempted()`.
- [#3015699](https://www.drupal.org/node/3015699) Константа `MENU_MAX_MENU_NAME_LENGTH_UI` помечена устаревшей, вместо неё предложено использовать `MenuStorage::MAX_ID_LENGTH`.
- [#3101130](https://www.drupal.org/node/3101130) Улучшен тест `ConfigEntityQueryTest::testCaseSensitivity`, который мог проваливаться если в названии сгенерированной сущности оказывалось слово «test».
- [#2937782](https://www.drupal.org/node/2937782) Метод `getDefinitionFromEntity` от `ContentEntity` и `EntityContentBase` вынесен в трейт `EntityFieldDefinitionTrait`.
- [#3103976](https://www.drupal.org/node/3103976) Исправлены опечатки и форматирование Twig комментариев для views шаблонов.
- [#3094304](https://www.drupal.org/node/3094304) Добавлены тесты покрывающие не полные релизы контриб модулей, а также патчи для них.
- [#3064523](https://www.drupal.org/node/3064523) В базовом плагине сортировки views для слова «Order» добавлен контекст «Sort order».
- [#3105288](https://www.drupal.org/node/3105288) Исправлены тайпхинты с `\Drupal\workflows\WorkflowInterface` на `\Drupal\workflows\WorkflowTypeInterface` в `\Drupal\workflows\State` и `\Drupal\workflows\Transition`.
- [#3100611](https://www.drupal.org/node/3100611) Улучшен пользовательский интерфейс для модуля Workspaces.
- [#3100066](https://www.drupal.org/node/3100066) Для фильтра «Конвертация переноса строк в HTML» добавлено исключение тега `<drupal-media>`.
- [#3058853](https://www.drupal.org/node/3058853) Исправлена ошибка в запросе приводящая к проблемам на PostgreSQL 12.
- [#3106654](https://www.drupal.org/node/3106654) Исправлена ошибка в документации `hook_toolbar()` указывающая на `toolbar_pre_render()` вместо актуального `Drupal\toolbar\Element\Toolbar::preRenderToolbar()`.
- [#3092408](https://www.drupal.org/node/3092408) Исправлены ошибки в документации `field_ui_form_node_type_form_alter()` и `FieldStorageConfigListBuilder`.
- [#3094913](https://www.drupal.org/node/3094913) Исправлена ошибка в магическом сеттере `EntityForm` которая некорректно работала если для поля не было задано свойство.
- [#3086850](https://www.drupal.org/node/3086850) Для `EntityStorageBase` ошибка «Cannot load a NULL ID.» была заменена на более конкретную «Cannot load the "%s" entity with NULL ID.», где `%s` заменяется на машинное название сущности.
- [#3096566](https://www.drupal.org/node/3096566) Стили для Media Library были скопированы из Seven в Claro.
- [#2969262](https://www.drupal.org/node/2969262) Исправлена ошибка «Warning: count(): Parameter must be an array or an object that implements Countable n Drupal\views\Plugin\views\argument_validator\Entity->validateEntity()» возникающая при валидации аргументов сущности без бандлов.
- [#3096831](https://www.drupal.org/node/3096831) Исправлены стили для ссылки-кнопки темы Claro.
- [#3100470](https://www.drupal.org/node/3100470) Исправлена ошибка «undefined index» в `EditorMediaDialog` для `data-view-mode`.
- [#3027998](https://www.drupal.org/node/3027998) Улучшен плагин миграции `default_value` для обработки множественных полей.
- [#3079330](https://www.drupal.org/node/3079330) Улучшен тест `LocaleConfigSubscriberTest`. Убраны неиспользуемые проверки, добавлены новые.
- [#3099364](https://www.drupal.org/node/3099364) Для модуля Content Moderation улучшена проверка требований с корректной обработкой ситуации когда модуль Views UI отключен.
- [#3073261](https://www.drupal.org/node/3073261) Множественные улучшения трейта `CKEditorTestTrait`.
- [#3101818](https://www.drupal.org/node/3101818) Для `FieldDiscovery` изменены тайпхинты с `\Drupal\Core\Logger\LoggerChannelInterface` на `\Psr\Log\LoggerInterface`.
- [#3104420](https://www.drupal.org/node/3104420) Убрано использование фигурных скобок для массивов синтаксис которых помечен устаревшим для PHP 7.4.
- [#2903831](https://www.drupal.org/node/2903831) Исправлена ошибка в аттачментах Views приводящая к их отсутствию в некоторых случаях.

> [!NOTE]
> Список изменений переведён не до конца.

## Ссылки

- [Drupal 8.8.2](https://www.drupal.org/project/drupal/releases/8.8.2) (англ.), drupal.org, 2 февраля 2020
