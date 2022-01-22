---
title: Подключение библиотеки
slug: wiki/9/libraries/attach
core: 9
metatags:
  title: 'Drupal 9: Подключение библиотеки'
  description: 'Подключение необходимых библиотек на страницах в Drupal 9.'
category:
  area: Библиотеки
  title: Подключение
  order: 3
authors:
  - Niklan
---

Для того чтобы ассеты библиотеки были загружены на странице, к странице нужно указать зависимость от данной библиотеки. Это можно сделать различными способами и на разных этапах выполнения.

### Подключение через render array

Для того чтобы подключить библиотеку при помощи render array, вы должны модифицировать его примерно таким образом:

```php
$build['some_render_element']['#attached']['library'][] = 'mymodule/cuddly-slider';
```

Данный способ применим ко всем render array, не зависимо где вы находитесь. Это может касаться hook theme, форм, препроцесса страниц, render element и др.

### Подключение библиотеки в Twig шаблонах

Вы можете подключить библиотеку к шаблону, при помощи использования необходимого `hook_preprocess_HOOK()` и способа подключения через render array, а также при помощи специальной Twig функции `attach_library()`.

```twig
{{ attach_library('mymodule/cuddly-slider') }}
<div>Some markup {{ message }}</div>
```
