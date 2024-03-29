# Документация

Документация — статические страницы с описанием или документированием чего-либо.

## Введение

Для документирования или описания тех, или иных технологий, возможностей и прочего используется документация. Подразумевает что со временем в них могут потребоваться улучшения, изменения или дополнения.

## Структура документации

Вся документация располагается в корневой директории `/docs`, в которой должны находиться директори с языковым кодом в формате (ISO 639-1), например `/docs/ru`, `/docs/en`.

Внутри языковых директорий могут быть какие угодно директории с любой вложенностью.

### Базовые концепции

* **Страница документации** — директория содержащая `index.md`, в котором и находится содержимое для документа. Все прочие `*.md` файлы игнорируются.
* **Содержимое пишется с использованием Markdown** — все документы наполняются с
  использованием [Markdown](https://ru.wikipedia.org/wiki/Markdown) синтаксиса по
  спецификации [CommonMark](https://commonmark.org/). На проекте также добавлена поддержка собственных разметок. Для более детальной информации ознакомьтесь с разделом [Markdown](../../CONTRIBUTING.md).
* **Все документы начинаются с Front Matter** — каждый документ должен иметь Front Matter разметку в начале документа. Если вы не знакомы с Front Matter, рекомендуется прочитать раздел [Front Matter](../../CONTRIBUTING.md).

## Описание Front Matter документации

* `title` (обязательно): Заголовок документа. То что будет использовано в `<h1>` и `<title>`.
* `slug` (обязательно): Уникальный путь документа. Используется как URL документа. Например, документ со `slug: wiki/drupal/about` будет доступен по адресу `https://druki.ru/wiki/drupal/about`. Начальный и конечный слэш не указывается.
* `core`: Номер версии ядра, для которой написан данный документ. Используется в материалах которые написаны в контексте
  определённой версии Drupal. Значение должно быть `int`. Например `core: 9`.
* `category`: Позволяет категоризировать материал по названию категории в пределах `core` версии. Принимает следующие
  значения:
    * `area`: (обязательно) В этом параметре указывается заголовок категории. Материалы в категории будут искаться по
      схожему заголовку.
    * `order`: Указывает позицию данного материала в меню категории среди других материалов. Сортировка по возрастанию.
    * `title`: Переопределяет заголовок в меню категории на указанный здесь. Если не указано, будет использоваться
      значение `title`.
- `search-keywords`: Массив со строками, которые используются только в поиске, и имеют повышенный приоритет. В идеале,
  должны иметь ключевые фразы, по которым, скорее всего, люди попытаются найти материал. Например, для Libraries API это
    - "как подключать js css", "как добавить скрипт на страницу" и т.д. То есть то, что в заголовке никак не отражено,
      но имеет прямое отношение к нему.
- `metatags`: (массив) Данный параметр позволяет задать конкретные мета-теги для материала в принудительном порядке. На
  данный момент поддерживаются следующие переопределяемые значения:
    - `title`: (опционально) Позволяет переопределить заголовок страницы `<title></title>`, а также, все связанные
      метатеги `<meta name="title">`, `<meta name="twitter:title">`, `<meta property="og:title">`. Данное значение не
      влияет на `title` материала, он останется прежним и будет находиться в `<h1>`.
    - `description`: (опционально) Позволяет переопределить описание страницы. Влияет на следующие
      мета-теги: `<meta name="description">`, `<meta property="og:description">`
- `authors`: (рекомендуется) Массив с ID авторов, которые принимали участие в создании, наполнении или редактировании материала. ID авторов и информация о них описана в [«Профиль автора»](author-profile.md).

_Все прочие свойства, если они присутствуют, игнорируются._

### Шаблон документации

```markdown
---
title: Заголовок документации
slug: wiki/url-of-documentation
core: 10
category:
  area: Название группы материалов
  order: 1
  title: Короткий заголовок
search-keywords:
  - Шаблон
  - Документация
metatags:
  title: 'Drupal 10: Документация'
  description: 'Пример документации.'
authors:
  - Dries
  - zuck
---

Содержимое.
```