---
id: drupal-core-composer-scaffold
title: drupal/core-composer-scaffold
search-keywords:
  - composer
metatags:
  description: 'Метапакет Composer содержащий scaffold файлы Drupal ядра.'
---

**drupal/core-composer-scaffold** — метапакет содержащий [Composer](composer.md) плагин для управления scaffold файлами (те что находятся в корне ядра, например `index.php`, `update.php`, ...) из пакета `drupal/core`.

Цель scaffold файлов в том, чтобы позволить управлять Drupal проектами целиком при помощи Composer, при этом, иметь контроль над тем, где и какие файлы будут расположены. Цель данных действий в том, чтобы была возможность настроить composer шаблоны таким образом, чтобы они были идентичны структуре Drupal ядра из архивов версии [8.7.x](../8/releases/release-8.7.0.md) и более ранних.

Например, вы можете отключить создание (копирование) `.htaccess` файла, или указать что добавить в файл `robots.txt` при обновлении.

## Использование

Для того чтобы воспользоваться возможностями пакета, его необходимо запросить в проект при помощи [Composer](composer.md). Данный пакет уже является зависимостью в шаблонах [drupal/recommended-project](drupal-recommended-project.md) и [drupal/legacy-project](drupal-legacy-project.md).

Затем, вы можете настроить поведение данного пакета при помощи `extra` раздела в корневом `composer.json` файле проекта.

Обычно, процедура синхронизации файлов и всех сопутствующих операций, выполняется автоматически после `composer install`, `composer update` и `composer required`. Если вам необходимо принудительно вызывать плагин, можете воспользоваться командой:

```bash
composer drupal:scaffold
```

## Разрешенные пакеты

Scaffold файлы хранятся в тех проектах, которые запрошены в основном `composer.json` файле проекта. После выполнения `composer install`, `composer update` или `composer required`, запускается соответствующий плагин.

По умолчанию, плагин работает со всеми пакетами, но вы можете ограничить список пакетов, например, только для `drupal/core`.

```json
  "name": "my/project",
  ...
  "extra": {
    "drupal-scaffold": {
      "allowed-packages": [
        "drupal/core"
      ],
      ...
    }
  }
```

Предоставляя доступ пакету на scaffold файлы, это также делегирует права доступа ко всем файлам пакетов, которые запрашивает данный пакет (его зависимости). Это позволяет пакетам организовывать их ассеты так, как им это требуется. Например, пакет `drupal/core` может указать что свои assets файлы нужно хранить в подпроекте `drupal/assets`.

## Указание местоположения проекта

Для корректной работы плагина, корневой файл `composer.json` проекта, должен указать где находится корневой документ. Данное значение задается в `extra.drupal-scaffold.locations` в качестве соответствий `"ключ": "путь"`.

```json
  "name": "my/project",
  ...
  "extra": {
    "drupal-scaffold": {
      "locations": {
        "web-root": "./web/"
      },
      ...
    }
  }
```

Это позволяет настраивать проект с различными структурами, как это делают [drupal/recommended-project](drupal-recommended-project.md) и [drupal/legacy-project](drupal-legacy-project.md).

Если вы явно не укажете значение для `web-root` ключа, то по умолчанию оно примет значение `./`.

## Изменяем Scaffold файлы

Иногда, на проекте необходимо использовать scaffold файлы, но они требует некоторых изменений специфичных для проекта. Плагин предоставляет два способа внедрения изменений: добавление в конец и патчинг.

Пример ниже показывает как проект добавляет свои значения в конец файла `robots.txt`, который предоставляется `drupal/core`:

```json
  "name": "my/project",
  ...
  "extra": {
    "drupal-scaffold": {
      "file-mapping": {
        "[web-root]/robots.txt": {
          "append": "assets/my-robots-additions.txt",
        }
      }
    }
  }
```

Вы также можете добавлять изменения в начало файла, при помощи команды `prepend`.

Пример ниже демонстрирует использование события `post-drupal-scaffold-cmd`, который патчит `.htaccess` файл при помощи команды `patch`:

```json
  "name": "my/project",
  ...
  "scripts": {
    "post-drupal-scaffold-cmd": [
      "cd docroot && patch -p1 <../patches/htaccess-ssl.patch"
    ]
  }
```

## Объявление Scaffold файлов

Расположением scaffold файлов управляет пакет, который их и предоставляет, но местоположение всегда относительно какой-то директории, указанной в основном проекте — обычно это web-корень (где находится `index.php`).

В примере ниже, scaffold файл `robots.txt` копируется из его оригинального местоположения `[web-root]/robots.txt` в папку assets:

```json
{
  "name": "drupal/assets",
  ...
  "extra": {
    "drupal-scaffold": {
      "file-mapping": {
        "[web-root]/robots.txt": "assets/robots.txt",
        ...
      }
    }
  }
}
```

## Исключение Scaffold файлов

