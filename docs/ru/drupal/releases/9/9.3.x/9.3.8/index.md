---
title: 'Drupal 9.3.8'
slug: wiki/drupal/releases/9.3.8
core: 9
metatags:
  title: 'Drupal 9.3.8: Список изменений'
  description: 'Список изменений Drupal 9.3.8.'
authors:
  - Niklan
category:
  area: 'Drupal 9.3.x'
  title: Drupal 9.3.8 (в разработке)
  order: 7
---

<Aside type="warning" header="В разработке">

Данная версия находится в разработке и не готова к использованию. Список изменений может меняться.

</Aside>

## Добавлен API для подключения собственны стилей оформления CKEditor 5

- [#3194084](https://www.drupal.org/node/3194084)

Добавлена возможность подключение собственный файлов стилей оформления для редактора CKEditor 5 при помощи свойства `ckeditor5-stylesheets`.

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

## CKEditor 5

- [#3261600](https://www.drupal.org/node/3261600) CKEditor 5 обновлён до версии 32.
- [#3260032](https://www.drupal.org/node/3260032) Библиотеке `ckeditor5/drupal.ckeditor5` добавлена зависимость `ckeditor5/ie11.user.warnings`.
- [#3260853](https://www.drupal.org/node/3260853) Добавлена поддержка wildcard для аттрибутов и их значений, например: `<foo data-*>`, `<foo *-bar-*>`, `<foo *-bar>`, `<h2 id="jump-*">`.
- [#3264775](https://www.drupal.org/node/3264775) Панель инструментов больше не скрывается если `<drupal-media>` находится в фокусе.
- [#3264727](https://www.drupal.org/node/3264727) Добавлена поддержка использования `<drupal-image>` и `<drupal-media>` внутри строки (`inline`).
- [#3268368](https://www.drupal.org/node/3268368) Внесены улучшения в тест `MediaLibraryTest`.
- [#3268272](https://www.drupal.org/node/3268272) Добавлен тайпкастинг при вызове `strpos()`.
- [#3248430](https://www.drupal.org/node/3248430) Улучшена документация для `ckeditor5.es6.js`.
- [#3269064](https://www.drupal.org/node/3269064) CKEditor 5 обновлён до версии 33.

## Database Logging

- [#3250397](https://www.drupal.org/node/3250397) 

## JavaScript

- [#3265652](https://www.drupal.org/node/3265652) В связи с тем что проект jQuery UI снова получает обновления безопасности, было решено удалить форк библиотеки из ядра Drupal и загружать зависимость с официальными сборками.
- [#3267705](https://www.drupal.org/node/3267705) Исправлена неполадка, из-за которой команда `yarn check -s` проваливалась при проверке коммита.

## Migrate System

- [#3252562](https://www.drupal.org/node/3252562) Для `callback` плагина добавлена документация с примером, как использовать функции обратного вызова без аргументов.

## MySQL DB Driver

- [#2797141](https://www.drupal.org/node/2797141) Удалены методы `::tableExists()` и `::fieldExists()` из `Drupal\Core\Database\Driver\mysql\Schema`.

## Shortcut

- [#3263201](https://www.drupal.org/node/3263201) В документации [хука](../../../../9/hooks/index.md) `hook_shortcut_default_set()` добавлен тайпхинт для параметра `$account`.

## Тестирование

- [#3041900](https://www.drupal.org/node/3041900) В `JSWebAssert` упоминание «css» и «xpath» приведено к единому нижнему регистру.

## Прочие изменения

- [#3226716](https://www.drupal.org/node/3226716) Для методов `Drupal\Core\TypedData\TranslatableInterface` добавлена недостающая документация.