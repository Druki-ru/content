---
title: 'Drupal 8.7.2'
slug: 8/releases/8.7.2
core: 8
metatags:
  title: 'Drupal 8.7.2: Список изменений'
  description: 'Исправление критических ошибок и прочие изменения.'
---

**Дата релиза**: 23 мая 2019 г.

Это патч-релиз Drupal 8, готовый к использованию на продакшен сайтах.

## Исправления критических ошибок

- [#3052271](https://www.drupal.org/project/drupal/issues/3052271) Исправлена ошибка обновления `media_library_update_8701()` при обновлении с Drupal 8.6.15.
- [#3052147](https://www.drupal.org/project/drupal/issues/3052147) Исправлена ошибка обновления `comment_update_8701()`, если комментарии не имеют поля `field_name`.
- [#3052467](https://www.drupal.org/project/drupal/issues/3052467) Исправлена ошибка обновления `system_update_8702()`, при которой происходила ошибка "Call to a member function getKey() on null".
- [#3053552](https://www.drupal.org/project/drupal/issues/3053552) Библиотека `typo3/phar-stream-wrapper` была обновлена до версии 2.1.2, где была удалена зависимость "fileinfo", приводящая к ошибкам.
- [#3052431](https://www.drupal.org/project/drupal/issues/3052431) Исправлена ошибка в `layout_builder_post_update_make_layout_untranslatable()`, при которой, функция по прежнему пыталась получить все ревизии сущности без поддержки ревизий.

## Прочие изменения

- Откачены изменения [#2822778](https://www.drupal.org/node/2822778), которые исправляли поведение модальных окон Views при активном трее тулбара. Так как оно приводит к новым проблемам с модальными окнами, самым безболезненным выходом было откатить прошлые изменения и начать поиск нового решения.
- [#3030363](https://www.drupal.org/node/3030363) Добавлен тест для проверки возможности нажатия AJAX кнопки пробелом.
- [#3053129](https://www.drupal.org/node/3053129), [#3050448](https://www.drupal.org/node/3050448) Исправлены ссылки на документацию Twig, которые были изменены и теперь отдают 404.
- [#3052971](https://www.drupal.org/node/3052971) Исправлено уведомление "Notice: Undefined index: #include_fallback in Drupal\Core\Render\Element\StatusMessages::generatePlaceholder()".
- [#3034599](https://www.drupal.org/node/3034599) Исправлена ошибка для PHP 7 "Undefined class constant 'SOURCE_IDS_HASH'".
- [#3001299](https://www.drupal.org/node/3001299) Исправлена ошибка "Uncaught exception 'Error' with message 'Call to a member function wasDefaultRevision() on null'", происходящая при попытке загрузить перевод сущности, для языка, который не представлен в текущей версии сущности.
- [#3055495](https://www.drupal.org/node/3055495) Тесты поисковой страницы для миграций перенесены в соответствующие неймспейсы.
- [#3042124](https://www.drupal.org/node/3042124) Исправлена ошибка, приводящая к пустому телу ответа, когда пользователь является администратором и произошла ошибка на сайте.
- [#3024460](https://www.drupal.org/node/3024460) Добавлена миграция языковых настроек типов комментариев из Drupal 7.
- [#2884052](https://www.drupal.org/node/2884052) Исправлена ошибка для `managed_file` элемента, при которой загрузка множественных файлов приводила к нажатию кнопки "удаления" ранее загруженных файлов в элемент.
- [#3049437](https://www.drupal.org/node/3049437) Исправлена ошибка, при которой `SharedTempStore` не сериализуется на PHP 7.3.
- [#3054392](https://www.drupal.org/node/3054392) Добавлена отсутствующая документация для методов `LocalAwareRedirectResponseTrait`.
- [#3040166](https://www.drupal.org/node/3040166) Исправлена проблема, при которой состояние гонки в `BrowserHtmlDebugTrait.php:141` приводило к ошибкам `mkdir()`.
- [#3053529](https://www.drupal.org/node/3053529) Исправлена ошибка, при которой невозможно включить переопределение лейаута, если для другого подтипа сущности выбран полный вариант отображения.
- [#3050275](https://www.drupal.org/node/3050275) Заменены оставшиеся отсылки к PSR-0 на PSR-4.
- [#3052059](https://www.drupal.org/node/3052059) Исправлена некорректная отсылка в документации к `StringFilter`.
- [#3049685](https://www.drupal.org/node/3049685) Тесты `MigrateNodeRevisionTest` и `NodeMigrateDeriverTest` перенесены в `Kernel` неймспейс.
- [#3050172](https://www.drupal.org/node/3050172) Тест `MigrateLanguageTest` перенесен в `Kernel` неймспейс.
- [#2923428](https://www.drupal.org/node/2923428) Использование свойства status для файла `$file->status !== FILE_STATUS_PERMANENT` было заменено на соответствующий метод `$file->isTemporary()` в модуле editor.
- [#3051889](https://www.drupal.org/node/3051889) Исправлена ошибка в документации к `hook_update_N()`, где для `$sandbox['max']` также производилось вычитание `-1`, которое не требуется и приводит к рекурсии.
- [#3045148](https://www.drupal.org/node/3045148), [#3045148](https://www.drupal.org/node/3045148) Исправлено отображение вкладки Workspaces в тулбаре для мобильных устройств.
- [#3037668](https://www.drupal.org/node/3037668) Окно виджета Media Library улучшено в соответствии с общей визуальной согласованностью (относительно позиционирование, визуальные вес, отступы, положение и т.д.).
- [#3052625](https://www.drupal.org/node/3052625) Исправлена ошибка, при которой `DeprecationListenerTrait` считал ошибкой отсутствие результатов.
- [#2859297](https://www.drupal.org/node/2859297) Добавлена миграция связей с терминами таксономии для переводов содержимого.
- [#3052492](https://www.drupal.org/node/3052492) Исправлена ошибка, при которой `ViewsEntitySchemaSubscriber` пересохранял все Views принадлежащие обновляемому типу сущности, но при этом, представления не были изменены. Из-за некорректного объявления зависимостей хендлерами представлений, это может приводить к поломке представлений и необходимости ручного исправления.
- [#3050429](https://www.drupal.org/node/3050429) Исправлены опечатки "lable" в `layout_builder.schema.yml` и `views.schema.yml`.
- [#3046694](https://www.drupal.org/node/3046694) Тест `BookInstallTest` перенесен в `Kernel` неймспейс.
- [#3042882](https://www.drupal.org/node/3042882) Исправлены ошибки в README.txt.
- [#3047757](https://www.drupal.org/node/3047757) Исправлено дублирование "a" в документации к `ViewsHandlerInterface::broken()`.
- [#3048885](https://www.drupal.org/node/3048885) Примеры с массивами для [settings.php](../../../settings-php/index.md) заменены на короткий формат.
- [#3027318](https://www.drupal.org/node/3027318) Улучшено покрытие тестов для Inline Form Errors.
- [#3034885](https://www.drupal.org/node/3034885) Исправлен приоритет сервиса `paramconverter.configentity_admin`.
- [#2981584](https://www.drupal.org/node/2981584) Исправлена проблем, при которой Big Pipe "падал" при большом количестве плейсхолдеров на странице (1000+, no_js). Логика работы с плейсхолдерами улучшена и теперь, при большом количестве они будут обрабатываться иначе (более медленный, но более надежный: вместо `preg_split()` будет использоваться `str_replace()` + `array_filter()`).
- [#3049604](https://www.drupal.org/node/3049604) Тест `CommentFieldNameTest` перенесен в `Kernel` неймспейс.
- [#3041842](https://www.drupal.org/node/3041842) Тест `ArgumentValidateTest` перенес в `Kernel` неймспейс.
- [#2970912](https://www.drupal.org/node/2970912) Добавлена отсутствующая схема для значения по умолчанию поля Telephone.
- [#2969691](https://www.drupal.org/node/2969691) Исправлена ошибка, из-за которой для видеофайлов не добавлялся атрибут `muted` если включена соответствующая опция.
- [#3046937](https://www.drupal.org/node/3046937) Тест `TrackerUserUidTest` перенесен в `Kernel` неймспейс. Также `TrackerTestBase` помечен устаревшим.
- [#3052140](https://www.drupal.org/node/3052140) Исправлена ошибка, из-за которой было невозможно конвертировать тип сущности без поддержки ревизий, в сущность в поддержкой ревизий.
- [#3049938](https://www.drupal.org/node/3049938) Исправлены ссылка на ишью для устаревшего `KernelTestBase`.
- [#3055990](https://www.drupal.org/node/3055990) В тестах `WebDriverTestBase` отключены анимации. Если они вам нужны, вы можете установить `FALSE` для свойства `$disableCssAnimations`.

## Ссылки

- [Drupal 8.7.2](https://www.drupal.org/project/drupal/releases/8.7.2) (англ.), drupal.org, 23 мая 2019
