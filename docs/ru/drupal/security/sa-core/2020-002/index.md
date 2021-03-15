---
title: 'SA-CORE-2020-002'
slug: security/sa-core-2020-002
metatags:
  title: 'Drupal: SA-CORE-2020-002'
  description: 'Умеренно критический. Исправлено в версиях: 8.7.14 и 8.8.6.'
---

**Проект:** Drupal Core\
**Дата публикации:** 20 мая 2020\
**Риск безопасности:** Умеренно критический. 10/25 AC:Complex A:Admin CI:Some II:Some E:Theoretical TD:Uncommon\
**Уязвимость**: Сторонние библиотеки

## Описание

jQuery выпустили обновление 3.5.0, которое также включает устранение двух уязвимостей. Данным уязвимостям подвержены все предыдущие версии библиотеки.

Соответствующие уведомления об уязвимостях:

- [CVE-2020-11022](https://github.com/jquery/jquery/security/advisories/GHSA-gxr4-xjj5-5px2)
- [CVE-2020-11023](https://github.com/jquery/jquery/security/advisories/GHSA-jpcq-cgw6-v4j6)

Данные уязвимости могут быть использованы на некоторых Drupal сайтах.

Были внесены изменения для сохранения обратной совместимости и минимизации регрессии на действующих Drupal сайтах. Методы jQuery будут работать, как и ранее, в соответствии с предыдущими версиями. Начиная с jQuery 3.5, неверные cамозакрывающиеся HTML-теги для элементов JavaScript [ведут себя иначе](https://jquery.com/upgrade-guide/3.5/#description-of-the-change). В связи с чем, для Drupal 8.8.x и 8.7.x сохраняется старое поведение, чтобы это не сказалось на работе сайтов.

В [Drupal 8.9](../../../8/releases/8.9.x/8.9.0/index.md) и [Drupal 9.0](../../../9/releases/9.0.x/9.0.0/index.md) [обратная совместимость](../../../../backward-compatibility/index.md) не будет сохранена.

## Решение

Обновиться до актуальных версий:

- Если используете Drupal 8.8.x, обновитесь до [Drupal 8.8.6](../../../8/releases/8.8.x/8.8.6/index.md).
- Если используете Drupal 8.7.x, обновитесь до [Drupal 8.7.14](../../../8/releases/8.7.x/8.7.14/index.md).

## Ссылки

- [SA-CORE-2020-002](https://www.drupal.org/sa-core-2020-002) (англ.), drupal.org, 20 мая 2020
- [jQuery Core 3.5 Upgrade Guide](https://jquery.com/upgrade-guide/3.5) (англ.), jQuery