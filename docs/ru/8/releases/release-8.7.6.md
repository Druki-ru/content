---
id: release-8.7.6
title: 'Drupal 8.7.6'
path: /8/releases/8.7.6
core: 8
metatags:
  title: 'Drupal 8.7.6: Список изменений'
  description: 'В данном релизе исправлены различные ошибки и внесены улучшения.'
---

**Дата релиза**: 8 августа 2019 г.

- [#2825516](https://www.drupal.org/node/2825516) Исправлена ошибка при которой `hook_node_access()` вызывался с `$op = 'delete'` при генерации формы создания материала.
- [#3029627](https://www.drupal.org/node/3029627) `FormatterBase` теперь включает в себя third party settings.
- [#2973137](https://www.drupal.org/node/2973137) Исправлена ошибка при которой `EntityViewsData` не проводил валидацию ревизий.
- [#3067041](https://www.drupal.org/node/3067041) Исправлена ошибка, когда при передаче некорректного `entity_id` для Media Library, происходила поломка пагинации.
- [#3070604](https://www.drupal.org/node/3070604) Улучшена документация для аргумента `FormBuilder::buildForm()`.
- [#3070880](https://www.drupal.org/node/3070880) Актуализирована документация по сборке CKEditor для Drupal.
- [#3066439](https://www.drupal.org/node/3066439) `EntityStorageInterface::restore()` больше не вызывает `$entity->preSave()`.
- [#3068661](https://www.drupal.org/node/3068661) Исправлено описание для `AccessResult`.
- [#2780475](https://www.drupal.org/node/2780475) Улучшены проверки `JavascriptTestBase::assertEscaped()` и `JavascriptTestBase::assertUnescaped()`.
- [#3060550](https://www.drupal.org/node/3060550) Улучшена обработка URL параметра для OEmbed ссылки в query параметре.
- [#1476782](https://www.drupal.org/node/1476782) В методе `DatabaseStatementPrefetch::current()` `array_unshift()` заменан на `array_shift()`.
- [#3065212](https://www.drupal.org/node/3065212) Улучшен запрос для материалов Workspaces на мультиязычных сайтах.
- [#3063020](https://www.drupal.org/node/3063020) Названия стилей CKEditor теперь являются переводимыми.
- [#2863986](https://www.drupal.org/node/2863986) Улучшен процесс обновления модулей. Теперь он не будет прерываться, если у модуля появилась новая зависимость в виде сервиса, о котором на момент обновления не знает контейнер.
- [#3065663](https://www.drupal.org/node/3065663) Исправлены ошибки в некоторых post-update функциях, приводящие к ошибкам, из-за отсутствия дополнительных проверок на зависимости.
- [#3062157](https://www.drupal.org/node/3062157) Добавлена документация переменной `summary_attributes` в шаблонах `details.html.twig`.
- [#3065586](https://www.drupal.org/node/3065586) `ConditionManager::execute()` теперь проводит проверку на соответствие `ConditionInterface`.
- [#2996431](https://www.drupal.org/node/2996431) Улучшен тайпхинтинг для свойства `ConditionPluginBase::executableManager`.
- [#3050757](https://www.drupal.org/node/3050757) CKeditor обновлен до 4.11.4.

## Ссылки

- [Drupal 8.7.6](https://www.drupal.org/project/drupal/releases/8.7.6) (англ.), drupal.org, 8 августа 2019
