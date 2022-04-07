---
title: 'Drupal 9.3.10'
slug: wiki/drupal/releases/9.3.10
core: 9
metatags:
  title: 'Drupal 9.3.10: Список изменений'
  description: 'Список изменений Drupal 9.3.10.'
authors:
  - Niklan
  - a-kovrigin
category:
  area: 'Drupal 9.3.x'
  title: Drupal 9.3.10 (в разработке)
  order: 10
---

<Aside type="warning" header="В разработке">

Данная версия находится в разработке и не готова к использованию. Список изменений может меняться.

</Aside>

## Добавлен API для подключения собственных стилей оформления CKEditor 5

- [#3194084](https://www.drupal.org/node/3194084)

Добавлена возможность подключения собственных файлов стилей оформления для редактора CKEditor 5 при помощи свойства `ckeditor5-stylesheets`.

Новое свойство работает не так как `ckeditor_stylesheets`, которое используется для редактора CKEditor 4. В отличие от CKEditor 4, 5 версия не использует `<iframe>` для редактора, а значит, стили добавляемые для редактора будут применяться для всей страницы. В зависимости от того, какие стили использует ваша тема оформления, возможно вам потребуется добавить дополнительные стили для CKEditor 5, чтобы исправить неточности или улучшить под себя те или иные элементы.

Стили указанные в `ckeditor5-stylesheets` будут загружаться только на страницах где используется соответствующий редактор.

**Пример:**

```yaml
# themename.info.yml
ckeditor5-stylesheets:
  - css/base-ckeditor5.css
```

```css
/* css/base-ckeditor5.css */
/* Prefix the h2 with .ck-content to keep the style in the editor, and avoid polluting the page with unwanted h2 styles */
.ck-content h2 {
  color: blue;
}
```

## Block Content

- [#3267644](https://www.drupal.org/node/3267644) Тесты теперь используют [тему оформления](../../../../9/themes/index.md) Stark вместо Classy.

## Cache System

- [#2873732](https://www.drupal.org/node/2873732) Внесены улучшения в `CookiesCacheContext`, который мог приводить к ошибке «Array
  to string conversion in CacheContextsManager::convertTokensToKeys()».

## CKEditor 5

- [#3261600](https://www.drupal.org/node/3261600) CKEditor 5 обновлён до версии 32.
- [#3260032](https://www.drupal.org/node/3260032) Библиотеке `ckeditor5/drupal.ckeditor5` добавлена зависимость `ckeditor5/ie11.user.warnings`.
- [#3260853](https://www.drupal.org/node/3260853) Добавлена поддержка wildcard для аттрибутов и их значений, например: `<foo data-*>`, `<foo *-bar-*>`, `<foo *-bar>`, `<h2 id="jump-*">`.
- [#3264775](https://www.drupal.org/node/3264775) Панель инструментов больше не скрывается если `<drupal-media>` находится в фокусе.
- [#3264727](https://www.drupal.org/node/3264727) Добавлена поддержка использования `<drupal-image>` и `<drupal-media>` внутри строки (`inline`).
- [#3268368](https://www.drupal.org/node/3268368) Внесены улучшения в тест `MediaLibraryTest`.
- [#3268272](https://www.drupal.org/node/3268272) Добавлен тайпкастинг при вызове `strpos()`.
- [#3248430](https://www.drupal.org/node/3248430) Улучшена документация для `ckeditor5.es6.js`.
- [#3269064](https://www.drupal.org/node/3269064) (отменено) CKEditor 5 обновлён до версии 33.
- [#3268174](https://www.drupal.org/node/3268174) Исправлена неполадка в обновлении `format` с CKEditor 4 до CKEditor 5. 
- [#3248228](https://www.drupal.org/node/3248228) Исправлена неполадка, из-за которой встроенные Media внутри строки с применением декораторов становились недоступными для редактирования. 
- [#3231337](https://www.drupal.org/node/3231337) Внесены улучшения в плагин `DrupalMediaEditing`, которые позволяют корректно сохранять модель элемента.
- [#3260869](https://www.drupal.org/node/3260869) Улучшена поддержка аттрибутов на блоковых HTML элементах.
- [#3270112](https://www.drupal.org/node/3270112) Исправлена неполадка, из-за которой анонсирование информации для экранных читателей могла дублироваться.
- [#3270110](https://www.drupal.org/node/3270110) При фокусировке кнопки добавлена информация для экранных читателей о том что она делает и как ей пользоваться.
- [#3231328](https://www.drupal.org/node/3231328) Улучшена «умная» выборка плагинов по умолчанию на основе используемого формата ввода.
- [#3270108](https://www.drupal.org/node/3270108) Исправлена неполадка, из-за которой редактор не загружался в браузере Edge с активным режимом WHCM (Windows Hight Constrast Mode).
- [#3259443](https://www.drupal.org/node/3259443) Исправлена неполадка, из-за которой не отображались настройки, если кнопка была добавлена после удаления всех прочих кнопок.
- [#3268860](https://www.drupal.org/node/3268860) Исправлена неполадка, из-за которой оборачивание `<drupal-media>` отличным от `<a>` элементом, приводило к его некорректному преобразованию.
- [#3268307](https://www.drupal.org/node/3268307) Использование `$block` заменено на `$text-container` из-за возможных пересечений с реальными блоками.

## Claro

- [#3219921](https://www.drupal.org/node/3219921) Исправлена неполадка, из-за которой отображалась полоска прокрутки для элемента автодополнения.

## Composer

- [#3162228](https://www.drupal.org/node/3162228) Исправлена неполадка, из-за которой на Composer 2 могла происходить фатальная ошибка «Call to undefined method Composer\DependencyResolver\Operation\UpdateOperation::getJobType() in /home/mysite/public_html/core/lib/Drupal/Core/Composer/Composer.php:170».

## Database Logging

- [#3250397](https://www.drupal.org/node/3250397) Исправлена неполадка, из-за которой модуль мог приводить к ошибкам из CLI на PHP 8.1.

## Datetime

- [#2636086](https://www.drupal.org/node/2636086) (отменено) Внесены улучшения в `FilterDateTest`.

## Editor

- [#3230829](https://www.drupal.org/node/3230829) `editor_form_filter_format_form_alter()` теперь также корректно очищает значение `editor_plugin`.

## Form System

- [#2911473](https://www.drupal.org/node/2911473) Улучшена обработка отключенных, но при этом активных чекбоксов. Теперь, активный (отмеченный) отключенный чекбокс корректно передаёт значение и сохраняет его.

## JavaScript

- [#3265652](https://www.drupal.org/node/3265652) В связи с тем что проект jQuery UI снова получает обновления безопасности, было решено удалить форк библиотеки из ядра Drupal и загружать зависимость с официальными сборками.
- [#3267705](https://www.drupal.org/node/3267705) Исправлена неполадка, из-за которой команда `yarn check -s` проваливалась при проверке коммита.

## JSON:API

- [#3272731](https://www.drupal.org/node/3272731) Тесты модуля теперь используют тему оформления Stark вместо Classy.

## Layout Builder

- [#3268680](https://www.drupal.org/node/3268680) Восстановлен и исправлен тест `LayoutBuilderDisableInteractionsTest::testFormsLinksDisabled()`.

## Layout Discovery

- [#3272746](https://www.drupal.org/node/3272746) Тесты модуля теперь используют тему оформления Stark вместо Classy.

## Media Library

- [#3115054](https://www.drupal.org/node/3115054) Исправлена неполадка, из-за которой добавление или удаление элементов сбрасывало сортировку в виджете. 

## Migrate Drupal

- [#3266443](https://www.drupal.org/node/3266443) Тест `StaetFileExists` переименован в `StateFileExistsTest`.

## Migrate System

- [#3252562](https://www.drupal.org/node/3252562) Для `callback` плагина добавлена документация с примером, как использовать функции обратного вызова без аргументов.

## MySQL DB Driver

- [#2797141](https://www.drupal.org/node/2797141) Удалены методы `::tableExists()` и `::fieldExists()` из `Drupal\Core\Database\Driver\mysql\Schema`.

## Render System

- [#2779999](https://www.drupal.org/node/2779999) Добавлены примеры как индивидуальным чекбоксам или радио-кнопкам добавить описание.
- [#3265929](https://www.drupal.org/node/3265929) Примеры для чекбоксов теперь имеют более нейтральные примеры.

## Shortcut

- [#3263201](https://www.drupal.org/node/3263201) В документации [хука](../../../../9/hooks/index.md) `hook_shortcut_default_set()` добавлен тайпхинт для параметра `$account`.

## Views UI

- [#3112547](https://www.drupal.org/node/3112547) Тесты модуля теперь используют тему оформления Stark вместо Classy.

## Тестирование

- [#3041900](https://www.drupal.org/node/3041900) В `JSWebAssert` упоминание «css» и «xpath» приведено к единому нижнему регистру.

## Прочие изменения

- [#3226716](https://www.drupal.org/node/3226716) Для методов `Drupal\Core\TypedData\TranslatableInterface` добавлена недостающая документация.
- [#3265723](https://www.drupal.org/node/3265723) В `FormattableMarkup` исправлено дублирование слова «directly».