---
id: structure-of-routes
title: Структура маршрутов
path: /docs/8/routing/structure-of-routes
core: 8
category-area: Маршрутизация
category-order: 3
search-keywords:
  - свойства маршрутов маршрута
  - какие параметры можно задать маршруту маршрутам контроллеру контроллерам
---

Маршруты, объявляемые в `*.routing.yml` могут содержать множество дополнительных свойств:

- **path**: (обязательно) Путь маршрута, должен начинаться со слеша `path: '/example'`. Понимает динамически части, заключенные в фигурные скобки (например: `path: `/foo/{argument}/bar'`).
- **defaults**:
  - **Обязательный параметр.** Должен быть один из перечисленных:
    - **_controller**: Значение типа [callable](https://www.php.net/manual/en/language.types.callable.php):
      - **Class:method**: В формате `\Drupal\[module name]\Controller\[ClassName]::[method]`, например `_controller: '\Drupal\foo\Controller\FooController::build'`,.

        Проверка
      - Сервис
        Тоже самое без пробела.

## Ссылки

- [Structure of routes](https://www.drupal.org/docs/8/api/routing-system/structure-of-routes) (англ), Drupal документация.