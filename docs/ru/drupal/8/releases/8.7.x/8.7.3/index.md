---
title: 'Drupal 8.7.3'
slug: 8/releases/8.7.3
core: 8
metatags:
  title: 'Drupal 8.7.3: Список изменений'
  description: 'Исправление критических ошибок и прочие изменения.'
---

**Дата релиза**: 6 июня 2019 г.

Это патч-релиз Drupal 8, готовый к использованию на продакшен сайтах.

## Список изменений

- [#3057314](https://www.drupal.org/node/3057314) Улучшены сверки хэшей в ядре, которые были написаны не совсем надежно.
- [#3041326](https://www.drupal.org/node/3041326) Из `MenuSettingsConstraintValidator` убраны обязательные требования для "title" и "description".
- [#2939356](https://www.drupal.org/node/2939356) Исправлена ошибка при импорте перевода для конфигурации "workflows.workflow.editorial.yml".
- [#3054315](https://www.drupal.org/node/3054315) Откачено изменение сортировки `ApcuBackendTest` для 8.7.x из-за проблем совместимости с PHP 5.
- [#3048196](https://www.drupal.org/node/3048196) Исправлена ошибка, когда не переводилось поле заголовка для картинки профиля, если выбран метод определения языка "Страницы администрирования учётных записей".
- [#2994315](https://www.drupal.org/node/2994315) Изменены ограничения для зависимости "paragonie/random_compat", что позволяют использовать актуальную версию на PHP 7.0+.
- [#3057370](https://www.drupal.org/node/3057370) Исправлена проблема, когда при вызове `MediaLibraryState::fromRequest()` для запроса, где не представлен никакой аргумент Media Library, выходила ошибка.
- [#3058013](https://www.drupal.org/node/3058013) [@plach](https://www.drupal.org/u/plach) повышен до Framework Manager. Исправлено упоминание в MAINTAINERS.txt.
- [#2927012](https://www.drupal.org/node/2927012) Исправлена ошибка, при которой `_drupal_log_error()` возвращать код `exit` равный 0 при ошибках.
- [#3043907](https://www.drupal.org/node/3043907) Улучшена обработка исключения для `DatabaseCacheBackend::ensureBinExists()`.
- [#3023220](https://www.drupal.org/node/3023220) Предотвращение выполнения ненужного кода в Layout Builder, когда поля рендерятся изолировано (результаты Views, FieldBlock и т.д.).
- [#3046007](https://www.drupal.org/node/3046007) Исправлена ошибка, когда при некоторых ситуациях данные о поле не очищались при удалении bundle сущности, что приводило к невозможности применения обновлений.
- [#3056348](https://www.drupal.org/node/3056348) Исправлен комментарий для `NodeRevisionDeleteForm`, который по ошибке описывал, что форма откатывает ревизию, а не удаляет её.
- [#3051908](https://www.drupal.org/node/3051908) Исправлена ошибка в документации "json.api.php", которая описывала неправильный параметр запроса.
- [#3053330](https://www.drupal.org/node/3053330) Исправлен формат зависимостей модуля Workspaces для соответствия стандартам.
- [#3045211](https://www.drupal.org/node/3045211) Исправлена проблема, при которой миграции ссылок создавали некорректные атрибуты.
- [#3055474](https://www.drupal.org/node/3055474) `template_preprocess_file_link` теперь не пытается загрузить файл из `stdClass` объекта и всегда ожидает сущность файла.
- [#3055918](https://www.drupal.org/node/3055918) Исправлена опечатка в `LibraryDiscoveryParser::parseLibraryInfo()`.
- [#3053827](https://www.drupal.org/node/3053827) Исправлена ошибка утечки метаданных кэша при использовании JSON:API для получения комментария с родителем.
- [#3035980](https://www.drupal.org/node/3035980) Улучшено сообщение об ошибке при передаче `NULL` в `EntityStorageBase::load()`.
- [#3048434](https://www.drupal.org/node/3048434) Тест `FileManagedAccessTest` перенесен в пространство имен `Kernel`.
- [#2892440](https://www.drupal.org/node/2892440) Добавлен новый хелпер для тестов `JSWebAssert::assertNoElementAfterWait()`, который проверяет наличие элемента на странице спустя определенный тайм-аут, например, когда элемент должен был удалиться со страницы.
- [#3056536](https://www.drupal.org/node/3056536) Улучшен код теста `LayoutBuilderDisableInteractionsTest`, для решения проблемы, когда он мог непредсказуемо провалиться.
- [#3048707](https://www.drupal.org/node/3048707) Аргументы для Views AJAX теперь декодируют HTML.
- [#3052940](https://www.drupal.org/node/3052940) Исправлен тип параметра `$expected` в комментарии для `FilterHtmlTest::testfilterAttributes()`.
- [#3055001](https://www.drupal.org/node/3055001) Исправлена опечатка в `comment_help()`.
- [#3043087](https://www.drupal.org/node/3043087) Улучшена производительность для блоков с контекстами.
- [#2901792](https://www.drupal.org/node/2901792) Отключены все анимации в JavaScript тестах.

## Ссылки

- [Drupal 8.7.3](https://www.drupal.org/project/drupal/releases/8.7.3) (англ.), drupal.org, 6 июня 2019
