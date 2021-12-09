---
title: SA-CORE-2021-005
slug: wiki/security/sa-core-2021-005
metatags:
  title: 'Drupal: SA-CORE-2021-005'
  description: 'Умеренно критично. Исправлено в версиях: 8.9.18, 9.1.12, 9.2.4.'
---

**Проект:** Drupal Core\
**Дата публикации:** 12 августа 2021\
**Риск безопасности:** Умеренно критично. Critical 13∕25 AC:Basic/A:User/CI:Some/II:Some/E:Theoretical/TD:Default\
**Уязвимость:** Сторонние библиотеки.

## Описание

Drupal ядро использует библиотеку CKEditor. Для данной библиотеки вышло [обновление безопасности которое также влияет на Drupal](https://ckeditor.com/blog/ckeditor-4.16.2-with-browser-improvements-and-security-fixes/).

Уязвимости возможны, если Drupal настроен на использование библиотеки CKEditor. Злоумышленник, который может создавать или редактировать контент (даже без доступа к CKEditor), может использовать одну или несколько уязвимостей межсайтового скриптинга (XSS), направленных на пользователей с доступом к CKEditor, включая администраторов сайта с привилегированным доступом.

Для детальной информации смотрите [официальный анонс CKEditor](https://ckeditor.com/blog/ckeditor-4.16.2-with-browser-improvements-and-security-fixes/).

[Drupal Steward](https://www.drupal.org/steward) не покрывает данную проблему.

## Решение

Обновиться до актуальных версий:

- Если используете Drupal 8.9.x, обновитесь до [Drupal 8.9.18](../../../releases/8/8.9.x/8.9.18/index.md).
- Если используете Drupal 9.1.x, обновитесь до [Drupal 9.1.12](../../../releases/9/9.1.x/9.1.12/index.md).
- Если используете Drupal 9.2.x, обновитесь до [Drupal 9.2.4](../../../releases/9/9.2.x/9.2.4/index.md).

## Ссылки

- [SA-CORE-2021-005](https://www.drupal.org/sa-core-2021-005) (англ.), drupal.org, 12 августа 2021
- [CKEditor 4.16.2 with browser improvements and security fixes](https://ckeditor.com/blog/ckeditor-4.16.2-with-browser-improvements-and-security-fixes/) (англ.), ckeditor.com, 12 августа 2021
