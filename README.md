[![Build Status](https://travis-ci.org/anasteyshakoshman/lab07.svg?branch=master)](https://travis-ci.org/anasteyshakoshman/lab07)


## Laboratory work VII

Данная лабораторная работа посвещена изучению систем документирования исходного кода на примере **Doxygen**

```ShellSession
$ open https://www.stack.nl/~dimitri/doxygen/manual/index.html
```

## Tasks

- [X] 1. Создать публичный репозиторий с названием **lab07** на сервисе **GitHub**
- [X] 2. Выполнить инструкцию учебного материала
- [X] 3. Ознакомиться со ссылками учебного материала
- [X] 4. Составить отчет и отправить ссылку личным сообщением в **Slack**

## Tutorial
Задание переменной окружения
```ShellSession
$ export GITHUB_USERNAME=anasteyshakoshman
$ alias edit=vi
```


```ShellSession
$ git clone https://github.com/${GITHUB_USERNAME}/lab06 lab07
$ cd lab07
$ git remote remove origin
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab07
```
Создание документации
```ShellSession
$ mkdir docs
$ doxygen -g docs/doxygen.conf
$ cat docs/doxygen.conf
```

```ShellSession
$ sed -i  's/\(PROJECT_NAME.*=\).*$/\1 print/g' docs/doxygen.conf #устанавливает название проекта print
$ sed -i  's/\(EXAMPLE_PATH.*=\).*$/\1 examples/g' docs/doxygen.conf #устанавливает путь к examples
#устанавливает путь к examples, где есть include
$ sed -i  's/\(INCLUDE_PATH.*=\).*$/\1 examples/g' docs/doxygen.conf 
$ sed -i  's/\(INPUT *=\).*$/\1 README.md include/g' docs/doxygen.conf #устанавливает INPUT равным print
# Указание файла README.md как основого
$ sed -i  's/\(USE_MDFILE_AS_MAINPAGE.*=\).*$/\1 README.md/g' docs/doxygen.conf  
$ sed -i  's/\(OUTPUT_DIRECTORY.*=\).*$/\1 docs/g' docs/doxygen.conf #  Указание пути к каталогу doc
```

```ShellSession
$ sed -i '' 's/lab07/lab07/g' README.md
```

```ShellSession
# документируем функции print 
$ edit include/print.hpp
```
Коммит
```ShellSession
$ git add .
$ git commit -m"added doxygen.conf"
$ git push origin master
```
Подключение к Travis
```ShellSession
$ travis login --auto
$ travis enable
```
Работа с файлами doxygen
```
$ doxygen docs/doxygen.conf
$ ls | grep "[^docs]" | xargs rm -rf  # фильтр файла для удаления
$ mv docs/html/* . && rm -rf docs     # переименовывание и перемещение файла
$ git checkout -b gh-pages            # создание ветки gh-page
$ git add .
$ git commit -m"added documentation"  
$ git push origin gh-pages            
$ git checkout master                 # возврат к ветке master
```
Создание папки artifacts и добавление туда скриншота. Отправление на майл  rusdevops@gmail.com
```ShellSession
$ mkdir artifacts && cd artifacts
$ screencapture -T 10 screenshot.jpg # или png
<Command>-T
$ open https://${GITHUB_USERNAME}.github.io/lab07/print_8hpp_source.html    # открываем gh-page
$ gdrive upload screenshot.jpg # или png
$ SCREENSHOT_ID=`gdrive list | grep screenshot | awk '{ print $1; }'`
$ gdrive share ${SCREENSHOT_ID} --role reader --type user --email rusdevops@gmail.com
$ echo https://drive.google.com/open?id=${SCREENSHOT_ID} #вывод ссылки на скриншот
```

## Report

```ShellSession
$ cd ~/workspace/labs/
$ export LAB_NUMBER=07
$ git clone https://github.com/tp-labs/lab${LAB_NUMBER} tasks/lab${LAB_NUMBER}
$ mkdir reports/lab${LAB_NUMBER}
$ cp tasks/lab${LAB_NUMBER}/README.md reports/lab${LAB_NUMBER}/REPORT.md
$ cd reports/lab${LAB_NUMBER}
$ edit REPORT.md
$ gistup -m "lab${LAB_NUMBER}"
```

## Links

- [HTML](https://ru.wikipedia.org/wiki/HTML)
- [LAΤΕΧ](https://ru.wikipedia.org/wiki/LaTeX)
- [man](https://ru.wikipedia.org/wiki/Man_(%D0%BA%D0%BE%D0%BC%D0%B0%D0%BD%D0%B4%D0%B0_Unix))
- [CHM](https://ru.wikipedia.org/wiki/HTMLHelp)
- [PostScript](https://ru.wikipedia.org/wiki/PostScript)

```
Copyright (c) 2017 Братья Вершинины
```
