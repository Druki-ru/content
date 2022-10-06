---
title: 'Drupal 9.4.8'
slug: wiki/drupal/releases/9.4.8
core: 9
metatags:
  title: 'Drupal 9.4.8: Список изменений'
  description: 'Список изменений Drupal 9.4.8.'
authors:
  - Niklan
category:
  area: 'Drupal 9.4.x'
  title: Drupal 9.4.8
  order: 8
---

<Aside type="warning">

Данная версия находится в разработке и не предназначена для использования.

</Aside>

## Cache

- [#3296987](https://www.drupal.org/node/3296987) В документацию метода `CacheableDependencyInterface::getCacheMaxAge()` добавлена информация о `Cache::PERMANENT`.

## CKEditor 5

- [#3306153](https://www.drupal.org/node/3306153) CKEditor 5 обновлён до версии 35.1.0.
- [#3268306](https://www.drupal.org/node/3268306) Исправлена неполадка, из-за которой могли не сохранятся
  собственные HTML-теги, например: `<drupal-media>`, `<drupal-entity>`, `<foobar>`.
- [#3270734](https://www.drupal.org/node/3270734) Тесты модулей Editor и CKEditor 5 больше не используют CKEditor 4.
- [#3222756](https://www.drupal.org/node/3222756) Добавлена поддержка загрузки изображений из внешних источников.
- [#3280343](https://www.drupal.org/node/3280343) Актуализированы `@todo` комментарии.
- [#3306216](https://www.drupal.org/node/3306216) Прозрачные иконки теперь форсируются быть полностью видимыми.
- [#3283776](https://www.drupal.org/node/3283776) Внесены улучшения в `CKEditor5PluginDefinition::getElements()`.
- [#3284254](https://www.drupal.org/node/3284254) `HTMLRestrictions` больше не позволяет добавить разрешение `<tag attr="*">` потому что это равносильно `<tag attr>`.

## Comment

- [#3166561](https://www.drupal.org/node/3166561) Исправлена неполадка, из-за которой при удалении пользователя, его комментарии могли быть удалены, вместо того чтобы быть переназначены на анонимного пользователя.

## Config System

- [#3312001](https://www.drupal.org/node/3312001) Исправлено состояние гонки в `ConfigImporter`.

## Database

- [#3294695](https://www.drupal.org/node/3294695) Исправлена неполадка, из-за которой слой обратной совместимости драйверов баз данных мог не работать для реплик.
- [#3260227](https://www.drupal.org/node/3260227) Тесты драйверов баз данных перенесены в соответствующие модули драйверов.

## Datetime

- [#2993165](https://www.drupal.org/node/2993165) Исправлена неполадка, из-за которой Date Only поле могло некорректно трактовать значение по умолчанию если у пользователя используется собственный часовой пояс.

## Filter

- [#2710427](https://www.drupal.org/node/2710427) Исправлена неполадка из-за которой обновление разрешенных тегов могло приводить к некорректным разрешениям при использовании пустого значения.

## Form API

- [#3307468](https://www.drupal.org/node/3307468) Улучшена документация для `$context['sandbox']` из Batch API.
- [#2842141](https://www.drupal.org/node/2842141) В документацию `hook_form_FORM_ID_alter()` добавлено объяснение в каком порядке вызываются данные хуки. 

## Layout Builder

- [#3119786](https://www.drupal.org/node/3119786) Исправлена неполадка, из-за которой не отображались изображения по умолчанию указанные в настройках поля.

## Migrate

- [#3202665](https://www.drupal.org/node/3202665) `migration_lookup` теперь выдаёт больше информации при выбрасывании исключения.

## Olivero

- [#3281341](https://www.drupal.org/node/3281341) Исправлена неполадка из-за которой оформление цитат могло быть некорректным в некоторых случаях.

## Views

- [#2875987](https://www.drupal.org/node/2875987) Исправлена неполадка из-за которой вызов альтера формы Views вызывался дважды.
- [#3291520](https://www.drupal.org/node/3291520) Исправлена неполадка, из-за которой неправильно отображалось название термина если оно начиналось с нуля.
- [#3006812](https://www.drupal.org/node/3006812) Исправлен некорректный тип для `ViewsExecutable->exposed_widgets` со `string` на `array`.

## Прочие изменения

- [#3309719](https://www.drupal.org/node/3309719) Исправлен некорректный тайпхинт для переменной в `EntityAutocomplete`.
- [#3060716](https://www.drupal.org/node/3060716) Исправлен некорректный `@oversDefaultClsas`.
- [#3311476](https://www.drupal.org/node/3311476) Тест `DownloadTest` будет пропускаться на SQLite базах данных, так как сейчас это приводит к блокировке БД.
- [#3285054](https://www.drupal.org/node/3285054) В темы Claro и Olivero добавлено отключение стилей CKEditor 5 `ckeditor5-stylesheets: false`.
- [#2617330](https://www.drupal.org/node/2617330) Исправлено некорректное условие в `LogMessageParser::parseMessagePlaceholders()`.
- [#3309173](https://www.drupal.org/node/3309173) Исправлены ссылки на документацию драйверов баз данных на актуальные.
- [#3310760](https://www.drupal.org/node/3310760) Исправленные битые линки в `@todo`.
- [#3308449](https://www.drupal.org/node/3308449) Исправлена неполадка в `CategorizingPluginManagerTrait::getSortedDefinitions()`, из-за которой результат мог отличаться если категория не являлась строкой.
- [#3265574](https://www.drupal.org/node/3265574) Там где возможно, оставшиеся вызовы с использованием xpath заменены на WebAssert методы.