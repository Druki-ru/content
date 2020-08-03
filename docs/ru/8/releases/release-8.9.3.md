---
id: release-8.9.3
title: 'Drupal 8.9.3'
path: /8/releases/8.9.3
core: 8
metatags:
  title: 'Drupal 8.9.3: Список изменений'
  description: 'Список изменений Drupal 8.9.3.'
---

> [!IMPORTANT]
> Данный релиз находится в разработке. Актуальная версия [Drupal 8.9.2](release-8.9.2.md).

## Block

- [#3091309](https://www.drupal.org/project/drupal/issues/3091309) `broken` плагин не объявляет контексты, поэтому он больше является context-aware чтобы не выходило исключение.

## CKeditor

- [#3155110](https://www.drupal.org/project/drupal/issues/3155110) CKEditor обновлён до версии 4.14.1.

## Comment

- [#2855068](https://www.drupal.org/project/drupal/issues/2855068) Исправлен метод получения настроек поля для комментариев. Теперь данный тип поля может быть объявлен как базовое поле сущности.

## Composer

- [#3153869](https://www.drupal.org/project/drupal/issues/3153869) Удалены оставшиеся настройки `wikimedia/composer-merge-plugin`.
- [#3159730](https://www.drupal.org/project/drupal/issues/3159730) `composer/installers` обновлён до версии 1.9.0. Это позволит управлять зависимости при помощи Composer 2.
- [#3162479](https://www.drupal.org/project/drupal/issues/3162479) Исправлены неправильные указания с `Drupal\Composer\VendorHardening` на `Drupal\Composer\Plugin\VendorHardening`.

## Config

- [#2728507](https://www.drupal.org/project/drupal/issues/2728507) В форму импорта одиночной конфигурации добавлена валидация что выбрана конфигурация, в дополнение к HTML5 валидации.

## Content Moderation

- [#3112916](https://www.drupal.org/project/drupal/issues/3112916) Добавлен джоин по Entity ID, что сделало запросы быстрее.
- [#3040361](https://www.drupal.org/project/drupal/issues/3040361) Фильтр состояния теперь также работает со связанными таблицами сущности.

## Content translations

- [#2521782](https://www.drupal.org/project/drupal/issues/2521782) Исправлена неполадка, из-за которой генерировались некорректные `hreflang` ссылки на неопубликованные переводы.

## Database System

- [#3151975](https://www.drupal.org/project/drupal/issues/3151975) `NodeRevisionsTest` теперь использует EntityQuery.
- [#3159982](https://www.drupal.org/project/drupal/issues/3159982) SQL запрос в `\Drupal\Core\Database\Driver\mysql\Schema::getComment` теперь использует `AS` вместо `as`.

## Datetime

- [#3157369](https://www.drupal.org/project/drupal/issues/3157369) Неиспользуемая переменная `$filters` в `DateTimeSchemaTest` теперь используется.

## Entity System

- [#3154125](https://www.drupal.org/project/drupal/issues/3154125) Тип данных, возвращаемый методом `ContentEntityFormInterface::validateForm()` изменён на корректный `\Drupal\Core\Entity\ContentEntityInterface` вместо `\Drupal\Core\Entity\ContentEntityTypeInterface`.
- [#3154858](https://www.drupal.org/project/drupal/issues/3154858) `Drupal\Core\Config\Entity\Query\Condition::notExists()` теперь также проверяет родительское свойство на существование.
- [#2942569](https://www.drupal.org/project/drupal/issues/2942569) Исправлена сортировка свойств в запросах конфигурационных сущностей.
- [#3039991](https://www.drupal.org/project/drupal/issues/3039991) Исправлена неполадка, из-за которой удаление базовых полей мультиязычных сущностей приводила к дальнейшей невозможности удаления модуля, который добавлял это поле.

## Field System

- [#3159382](https://www.drupal.org/project/drupal/issues/3159382) Для представления `test_view_fieldapi` добавлена явная сортировка по `nid`, чтобы тесты проходили на всех БД.
- [#3159739](https://www.drupal.org/project/drupal/issues/3159739) Исправлено сравнение строки с блобом в `EntityDisplayTest`.
- [#3089495](https://www.drupal.org/project/drupal/issues/3089495) Теперь значение «No» для `BooleanCheckboxWidget` является переводимой.

## Layout Builder

- [#3161300](https://www.drupal.org/project/drupal/issues/3161300) Улучшено покрытие тестами для `\Drupal\Tests\layout_builder\Unit\SectionTest::testUnsetThirdPartySetting()`.

## Locale

- [#2988960](https://www.drupal.org/project/drupal/issues/2988960) Значение `default_server_pattern` для тестов теперьи меет актуальное значение.

## Link

- [#3157919](https://www.drupal.org/project/drupal/issues/3157919) Удалена неиспользуемая переменная `$node`.

## Media

- [#3089745](https://www.drupal.org/project/drupal/issues/3089745) Улучшена работа фокуса виджета Media Library когда используется максимальное количество элементов (так как кнопка добавления и сопутствующие js, отсутствует).
- [#3122051](https://www.drupal.org/project/drupal/issues/3122051) Поле `name` теперь отображается только если поддерживается oEmbed провайдером.

## Menu Link Content

- [#3016038](https://www.drupal.org/project/drupal/issues/3016038) `\Drupal\menu_link_content\MenuLinkContentAccessControlHandler::checkAccess` теперь всегда возвращает объект реализующий `\Drupal\Core\Access\AccessResultInterface` в качестве результата.

## Migration System

- [#3155463](https://www.drupal.org/project/drupal/issues/3155463) Исправлена опечатка `emtity`. Фраза `linktitle` убрана из словаря, так как правописание корректное.
- [#3153791](https://www.drupal.org/project/drupal/issues/3153791) Добавлена таблица с комментариями для `et` типа содержимого (Drupal 7 фикстура).
- [#3126063](https://www.drupal.org/project/drupal/issues/3126063) Миграции форматов текста более не будут проваливаться если фильтр используется исключительно для трансформации.
- [#3151360](https://www.drupal.org/project/drupal/issues/3151360) В форме `CredentialFrom` улучшены описания для файловых путей.
- [#2912244](https://www.drupal.org/project/drupal/issues/2912244) Добавлена отсутствующая документация для `MigrateIdMapInterface`.
- [#3160031](https://www.drupal.org/project/drupal/issues/3160031) `destinationproperty` заменён на `destination_property`.

## MySQL драйвер

- [#3155563](https://www.drupal.org/project/drupal/issues/3155563) Добавлена экранирование алиасов, которые совпадают с зарезирвированными словами MySQL.

## Node System

- [#3146016](https://www.drupal.org/project/drupal/issues/3146016) В представление `test_node_revision_uid` добавлена конкретная сортировка, так как она ожидается при сравнении, но не была задана.
- [#2348203](https://www.drupal.org/project/drupal/issues/2348203) Право доступа на создание новых материалов теперь контролируется `hook_ENTITY_TYPE_create_access()` вместо `hook_node_access()`.

## Path

- [#3155765](https://www.drupal.org/project/drupal/issues/3155765) Исправлена опечатка в методе теста `AliasManagerTest`.

## System

- [#3084916](https://www.drupal.org/project/drupal/issues/3084916) Добавлена новая JavaScript функция `tableDragToggle()` для `Drupal.theme` которая отвечает за разметку кнопки сортировки.

## Taxonomy

- [#3160169](https://www.drupal.org/project/drupal/issues/3160169) Удалена неиспользуемая переменная `$a` в `Drupal\taxonomy\Plugin\Validation\Constraint\TaxonomyTermHierarchyConstraintValidator::validate()`.

## Transliteration System

- [#3151364](https://www.drupal.org/project/drupal/issues/3151364) Добавлены различные вариации написания `Æ` для корректной транслитерации.

## Typed Data System

- [#3142893](https://www.drupal.org/project/drupal/issues/3142893) Исправлена неполадка приводящая к утечке памяти.

## Views

- [#2801929](https://www.drupal.org/project/drupal/issues/2801929) Исправлена неполадка в запросе, из-за которой при добавления поля комментария пропадала статистика по комментариям из вывода.
- [#3157933](https://www.drupal.org/project/drupal/issues/3157933) Удалена неиспользуемая переменная `$new_block_title`.
- [#3161199](https://www.drupal.org/project/drupal/issues/3161199) Удалено свойство `Drupal\views\Plugin\views\filter\BooleanOperator::no_operator`.
- [#3074595](https://www.drupal.org/project/drupal/issues/3074595) `InOperator::validate()` теперь передаёт второй параметр в `var_export()`, чтобы передавался массив, а не строка.

## Views UI

- [#3157462](https://www.drupal.org/project/drupal/issues/3157462) Исправлена ошибка в комментарии для конструктора.

## Workspaces

- [#3092551](https://www.drupal.org/project/drupal/issues/3092551) Исправлено некорректное отображение области над тулбаром, которое выглядела как активная область, но таковой не являлась.

## Тестирование

- [#3158292](https://www.drupal.org/project/drupal/issues/3158292) Из теста `FormAjaxResponseBuilderTest` удалены неиспользуемые переменные.
- [#3156070](https://www.drupal.org/project/drupal/issues/3156070) Из теста `ConfigSchemaTest` удалены неиспользуемые переменные.
- [#3155796](https://www.drupal.org/project/drupal/issues/3155796) Из теста `NodeRevisionsUiBypassAccessTest` удалена неиспользуемая переменная.
- [#3156345](https://www.drupal.org/project/drupal/issues/3156345) Из теста `PathProcessorTest` удалена неиспользуемая переменная.
- [#3158281](https://www.drupal.org/project/drupal/issues/3158281) Из теста `ScaffoldTest` удалена неиспользуемая переменная.
- [#3158276](https://www.drupal.org/project/drupal/issues/3158276) Из теста `RequestFormatRouteFilterTest` удалена неиспользуемая переменная.
- [#3123120](https://www.drupal.org/project/drupal/issues/3123120) Метод `AssertLegacyTrait::pass` помечен устаревшим. Ядро больше не использует его.
- [#3156040](https://www.drupal.org/project/drupal/issues/3156040) В методах `AccessManagerTest::testCheckNamedRouteWithUpcastedValues()` и `AccessManagerTest::testCheckNamedRouteWithDefaultValue()` удалена бесполезная инициализации переменной `$map`.
- [#3158270](https://www.drupal.org/project/drupal/issues/3158270) Из теста `SelectComplexTest` удалены неиспользуемые переменные.
- [#3153264](https://www.drupal.org/project/drupal/issues/3153264) Прекращено использование `t()` в тестах `::clickViewsOperationLink()`, `::helperButtonHasLabel()` и `::optionExists()`.
- [#3142749](https://www.drupal.org/project/drupal/issues/3142749) Исправлены вызовы `AssertLegacyTrait::assertPattern()` где до сих пор передавался аргумента для `$message`.

## Прочие изменения

- [#2848367](https://www.drupal.org/project/drupal/issues/2848367) Исправлен пример в обзоре Render API.
- [#3153565](https://www.drupal.org/project/drupal/issues/3153565) Упоминания `Drupal\Core\Pager\RequestPagerInterface` заменены на `Drupal\Core\Pager\PagerParametersInterface`.
- [#3156123](https://www.drupal.org/project/drupal/issues/3156123) Исправлены ошибки в документации `MissingContentEvent` и `ConfigEvents::IMPORT_MISSING_CONTENT`.
- [#3154914](https://www.drupal.org/project/drupal/issues/3154914) Исправлены ошибки грамматические ошибки в документации.
- [#3156883](https://www.drupal.org/project/drupal/issues/3156883) Добавлена проверка что фрагмент URL не является пустой строкой.
- [#3156882](https://www.drupal.org/project/drupal/issues/3156882) `Drupal\Core\Render\Element\StatusReport::preRenderGroupRequirements()` и `Drupal\user\PermissionHandler::sortPermissions()` теперь используют spaceship оператор (`<=>`).
- [#3159102](https://www.drupal.org/project/drupal/issues/3159102) Исправлена ошибка в документации `Drupal\serialization\RegisterEntityResolversCompilerPass`.
- [#3138749](https://www.drupal.org/project/drupal/issues/3138749) Исправлены опечатки в слове «cache».
- [#3159531](https://www.drupal.org/project/drupal/issues/3159531) Исправлены опечатки: «attibute», «uneccesarilly», «colletion», «constucts», «worklow».
- [#3159528](https://www.drupal.org/project/drupal/issues/3159528) Исправлены опечатки: «exeption», «gaurd», «ouptut», «withut», «defintion».
- [#3085751](https://www.drupal.org/project/drupal/issues/3085751) Улучшена проверка, которая позволяет модулям в обновлениях описывать новые зависимости и использовать их [сервисы](../services/services.md) как зависимость для своих без исключения во время обновления.
- [#3155462](https://www.drupal.org/project/drupal/issues/3155462) Для блока «Сделано на Drupal» удален аттрибут `role="complementary"`.
- [#3160124](https://www.drupal.org/project/drupal/issues/3160124) Исправлены опечатки: «wiget», «escapeable», «PHPunit».
- [#3160020](https://www.drupal.org/project/drupal/issues/3160020) Исправлены опечатки: «iids», «twoa», «twob», «roota», «rootb», «parentc».
- [#3117396](https://www.drupal.org/project/drupal/issues/3117396) Предупреждение о том что Pathauto версии меньше 1.5 не совместим с ядром больше не отображается на -dev версии ядра.
- [#2994319](https://www.drupal.org/project/drupal/issues/2994319) Для элемента формы `entity_autocomplete` добавлена более подробная документация и пример использования.
- [#3158589](https://www.drupal.org/project/drupal/issues/3158589) Улучшена документация для настройки переопределения конфигураций для разработки в `default.settings.php`.
- [#3161301](https://www.drupal.org/project/drupal/issues/3161301) Исправлена опеачтка «existant».
- [#3138766](https://www.drupal.org/project/drupal/issues/3138766) Исправлены опечатки и употребление «Don't».
- [#2875807](https://www.drupal.org/project/drupal/issues/2875807) Тайпхинт для параметра `$text` в `Drupal::l()` и `Link::fromTextAndUrl()` обновлён до актуального значения `string|array|\Drupal\Component\Render\MarkupInterface`.
- [#3156879](https://www.drupal.org/project/drupal/issues/3156879) `\Drupal\Component\Utility\Bytes::toInt()` теперь приндутильно преобразует значение переменной `$size` в `float`.