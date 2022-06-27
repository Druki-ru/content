---
title: 'Drupal 9.4.2'
slug: wiki/drupal/releases/9.4.2
core: 9
metatags:
title: 'Drupal 9.4.2: Список изменений'
description: 'Список изменений Drupal 9.4.2.'
authors:

- Niklan
  category:
  area: 'Drupal 9.4.x'
  title: Drupal 9.4.2 (в разработке)
  order: 2

---

<Aside type="warning">

Данная версия находится в разработке и не предназначена для использования.

</Aside>

## Layout Builder

- [#3264633](https://www.drupal.org/node/3264633), [#3292413](https://www.drupal.org/node/3292413) Функционал для интеграции
  Layout Builder с модулем Quick Edit вынесен непосредственно в Quick Edit.

## Прочие изменения

- [#3291877](https://www.drupal.org/node/3291877) Исправлен `InstallerTestBase`.
- [#3285230](https://www.drupal.org/node/3285230)
  В `DownloadFunctionalTest:: testExceptionThrow()` внесены улучшения для
  совместимости с `guzzlehttp/psr7` 2.3.0.