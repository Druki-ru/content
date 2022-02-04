---
title: SA-CORE-2021-011
slug: wiki/security/sa-core-2021-011
metatags:
  title: 'Drupal: SA-CORE-2021-011'
  description: 'Умеренно критично. Исправлено в версиях: 8.9.20, 9.1.14, 9.2.9.'
authors:
  - Niklan
---

**Проект:** Drupal Core\
**Дата публикации:** 17 ноября 2021\
**Риск безопасности:** Умеренно критично. Critical 13∕25 AC:Basic/A:User/CI:Some/II:Some/E:Theoretical/TD:Default\
**Уязвимость:** Обход доступа

## Описание

Drupal предоставляет интеграцию с библиотекой CKEditor для предоставления WYSIWYG-редактирования. Для библиотеки вышло обновление безопасности, которое влияет на Drupal.

Уязвимость доступна, если Drupal настроен на использование CKEditor в качестве редактора. Злоумышленник, который имеет права на создание и редактирование содержимого (даже без доступа к CKEditor), может использовать множественные атаки межсайтового скриптинга (XSS) для атаки на пользователей, которые имеют доступ к редактору, включая администраторов.

Чтобы получить больше информации о проблеме, изучите предупреждения по безопасности в от CKEditor:

* [CVE-2021-41164](https://github.com/ckeditor/ckeditor4/security/advisories/GHSA-pvmx-g8h5-cprj) Уязвимость в Advanced Content Filter (ACF) позволяет выполнить JavaScript код при помощи специально подготовленного HTML.
* [CVE-2021-41165](https://github.com/ckeditor/ckeditor4/security/advisories/GHSA-7h26-63m7-qhf2) HTML комментарии позволяют выполнять JavaScript код.

[Drupal Steward](https://www.drupal.org/steward) не покрывает данную проблему.

## Решение

Обновиться до актуальных версий:

- Если используете Drupal 9.2.x, обновитесь до [Drupal 9.2.9](../../../releases/9/9.2.x/9.2.9/index.md).
- Если используете Drupal 9.1.x, обновитесь до [Drupal 9.1.14](../../../releases/9/9.1.x/9.1.14/index.md).
- Если используете Drupal 8.9.x, обновитесь до [Drupal 8.9.20](../../../releases/8/8.9.x/8.9.20/index.md).

<Aside type="warning">

Обратите внимание, что срок службы [Drupal 8](../../../8/index.md) подошёл к концу. Это последний выпуск безопасности с исправлениями для Drupal 8.

</Aside>

## Ссылки

- [SA-CORE-2021-011](https://www.drupal.org/sa-core-2021-011) (англ.), drupal.org, 17 ноября 2021