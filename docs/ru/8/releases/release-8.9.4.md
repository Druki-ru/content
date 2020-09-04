---
id: release-8.9.4
title: 'Drupal 8.9.4'
path: /8/releases/8.9.4
core: 8
metatags:
  title: 'Drupal 8.9.4: Список изменений'
  description: 'Список изменений Drupal 8.9.4.'
---

**Дата релиза**: 2 сентября 2020

## Asset Library System

- [#3167036](https://www.drupal.org/project/drupal/issues/3167036) Исправлена неполадка из-за которой пустой файл `*.libraries.yml` приводил к фатальной ошибке.

## Claro

- [#3127466](https://www.drupal.org/project/drupal/issues/3127466) Исправлено отображение «обязательного поля» для IE 11.

## Comment

- [#2614720](https://www.drupal.org/project/drupal/issues/2614720) (откачено) Исправлена неполадка привяодащая к фатальной ошибке при попытке загрузить/отрендерить «битые» (например, если комментарии по какой-то причине остались, а материал, к которому они принадлежат уже удалён) комментарии.

## Composer

- [#3130685](https://www.drupal.org/project/drupal/issues/3130685) Исправлена ошибка стандартов кодирования для генерируемого [drupal/core-composer-scaffold](../../composer/drupal-core-composer-scaffold.md) `autoload.php` файла.

## Content Moderation

- [#3112433](https://www.drupal.org/project/drupal/issues/3112433) Исправлена неполадка из-за которой были некорректные результаты при фильтрации по статусу материала и использовании группировки.

## Database System

- [#3160267](https://www.drupal.org/project/drupal/issues/3160267) Употребление статичных запросов в тестах заменено на динамические запросы.

## Field System

- [#3163687](https://www.drupal.org/project/drupal/issues/3163687) Удалена неиспользуемая переменная `$id` в `BulkDeleteTest`.

## Filter

- [#3151101](https://www.drupal.org/project/drupal/issues/3151101) Употребление «whitelist» и «blacklist» в Filter модуле заменено на более подходящие.

## JSON:API

- [#3025372](https://www.drupal.org/project/drupal/issues/3025372) Исправлена неполадка из-за которой при фильтрации мог быть пустой объект для связи.
- [#3122113](https://www.drupal.org/project/drupal/issues/3122113) Ссылки на ишьюсы связанные с JSON:API в документации к модулю заменены на новые, так как модуль больше не контрибный.

## Migration System

- [#3116841](https://www.drupal.org/project/drupal/issues/3116841) Исправлена фикстура для drupal6 миграций Node 1.

## RDF

- [#2978320](https://www.drupal.org/project/drupal/issues/2978320) `rdf_comment_storage_load()` больше не будет пытаться загружать `NULL` комментарии.
- [#1929420](https://www.drupal.org/project/drupal/issues/1929420) Модуль теперь использует `$account->getDisplayName()` для вывода имени пользователя пользователя.

## Render System

- [#3167196](https://www.drupal.org/project/drupal/issues/3167196) Исправлена документация для `Drupal\Core\Render\ElementEmail`.

## Settings tray

- [#3070375](https://www.drupal.org/project/drupal/issues/3070375) (откачено в [Drupal 8.9.5](release-8.9.5.md) и [Drupal 9.0.5](../../9/releases/release-9.0.5.md)) Исправлены стили из-за которых отображались скрытые кнопки в off-canvas диалоге.

## User

- [#3005641](https://www.drupal.org/project/drupal/issues/3005641) Исправлена неполадка из-за которой вызывалось исключение при попытке сменить язык для пользователя если сущность переводима.

## Views

- [#3101738](https://www.drupal.org/project/drupal/issues/3101738) Раскрытые фильтры для терминов таксономии больше не будут показывать пользователю те, доступа к которым он не имеет (например неопубликованные).

## Views UI

- [#3035732](https://www.drupal.org/project/drupal/issues/3035732) Улучшено автодополнение для административных тегов представления.

## Прочие изменения

- [#3163703](https://www.drupal.org/project/drupal/issues/3163703) Исправлена опечатка в `DeprecatedServicePropertyTrait`.
- [#3166645](https://www.drupal.org/project/drupal/issues/3166645) Удалена отсылка к функции `image_scale()` для документации `Image::scaleDimensions()`.
- [#3164678](https://www.drupal.org/project/drupal/issues/3164678) Исправлена ссылка на RFC5242.
- [#3167390](https://www.drupal.org/project/drupal/issues/3167390) `ExceptionLoggingSubscriber` больше не записывает в лог стэк обратного вызова для исключений связанных с отсутствие прав доступа (403).

## Ссылки

- [Drupal 8.9.4](https://www.drupal.org/project/drupal/releases/8.9.4) (англ.), drupal.org, 2 сентября 2020