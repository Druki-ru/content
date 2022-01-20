---
title: SA-CORE-2022-001
slug: wiki/security/sa-core-2022-001
metatags:
  title: 'Drupal: SA-CORE-2022-001'
  description: 'Умеренно критично. Исправлено в версиях: 9.2.11, 9.3.3.'
---

**Проект:** Drupal Core\
**Дата публикации:** 19 января 2022\
**Риск безопасности:** Умеренно критично. Critical 14∕25 AC:Basic/A:User/CI:Some/II:Some/E:Theoretical/TD:Default\
**Уязвимость:** Подделка межсайтовых запросов

## Описание

jQuery UI — сторонняя библиотека, используемая в Drupal. Ранее считалось что поддержка библиотеки прекращена.

Позднее, в 2021 году, jQuery UI анонсировали что они продолжат разработку, а заодно и выпустили новый релиз [jQuery UI 1.13.0](https://blog.jqueryui.com/2021/10/jquery-ui-1-13-0-released/). В данном релизе они раскрыли следующую проблему безопасности, которая может повлиять на Drupal:

- [CVE-2021-41183](https://github.com/jquery/jquery-ui/security/advisories/GHSA-j7qv-pgf6-hvh4): XSS in the of option of the .position() util

Есть вероятность, что данную уязвимость можно использовать в связке с некоторыми модулями для Drupal. В целях предосторожности, были применены соответствующие изменения в jQuery версию, используемую в Drupal.

> [!NOTE]
> Исходный код jQuery UI был добавлен в [Drupal 9.0.0](../../../releases/9/9.0.x/9.0.0/index.md). Drupal прекращает использование данной библиотеки, в связи с чем, было применено только исправление безопасности. Все прочие новшества jQuery UI 1.13.0 и будущих версий, добавляться не будут.

[Drupal Steward](https://www.drupal.org/steward) не покрывает данную проблему.

## Решение

Обновиться до актуальных версий:

- Если используете Drupal 9.3.x, обновитесь до [Drupal 9.3.3](../../../releases/9/9.3.x/9.3.3/index.md).
- Если используете Drupal 9.2.x, обновитесь до [Drupal 9.2.11](../../../releases/9/9.2.x/9.2.11/index.md).

## Ссылки

- [SA-CORE-2022-001](https://www.drupal.org/sa-core-2022-001) (англ.), drupal.org, 19 января 2022
- [jQuery UI 1.13.0 released](https://blog.jqueryui.com/2021/10/jquery-ui-1-13-0-released/) (англ.), blog.jqueryui.com, 7 октября 2021
- [CVE-2021-41183](https://github.com/jquery/jquery-ui/security/advisories/GHSA-j7qv-pgf6-hvh4) (англ.), github.com, 26 октября 2021