Иногда, проект требует того, чтобы некоторые файлы не обновлялись и не создавались если их нету. Например, если вы используете модуль `robotstxt`, то файл `robots.txt` должен отсутствовать. Это значит, что его нужно удалять каждый раз, как происходит обновление. Чтобы этого не делать, можно запретить создание данного файла, указав ему в назначении значение `false`, как показано на примере ниже:

```json
  "name": "my/project",
  ...
  "extra": {
    "drupal-scaffold": {
      "file-mapping": {
        "[web-root]/robots.txt": false
      }
    }
  }
```

Исключение файла также означает, что вы не будете получать обновления и исправление ошибок для данного файла. По возможности, лучше используйте `append` или `prepend`.

## Перезапись

По умолчанию, scaffold файлы перезаписывают файлы, которые находятся в пункте назначения. Иногда, проектам может потребоваться предоставлять файлы только если они отсутствуют (по умолчанию), но не обновлять, если файл уже присутствует. Данное поведение может быть задано при помощи опции `overwrite` установленной в `false`, как показано в примере ниже:

```json
{
  "name": "service-provider/d8-scaffold-files",
  "extra": {
    "drupal-scaffold": {
      "file-mapping": {
        "[web-root]/sites/default/settings.php": {
          "mode": "replace",
          "path": "assets/sites/default/settings.php",
          "overwrite": false
        }
      }
    }
  }
}
```

## Опции drupal-scaffold

В данном разделе представлены все возможные опции, поддерживаемые плагином и задаваемые в разделе `extra.drupal-scaffold` вашего `composer.json` файла проекта.

### allowed-packages

Опция `allowed-packages` предоставляет список пакетов, которые будут использоваться на этапе работы плагина.

```json
"allowed-packages": [
  "drupal/core",
],
```

### file-mapping

Опция `file-mapping` позволяет настроить соответствия scaffold файл с файлами назначений на проекте, с возможность контролировать поведение.

Доступные свойства:

- `mode`: Один из: `replace`, `append`, `skip`. Если режим не указан, будут использованы следующие значения по умолчанию:
  - `replace`: Если `path` задан, а значение является строкой.
  - `append`: Если задано одно из свойств `preprend` или `append`.
  - `skip`: Если в значении указано `false`.
- `path`: Путь до файла источника, который будет записан в файл назначения.
- `prepend`: Путь до файла, который будет добавлен в начало содержимого файла источника.
- `append`: Аналогично `prepend`, только добавляет в конец файла.
- `overwrite`: Если `false`, то файл источника не будет перенесен, если по пути назначения уже имеется файл.

Примеры:

```json
"file-mapping": {
  "[web-root]/sites/default/default.settings.php": {
    "mode": "replace",
    "path": "assets/sites/default/default.settings.php",
    "overwrite": true
  },
  "[web-root]/sites/default/settings.php": {
    "mode": "replace",
    "path": "assets/sites/default/settings.php",
    "overwrite": false
  },
  "[web-root]/robots.txt": {
    "mode": "append",
    "prepend": "assets/robots-prequel.txt",
    "append": "assets/robots-append.txt"
  },
  "[web-root]/.htaccess": {
    "mode": "skip",
  }
}
```

Короткая запись примера выше:

```json
"file-mapping": {
  "[web-root]/sites/default/default.settings.php": "assets/sites/default/default.settings.php",
  "[web-root]/sites/default/settings.php": {
    "path": "assets/sites/default/settings.php",
    "overwrite": false
  },
  "[web-root]/robots.txt": {
    "prepend": "assets/robots-prequel.txt",
    "append": "assets/robots-append.txt"
  },
  "[web-root]/.htaccess": false
}
```

### gitignore

Опция `gitignore` отвечает за то, будут ли созданы `.gitignore` файлы в процессе работы или нет.

- `true`: `.gitignore` файлы будут обновлены или созданы когда произведется обработка.
- `false`: `.gitignore` файлы никогда не будут создаваться и изменяться.
- Не задано: `.gitignore` файлы будут обновлены если конечная директория является рабочей копией git репозитория, и `vendor` директория игнорируется в ней.

### locations

Опция `locations` позволяет настроить именованные пути, куда будут помещаться scaffold файлы. Единственным обязательным путем является `web-root`. Если необходимо, вы можете создавать какие угодно пути.

```json
"locations": {
  "web-root": "./docroot"
},
```

### symlink

Опция `symlink` задает поведение, при котором файлы не копируются в место назначения, а для них создаётся символическая ссылка до оригинала.

```json
"symlink": true,
```

> [!NOTE]
> `append` и `prepend` операции переопределяют значение `symlink` на `false`.

## Смотрите также

- [drupal/recommended-project](drupal-recommended-project.md)
- [drupal/legacy-project](drupal-legacy-project.md)
- [drupal/core-recommended](drupal-core-recommended.md)

## Ссылки

- [drupal/core-composer-scaffold](https://github.com/drupal/core-composer-scaffold), Github