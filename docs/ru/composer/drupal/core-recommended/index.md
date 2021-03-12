---
id: drupal-core-recommended
title: drupal/core-recommended
search-keywords:
  - composer
metatags:
  description: 'Метапакет Composer с Drupal ядром.'
---

**drupal/core-recommended** — матапакет предназначенный для использования на Drupal проектах управляемых через [Composer](../../index.md). Пакет используется в официальных Composer шаблонах проектов [drupal/recommended-project](../recommended-project/index.md) и [drupal/legacy-project](../legacy-project/index.md).

Рекомендуется запрашивать`drupal/core-recommended` вместо `drupal/core`, так как данный пакет включает в себя как `drupal/core`, так и все необходимые зависимости. Самое важное и единственное отличие между пакетами в том, что зависимостям `drupal/core-recommended` проставлены конкретные версии пакетов на момент релиза версии, для которой вы запрашиваете данный пакет, тогда как у `drupal/core` они прописаны в минимальной версии.

Последствия использования `drupal/core` заключаются в том, что зависимости Drupal ядра, могут обновиться до более свежих версий, с которыми Drupal ещё не тестировался. Иногда случается, что новые версии зависимостей приводят к ошибкам в Drupal (например, однажды [Twig 1.40.0 сломал Drupal 8.6.x](https://github.com/twigphp/Twig/issues/2990)), и скорость исправления проблем, зачастую, зависит от сторонних разработчиков. По этой причине, фиксированные версии для зависимостей ядра помогут избежать подобных ситуаций. Вы будете уверены, что ядро тестировалось с данными версиями пакетов и работает полноценно и корректно.

## Смотрите также

- [drupal/recommended-project](../recommended-project/index.md)
- [drupal/legacy-project](../legacy-project/index.md)
- [drupal/core-composer-scaffold](../core-composer-scaffold/index.md)

## Ссылки

- [drupal/core-recommended](https://github.com/drupal/core-recommended), Github
