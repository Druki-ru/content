~~---
title: SA-CORE-2022-002
slug: wiki/security/sa-core-2022-002
metatags:
  title: 'Drupal: SA-CORE-2022-002'
  description: 'Умеренно критично. Исправлено в версиях: 7.86.'
---

**Проект:** Drupal Core\
**Дата публикации:** 19 января 2022\
**Риск безопасности:** Умеренно критично. Critical 14∕25 AC:Basic/A:User/CI:Some/II:Some/E:Theoretical/TD:Default\
**Уязвимость:** Подделка межсайтовых запросов

## Описание

В дополнение к проблеме, описанной в [SA-CORE-2022-001](../2022-001/index.md), данный релиз также содержит исправления уязвимостей, которые могут повлиять исключительно на Drupal 7:

- [CVE-2021-41182](https://github.com/jquery/jquery-ui/security/advisories/GHSA-9gj3-hwp5-pmwc): XSS in the altField option of the Datepicker widget
- [CVE-2021-41183](https://github.com/jquery/jquery-ui/security/advisories/GHSA-j7qv-pgf6-hvh4): XSS in *Text options of the Datepicker widget
- [CVE-2016-7103](https://nvd.nist.gov/vuln/detail/CVE-2016-7103): XSS in closeText option of Dialog
- [CVE-2010-5312](https://nvd.nist.gov/vuln/detail/CVE-2010-5312): XSS in the title option of Dialog (applicable only to the jQuery UI version included in D7 core)

[Drupal Steward](https://www.drupal.org/steward) не покрывает данную проблему.

## Решение

Обновиться до актуальных версий:

- Если используете Drupal 7, обновитесь до Drupal 7.86

## Ссылки

- [SA-CORE-2022-002](https://www.drupal.org/sa-core-2022-002) (англ.), drupal.org, 19 января 2022
- [jQuery UI 1.13.0 released](https://blog.jqueryui.com/2021/10/jquery-ui-1-13-0-released/) (англ.), blog.jqueryui.com, 7 октября 2021
- [CVE-2021-41182](https://github.com/jquery/jquery-ui/security/advisories/GHSA-9gj3-hwp5-pmwc) (англ.), github.com, 26 октября 2021
- [CVE-2021-41183](https://github.com/jquery/jquery-ui/security/advisories/GHSA-j7qv-pgf6-hvh4) (англ.), github.com, 26
  октября 2021
- [CVE-2016-7103](https://nvd.nist.gov/vuln/detail/CVE-2016-7103) (англ.), github.com, 26 октября 2021
- [CVE-2010-5312](https://nvd.nist.gov/vuln/detail/CVE-2010-5312) (англ.), github.com, 26 октября 2021