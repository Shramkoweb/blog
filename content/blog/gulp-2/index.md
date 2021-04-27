---
title: Стартовый шаблон GULP - часть 2 
date: "2021-04-27"
description: "Продолжение создания стартового шаблона для верстки на GULP"
---

В [прошлой части](/gulp) мы настроили репозиторий и разобрали настройки проекта и типы его зависимостей.

#### Структура проекта

На всех проектах где я работал зачастую используют BEM, поэтому структура проекта зачастую основана
на [этих правилах](https://ru.bem.info/methodology/filestructure/).

У нас будет самый распространенный вариант для небольших проектов:

```markdown
project
    src/                          # Директория исходных файлов сборки
        /styles                   # Директория стилей
            input.scss            # Стили блока input
            popup.scss            # Стили блока input
        /js                       # Директория скриптов
        /images                   # Директория c картинками
        /fonts                    # Директория c шрифтами
        index.html                # Страницы сайта
```

### GULP & gulpfile.js

Так как у нас в проекте мы используем [SASS](https://sass-lang.com/) наши исходные файлы нужно преобразовать в css
который понимает браузер. Для этого нам нужен сборщик или таск-раннер как [gulp](https://gulpjs.com/) поэтому давайте
напишем для него файл конфигурации.

#### Установим все нужные пакеты для работы GULP

В [прошлом посте](/gulp) я описал типы зависимостей и как их нужно ставить.
Установим нужные нам пакеты в devDependencies с фиксированной версией.

```shell
npm i -DE gulp browser-sync autoprefixer del gulp-htmlmin gulp-imagemin gulp-plumber gulp-postcss gulp-rename gulp-sass gulp-sourcemaps gulp-uglify-es postcss postcss-csso
```

После команды вы увидите, что в блоке devDependencies появились версии нужных нашему проекту зависимостей.

```json
"devDependencies": {
    "autoprefixer": "10.2.5",
    "browser-sync": "2.26.14",
    "del": "6.0.0",
    "gulp": "4.0.2",
    "gulp-htmlmin": "5.0.1",
    "gulp-imagemin": "7.1.0",
    "gulp-plumber": "1.2.1",
    "gulp-postcss": "9.0.0",
    "gulp-rename": "2.0.0",
    "gulp-sass": "4.1.0",
    "gulp-sourcemaps": "3.0.0",
    "gulp-uglify-es": "2.0.0",
    "postcss": "8.2.13",
    "postcss-csso": "5.0.1"
}
```
