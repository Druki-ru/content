---
title: 'Drupal 8.8.7'
slug: wiki/drupal/releases/8.8.7
core: 8
metatags:
  title: 'Drupal 8.8.7: Список изменений'
  description: 'Патч релиз Drupal 8.8.7, список изменений.'
---

**Дата релиза**: 3 июня 2020

## Basic Auth

- [#3124281](https://www.drupal.org/node/3124281) Исправлены грамматически ошибки в документации `BasicAuth`.

## Comment

- [#3137713](https://www.drupal.org/node/3137713) (только для Drupal 8) Уведомления об устаревшем коде обновлены для конструктора `NodeNewComments`.

## Composer

- [#3078671](https://www.drupal.org/node/3078671) Зависимости Drupal `behat/mink` и `behat/mink-selenium2-driver` обновлены до стабильных релизов.
- [#3143722](https://www.drupal.org/project/drupal/issues/3143722) `symfony/http-foundation` обновлён до 3.4.35 (обновление безопасности).

## CKEditor

- [#3070745](https://www.drupal.org/node/3070745) `localStorage` теперь хранит только последние версии стилей, для избежания проблем лимита хранения данного хранилища.

## Field

- [#3113124](https://www.drupal.org/node/3113124) Исправлена ошибка в примере `hook_field_info_alter()`.

## JSON:API

- [#3126906](https://www.drupal.org/node/3126906) Исправлена ошибка в коде из-за которой файл `MenuLinkContentTest` распознавался как бинарный файл.

## Media Library

- [#3099528](https://www.drupal.org/node/3099528) Удалён дублирующий параграф в справке по модулю.

## Migrate

- [#3125763](https://www.drupal.org/node/3125763) Модуль `migrate_no_migrate_drupal_test` теперь имеет зависимость на `drupal:node`.

## Place Block

- [#3116399](https://www.drupal.org/node/3116399) (Только для Drupal 8) Модуль больше не будет вызывать ошибку что он устарел.

## Statistics

- [#3128761](https://www.drupal.org/node/3128761) Из запроса удалено дублирование заполнителя `:timestamp`.

## Taxonomy

- [#3101635](https://www.drupal.org/node/3101635) Исправлены опечатки в комментариях `taxonomy.es6.js` где было упоминание блоков вместо таксономии.

## Views

- [#2989745](https://www.drupal.org/node/2989745) Обновление конфигураций обновления `views_update_8500()` перенесены на процесс сохранения конфигурации для сохранения BC.

## Views UI

- [#3087465](https://www.drupal.org/node/3087465) Документация для хука `hook_views_ui_display_top_links_alter()` перенесена в `views_ui.api.php`.

## User

- [#3084813](https://www.drupal.org/node/3084813) Тайпхинт для `user_load()` был изменён с `object|bool` на `\Drupal\user\UserInterface|false`.

## Update

- [#2992631](https://www.drupal.org/node/2992631) Информация об обновлении больше не будет рекомендовать новую минорную версию при наличии обновления безопасности для текущей минорной.
- [#3120961](https://www.drupal.org/node/3120961) Из модулей для тестирования удалены `version: VERSION`, так как версии подставляются динамически во время тестирования.
- [#3111463](https://www.drupal.org/node/3111463) Улучшена документация кода для `Drupal\update\ProjectSecurityData`.
- [#3002820](https://www.drupal.org/project/drupal/issues/3002820) Добавлена проверка что данные являются массивом в `template_preprocess_update_report()`.

## Тестирование

- [#3126695](https://www.drupal.org/node/3126695) Возвращены методы `::assertEqualsCanonicalizing`, `::assertNotEqualsCanonicalizing`, `::assertEqualsCanonicalizing`, `::assertNotEqualsCanonicalizing` и `::testAssertEqualsCanonicalizing`, которые вызывают новые. Это изменение касается только Drupal 8. Тесты для Drupal 9 по-прежнему должны использовать новые методы.
- [#3126797](https://www.drupal.org/node/3126797) Возвращены методы `::assertStringContainsString`, `::assertStringContainsStringIgnoringCase`, `::assertStringNotContainsString`, `::assertStringNotContainsStringIgnoringCase`, `::assertStringContainsString`, `::assertStringContainsStringIgnoringCase`, `::assertStringNotContainsString`, `::assertStringNotContainsStringIgnoringCase`  и `::testAssertStringContainsString`, которые вызывают новые. Это изменение касается только Drupal 8. Тесты для Drupal 9 по-прежнему должны использовать новые методы.
- [#3122547](https://www.drupal.org/node/3122547) Исправлены опечатки при сравнении строк
- [#3132745](https://www.drupal.org/node/3132745) Свойство `$modules` приведено к стандартам. Там где оно превышает 80 символов, используется перенос.
- [#2978398](https://www.drupal.org/node/2978398) `UserPasswordResetTest` больше не расширяет `PageCacheTagsTestBase`.
- [#3131474](https://www.drupal.org/node/3131474) Сравнения, использующие `array_search()` заменены на `::assertContains()`, `::assertNotContains()`.
- [#3123253](https://www.drupal.org/node/3123253) Удалено использование `AssertLegacyTrait::pass()`.
- [#3126787](https://www.drupal.org/node/3126787) (только Drupal 8) Добавлены методы проверки типов для обратной с PHPUnit 6 и 7 через `::assertInternalType()`.
- [#3082602](https://www.drupal.org/node/3082602) Из `disable_transitions.theme.css` удалено правило отключения `tranform`.
- [#3121020](https://www.drupal.org/node/3121020) Фикстуры модуля `update_test` перенесены из корня модуля в `fixtures/release-history`.
- [#3134475](https://www.drupal.org/node/3134475) В тесте `CommentIntegrationTest` сравнение строки с blob значением теперь производится после запроса, а не внутри него.
- [#3126333](https://www.drupal.org/node/3126333) Использование параметра `$canonicalize` для `::assertEquals` заменено на `::assertEqualsCanonicalizing`.
- [#3134333](https://www.drupal.org/node/3134333) `SearchSimplifyTest` и `SearchTokenizerTest` теперь расширяют `KernelTest`.
- [#3135747](https://www.drupal.org/node/3135747) Исправлены проблемы в коде для обратной совместимости `::assertStringContainsString()`.
- [#3123933](https://www.drupal.org/node/3123933) `ComposerProjectTemplatesTest` больше не будет загружать пакеты из интернета.
- [#3135390](https://www.drupal.org/node/3135390) Использование `is_writable()` и `is_readable()` заменены на соответствующие стандартные методы `::assertDirectoryNotIsWritable()` и `::assertFileIsReadable()`.
- [#3139403](https://www.drupal.org/project/drupal/issues/3139403) Использование `::assertElementPresent()` и `::asertElementNotPresent()` заменены на `$this->assertSession()->elementExists()`.
- [#3139439](https://www.drupal.org/project/drupal/issues/3139439) Использование `::assertHeader()` заменено на `$this->assertSession()->responseHeaderEquals()`.
- [#3143339](https://www.drupal.org/project/drupal/issues/3143339) Аргументы для `WebAssert::titleEquals()` и `AssertLegacyTrait::assertTitle()` приведены к единому стилю.

## Прочие изменения

- [#3030989](https://www.drupal.org/node/3030989) Исправлена ошибка вызываемая при попытке массово удалить удалённые ноды.
- [#3074047](https://www.drupal.org/node/3074047) Исправлена и улучшена документация метода `MigrateDestinationInterface::import`. Теперь возвращаемый тип не `mixed`, а `array|bool`.
- [#3098475](https://www.drupal.org/node/3098475) Проверка на реализацию хука `hook_update_last_removed()` в процессе обновления баз данных стала более строгой и сообщение об ошибке более доступным.
- [#3121362](https://www.drupal.org/node/3121362) Различные исправления опечаток.
- [#3120901](https://www.drupal.org/node/3120901) Сообщения об устаревшем коде в 8.8.4 обновлены до 8.8.5, так как [Drupal 8.8.4](../8.8.4/index.md) — обновление безопасности.
- [#3130427](https://www.drupal.org/node/3130427) Исправлено значение в фикстуре `video_collegehumor.xml` приводящее к ошибке.
- [#3126957](https://www.drupal.org/node/3126957) Добавлены отсутствующие фигурные скобки для `@inheritdoc`.
- [#3120910](https://www.drupal.org/node/3120910) Сайты, у которых отсутствуют сведения о текущей схеме получат 8001 по умолчанию. Это позволит корректно запускаться обновлениям.
- [#3132287](https://www.drupal.org/node/3132287) Исправлены некорректные использования `{@inheritdoc}`.
- [#3020905](https://www.drupal.org/node/3020905) Удалён пример реализации метода `ModuleUninstallValidatorInterface::validate()` из документации метода.
- [#3100251](https://www.drupal.org/node/3100251) Исправлены некорректные неймспейсы для классов и интерфейсов в комментариях.
- [#3074064](https://www.drupal.org/node/3074064) Исправлен референс в `LoggerChannelFactoryInterface::addLogger()` на существующий.
- [#3063694](https://www.drupal.org/node/3063694) В документацию к классу `Url` добавлены примеры использования.
- [#3094067](https://www.drupal.org/node/3094067) Обновлены и добавлены отсутствующие `@param` и `@return` документации для `TypedDataInterface`.
- [#3110620](https://www.drupal.org/node/3110620) Исправлена документация `ModuleHandler::invokeAll()`.
- [#3134472](https://www.drupal.org/node/3134472) (Только Drupal 8.8) Убрана лишняя новая строка приводящая к ошибке при проверке на стандарты.
- [#3119733](https://www.drupal.org/node/3119733) Обновлён COPYRIGHT.txt.
- [#3137268](https://www.drupal.org/node/3137268) benjifisher добавлен в список меинтейнеров подсистемы Migrate.
- [#3136668](https://www.drupal.org/node/3136668) Теперь, сломанные и отсутствующие значения о модулях и обновлениях, которых больше нет, не будут приводить к фатальным ошибкам, а будут просто напоминать об этом, и пропускать обработку.
- [#3136302](https://www.drupal.org/node/3136302) Информация из UPDATE.txt заменена на актуальные ссылки по данным темам с drupal.org.
- [#3138731](https://www.drupal.org/node/3138731) Исправлены опечатки `inheritdoc` в ядре.
- [#3100712](https://www.drupal.org/project/drupal/issues/3100712) Добавлены дополнительные проверки на наличие обязательных значений для конфигураций при импорте.
- [#3110200](https://www.drupal.org/project/drupal/issues/3110200) Из документации убраны упоминания функции `filter_process_format()`, которой больше не существует.
- [#3134308](https://www.drupal.org/project/drupal/issues/3134308) В комментария к коду «is was» заменён на «is».
- [#3143115](https://www.drupal.org/project/drupal/issues/3143115) Улучшено форматирование README.txt.
- [#3138591](https://www.drupal.org/project/drupal/issues/3138591) (Только Drupal 8) Добавлены отсутствующие `E_USER_DEPRECATED` уведомления.
- [#3138775](https://www.drupal.org/project/drupal/issues/3138775) Исправлены опечатки в слове «Monoceros».
- [#3138785](https://www.drupal.org/project/drupal/issues/3138785) Исправлены опечатки в слове «Picasso».
- [#3138786](https://www.drupal.org/project/drupal/issues/3138786) Исправлены опечатки в слове «Protected».
- [#3138799](https://www.drupal.org/project/drupal/issues/3138799) Исправлены опечатки в слове «description».
- [#3138803](https://www.drupal.org/project/drupal/issues/3138803) Исправлены опечатки в слове «strength».
- [#3138802](https://www.drupal.org/project/drupal/issues/3138802) Исправлены опечатки в слове «snafus».
- [#3138792](https://www.drupal.org/project/drupal/issues/3138792) Исправлены опечатки в слове «compatibility».
- [#3138787](https://www.drupal.org/project/drupal/issues/3138787) Исправлены опечатки в слове «response».
- [#3138801](https://www.drupal.org/project/drupal/issues/3138801) Исправлены опечатки в слове «readily».
- [#3138793](https://www.drupal.org/project/drupal/issues/3138793) Исправлены опечатки в слове «configuration».

## Ссылки

- [Drupal 8.8.7](https://www.drupal.org/project/drupal/releases/8.8.7) (англ.), drupal.org, 3 июня 2020