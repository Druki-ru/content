---
title: 'Drupal 9.3.0'
slug: drupal/releases/9.3.0
core: 9
metatags:
  title: 'Drupal 9.3.0: Список изменений' 
  description: 'Список изменений Drupal 9.3.0.'
---

> [!WARNING]
> Данная версия находится в разработке. Список изменений может меняться и дополняться, даты могут смещаться.

**Дата релиза**: декабрь 2021\
**Активная поддержка до**: июнь 2022\
**Поддержка безопасности до**: декабрь 2022

## Маршруты ревизий нод теперь конвертируют параметры {node} и {node_revision} в сущности

* [#2730631](https://www.drupal.org/node/2730631)

Маршруты ревизий нод, такие, как `/node/123/revisions/456/view`, теперь возвращают объекты сущности Node при обращении к параметрам `\Drupal::routeMatch()->getParameter('node')` и `\Drupal::routeMatch()->getParameter('node_revision')`.

Данное изменение касается следующих маршрутов:

* `entity.node.revision`: `/node/{node}/revisions/{node_revision}/view`
* `node.revision_revert_confirm`: `/node/{node}/revisions/{node_revision}/revert`
* `node.revision_revert_translation_confirm`: `/node/{node}/revisions/{node_revision}/revert/{langcode}`
* `node.revision_delete_confirm`: `/node/{node}/revisions/{node_revision}/delete`

**Ранее:**

```php
$nid = \Drupal::routeMatch()->getParameter('node');
$node = \Drupal\node\Entity\Node::load($nid);
$vid = \Drupal::routeMatch()->getParameter('node_revision');
$node_revision = node_revision_load($vid);
```

До этого изменения, `$nid` и `$vid` возвращали целые числа, поэтому требовалось загружать сущность самостоятельно.

**Теперь:**

```php
$node = \Drupal::routeMatch()->getParameter('node');
$nid = $node->id();
$node_revision = \Drupal::routeMatch()->getParameter('node_revision');
$vid = $node_revision->getRevisionId();
```

Параметры возвращают объекты сущности.

## Database System

* [#3211780](https://www.drupal.org/node/3211780) `Connection::queryTemporary()` помечен устаревшим.
