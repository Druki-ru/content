---
title: 'Drupal 8.8.2'
slug: wiki/drupal/releases/8.8.2
core: 8
metatags:
  title: 'Drupal 8.8.2: Список изменений'
  description: 'Минорный релиз Drupal 8.8.2, список изменений.'
authors:
  - Niklan
---

**Дата релиза**: 2 февраля 2020 г.

## Важная информация перед обновлением

- `ConfigEntityUpdater` теперь требует использовать только одну функция в момент обновления. Ранее, где проблема протекала «тихо», теперь будут вызывать исключение. Изучите [#3100978](https://www.drupal.org/node/3100978) для более полной информации.
- Это первый релиз Drupal 8 который использует новый автоматический режим сборки (`composer create-project`) для создания релиз пакетов.

> [!NOTE]
> Файлы упакованные в tar и zip и загруженные в период 1 февраля 2020 23:20 (Москва) до 2 февраля 03:15 2020 (Москва) по ошибке содержали файлы [Drupal 8.8.1](../8.8.1/index.md). Если вы загрузили архив в данное окно, вам необходимо обновить файлы из актуального архива.

## Важные исправления ошибок

- [#3099986](https://www.drupal.org/project/drupal/issues/3099986) Обновление для модуля Workspaces было переработано в `hook_update_N()` для избежания конфликтов с контриб модулями.

## Прочие изменения

- [#3059934](https://www.drupal.org/node/3059934) [cilefen](https://www.drupal.org/u/cilefen) удалён из списка мейнтейнеров ядра Drupal по собственному желанию из-за нехватки времени для выполнения своих обязательств.
- [#3108025](https://www.drupal.org/node/3108025) SQL запрос для теста `testNumericExpressionSubstitution` переработан на более корректный синтаксис.
- [#3087606](https://www.drupal.org/node/3087606) Исправлена неполадка в `Datetime::getInfo()` которая кешировала временную зону пользователя, что в итоге приводило к непредсказуемым timestamp.
- [#3108287](https://www.drupal.org/node/3108287) Тест `UpdateTest::testPrimaryKeyUpdate()` был удалён по причине того что уже имеется более проработанный вариант `testMultiUpdate()`.
- [#3108021](https://www.drupal.org/node/3108021) Улучшен процесс определения [темы оформления](../../../../8/themes/index.md) по умолчанию для [установочных профилей](../../../../8/distributions/index.md), теперь он корректно определяет тему при установке из `config/sync`.
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
- [#3098707](https://www.drupal.org/node/3098707) В `WorkspaceManagerInterface` добавлен метод с документацией `purgeDeletedWorkspacesBatch`.
- [#2851204](https://www.drupal.org/node/2851204) Исправлено описание свойства `#size` для рендер элемента `select`.
- [#3104421](https://www.drupal.org/node/3104421) В ядре заменены все использования `implode` с аргументами в обратном порядке, так как данная возможность помечена устаревшей в PHP 7.4.
- [#3098244](https://www.drupal.org/node/3098244) Тест `SafeMarkupKernelTest` переименован в `FormattableMarkupKernelTest`.
- [#3102903](https://www.drupal.org/node/3102903) Внесены изменения в тест `MigrateExecutableMemoryExceededTest` для совместимости с PHPUnit 8.
- [#3087486](https://www.drupal.org/node/3087486) Для `PagerManagerInterface` улучшена документация и дополнен пример.
- [#3065166](https://www.drupal.org/node/3065166) Проведена модернизация и рефакторинг теста `ConnectionUnitTest`.
- [#3099971](https://www.drupal.org/node/3099971) Исправлен URI для `WorkflowListBuilder` который некорректно отрабатывал при установке Drupal в webroot.
- [#3100141](https://www.drupal.org/node/3100141) Исправлена ссылка для Automated Cron ведущая на документацию Drupal 7 вместо Drupal 8.
- [#3103913](https://www.drupal.org/node/3103913) Внесены улучшения в тесты `testAddHandler` и `testAddHandlerWithEntityField` для `ViewExecutableTest`.
- [#3096241](https://www.drupal.org/node/3096241) Произведён рефакторинг `image` и `file` виджетов полей. Теперь они ведут себя одинаково независимо от темы и не повторяют код друг друга.
- [#3100190](https://www.drupal.org/node/3100190) Внесены изменения в тест `ValidateMigrationStateTestTrait` для того чтобы тестировалась только одна версия в один момент.
- [#2620854](https://www.drupal.org/node/2620854) Актуализирована документация для `links.html.twig` и всех его вариантов.
- [#2936105](https://www.drupal.org/node/2936105) Константа `DRUPAL_PHP_FUNCTION_PATTERN` помечена устаревшей. На замену предлагается использовать `\Drupal\Core\Extension\ExtensionDiscovery::PHP_FUNCTION_PATTERN`.
- [#3102899](https://www.drupal.org/node/3102899) Исправлено использование мок-аргумента в `ViewExecutableTest` для совместимости с PHPUnit8.
- [#3101787](https://www.drupal.org/node/3101787) Изменения из [#2849628](https://www.drupal.org/node/3101787) также перенесены в модуль Views UI.
- [#3102329](https://www.drupal.org/node/3102329) В административной теме оформления Claro удален `transition` для `border-color` CKEditor.
- [#3097327](https://www.drupal.org/node/3097327) Исправлена ошибка в миграции `d7_node_title_label` приводящая к некорректной генерации значения `base_field_override`.
- [#2946889](https://www.drupal.org/node/2946889) Фильтры, которые заменяются на `filter_null` в процессе миграции теперь удаляются настройки, так как могли приводить к зависанию и ошибкам.
- [#3096969](https://www.drupal.org/node/3096969) Исправлена ошибка в модуле `migrate_drupal` которая приводила к обработке строки даже если там нет данных.
- [#3095195](https://www.drupal.org/node/3095195) Добавлена автоматическая корректировка даты при миграции значений типа поля `date` из старых инсталляций Drupal 7. Так как ранее могли сохраняться даты с числом и месяцем равным 00, что приводит к ошибкам на Drupal 8.
- [#3095146](https://www.drupal.org/node/3095146) Для миграции `date` поля из Drupal 7 улучшена проверка настроек точности, так как она может иметь разные структуры.
- [#3101556](https://www.drupal.org/node/3101556) Доработана фикстура `.eslintrc.json` для тестов Scaffold плагина Composer.
- [#3100496](https://www.drupal.org/node/3100496) Доработано Dependency Injection для `WorkspacesServiceProvider`, которое корректно обрабатывает переход синонимов на сущности и загружает нужный сервис.
- [#3101720](https://www.drupal.org/node/3101720) Внесены изменения в тест `FormStateDecoratorBaseTest` для совместимости `phpspec/prophecy` 1.10.0.
- [#2882031](https://www.drupal.org/node/2882031) Для Display плагинов Views улучшена проверка значения identifier которая исправляет ошибку «Undefined index: identifier in view's DisplayPluginBase->isIdentifierUnique()».
- [#2930283](https://www.drupal.org/node/2930283) Улучшено форматирование для backtrace значения в `DbLogController`.
- [#3018148](https://www.drupal.org/node/3018148) Для Views Bulk форм добавлены проверки прав на выполнение операций. Если доступа нет, теперь не будет происходить редиректа с ошибкой, а просто будет прерываться выполнение.
- [#3096811](https://www.drupal.org/node/3096811) Constraint Validator объекты теперь инициализируются на каждую проверку, чтобы избежать некорректных результатов.
- [#3092714](https://www.drupal.org/node/3092714) `ConfigEntityUpdater` теперь вызывается только для одного типа конфиг сущности за один запуск `hook_update_N()`. При попытке обработать более одного типа конфиг сущности будет вызвано исключение.
- [#3087061](https://www.drupal.org/node/3087061) Добавлены два новых теста `IdConflictTest` для миграций Drupal 6 и Drupal 7.
- [#3043467](https://www.drupal.org/node/3043467) Улучшены стили для Off Canvas форм, которые ломали оформление множественных селектов на браузерах основанных на движках WebKit и Mozilla.
- [#3098922](https://www.drupal.org/node/3098922) Исправлен комментарий с «reusable» на «not reusable» для `block_content_query_entity_reference_alter()`.
- [#3086238](https://www.drupal.org/node/3086238) Теперь при миграции проверяется первый ID из маппинга, он должен быть типа `integer`. Если он другого типа, будет вызвано исключение.
- [#3096609](https://www.drupal.org/node/3096609) Контрибным тест модулям разрешено не указывать значения для `core_version_requirement` и `core`. Они будут подставлены автоматически из используемого ядра для теста.
- [#3097765](https://www.drupal.org/node/3097765) Исправлена неполадка в модуле Views которая возникала если машинное имя бандла сущности состоит только из чисел.
- [#3009854](https://www.drupal.org/node/3009854) Улучшены пометки об устаревшем коде для `FileEntityNormalizer`.
- [#3100071](https://www.drupal.org/node/3100071) Исправлены некорректные упоминания `\Drupal\Update` на `\Drupal\update`.
- [#3099441](https://www.drupal.org/node/3099441) Исправлена ошибка в `seven_form_media_library_add_form_oembed_alter()` где некорректно передавался класс в рендер массиве.
- [#3098814](https://www.drupal.org/node/3098814) Исправлена ошибка «Class 'Drupal\Core\Controller\ArgumentResolver\RawParameterValueResolver' not found» при обновлении на Druapl 8.8.0.
- [#3093752](https://www.drupal.org/node/3093752) Исправлена ошибка в тесте `ResourceTestBase` которая вызывала метод `removeResourceTypeFromDocument` с более несуществующим параметром.
- [#2956722](https://www.drupal.org/node/2956722) Убрано повторное экранирование символов для метки сортировки Views.
- [#3093089](https://www.drupal.org/node/3093089) Добавлены небольшие улучшения для Help Topic блоков.
- [#3005403](https://www.drupal.org/node/3005403) Исправлена ошибка которая не позволяла редактировать или удалять блоки из макета `layout_builder`.
- [#3090904](https://www.drupal.org/node/3090904) Улучшены стили для Workspace тулбара чтобы они выглядели одинаково на разных административных темах.

## Ссылки

- [Drupal 8.8.2](https://www.drupal.org/project/drupal/releases/8.8.2) (англ.), drupal.org, 2 февраля 2020
