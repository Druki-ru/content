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
    - **_controller**: Значение типа [callable (callback-функция)](https://www.php.net/manual/en/language.types.callable.php):
      - **Class:method**: В формате `\Drupal\[module name]\Controller\[ClassName]::[method]`, например `_controller: '\Drupal\foo\Controller\FooController::build'`,.

        В данном случае Drupal вызовет метод `build()` класса `FooController` из неймспейса `\Drupal\foo\Controller`.

      - **{Сервис}(services:8)**: Позволяет формировать значение из сервиса. Значение задается в формате `mymodule.service_name:[method]`, где `mymodule.service_name` - название сервиса, а `[method]` - название метода сервиса, который необходимо вызвать. Например `_controller: 'mymodule.service_name:build'`.
    - **_form**: Класс, реализующий `Drupal\Core\Form\FormInterface`.
    - **_entity_view**: Значение в формате `[entity_type].[view_mode]`. Маршрут найдет сущность в пути и отрендерит её в указаном формате отображения. Например `_entity_view: node.teaser`.
    - **_entity_list**: Значение в формате `[entity_type]`. Выводит список сущностей указанного типа при помощи `EntityListController` данной сущности. Например `_entity_list: user` выведет список всех пользователей на сайте.
    - **_entity_form**: Значение в формате `[entity_type].[form_view_mode]`. Выведет форму редактирования сущности указанного типа, в указанном формате отображения формы. Например `_entity_form: node.default` выведет форму редактирования материала в стандартном режиме отображения формы.
    - **_route**: Название маршрута, чьим алиасом вы хотите сделать данный маршрут. Например `_route: views.files.page_1`.
  - **Опциональные параметры.**
    - **_title**: Заголовок страницы на **английском** языке. Данное значение будет обработано `t()`.
    - **_title_arguments**: Дополнительные аргументы, передаваемые с заголовком в `t()`.
    - **_title_context**: Контекст, передаваемый с заголовком в `t()`.
    - **_title_callback**: Значение типа callable, которое возвращает занчение для заголовка. В данном случае, перевод значения ложится на callback-функцию.
- **methods**: (опционально) В дополнение к пути, вы можете ограничить типы запросов, на который будет реагировать маршрут. Значение должно быть в виде массива с перечислением всех допустимых типов запроса. Например `methods: [GET, POST, UPDATE, DELETE]`.


## Ссылки

- [Structure of routes](https://www.drupal.org/docs/8/api/routing-system/structure-of-routes) (англ), Drupal документация.