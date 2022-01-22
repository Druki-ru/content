---
title: 'SA-CORE-2020-009'
slug: wiki/security/sa-core-2020-009
metatags:
  title: 'Drupal: SA-CORE-2020-009'
  description: 'Критическое. Исправлено в версиях: 8.8.10, 8.9.6, 9.0.6.'
authors:
  - Niklan
---

**Проект:** Drupal Core\
**Дата публикации:** 16 сентября 2020\
**Риск безопасности:** Критическое. 15/25 AC:Basic A:None CI:Some II:Some E:Theoretical TD:Default\
**Уязвимость:** Межсайтовый скриптинг

## Описание

Drupal 8 и 9 при определённых обстоятельствах подверженый межсайтовому скриптингу.

Злоумышленник может использовать процесс рендера HTML для поражённых форм, чтобы воспользоваться уязвимостью.

## Решение

Обновиться до актуальных версий:

- Если используете Drupal 8.8.x, обновитесь до [Drupal 8.8.10](../../../releases/8/8.8.x/8.8.10/index.md).
- Если используете Drupal 8.9.x, обновитесь до [Drupal 8.9.6](../../../releases/8/8.9.x/8.9.6/index.md).
- Если используете Drupal 9.0.x, обновитесь до [Drupal 9.0.6](../../../releases/9/9.0.x/9.0.6/index.md).

Контриб и кастом которые перезаписывают методы `\Drupal\Core\Form\FormBuilder::renderPlaceholderFormAction` и/или `\Drupal\Core\Form\FormBuilder::buildFormAction` должны убедиться что они проводят должную сантизиацию для ссылок.

## Ссылки

- [SA-CORE-2020-009](https://www.drupal.org/sa-core-2020-009) (англ.), drupal.org, 16 сентября 2020