[![Build Status](https://travis-ci.org/anasteyshakoshman/lab06.svg?branch=master)](https://travis-ci.org/anasteyshakoshman/lab06)

## Laboratory work VI

Данная лабораторная работа посвещена изучению фреймворков для тестирования на примере **Catch**

```ShellSession
$ open https://github.com/philsquared/Catch
```

## Tasks

- [x] 1. Создать публичный репозиторий с названием **lab06** на сервисе **GitHub**
- [x] 2. Выполнить инструкцию учебного материала
- [x] 3. Ознакомиться со ссылками учебного материала
- [x] 4. Составить отчет и отправить ссылку личным сообщением в **Slack**

## Tutorial

```ShellSession
$ export GITHUB_USERNAMEanasteyshakoshman
```

Создание директории лабораторной на основе предыдущей
```ShellSession
$ git clone https://github.com/${GITHUB_USERNAME}/Lab05 lab06
$ cd lab06
$ git remote remove origin
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab06
```

Скачивание библиотеки Catch и создание main с включенной билиотекой
```ShellSession
$ mkdir tests #создание директории 
#получение catch.hpp с сайта в директорию tests
$ wget https://github.com/philsquared/Catch/releases/download/v1.9.3/catch.hpp -O tests/catch.hpp 
$ cat > tests/main.cpp <<EOF # создание и редактирование файла main.cpp в tests
#define CATCH_CONFIG_MAIN
#include "catch.hpp"
EOF
```

Создание CMakeLists.txt
```ShellSession
$ sed -i '/option(BUILD_EXAMPLES "Build examples" OFF)/a\ #добавление строки в потоковый редактор
option(BUILD_TESTS "Build tests" OFF)
' CMakeLists.txt
$ cat >> CMakeLists.txt <<EOF # редактирование 
if(BUILD_TESTS)
	enable_testing() #включение теста
	file(GLOB \${PROJECT_NAME}_TEST_SOURCES tests/*.cpp)   # поиск файла по заданному шаблону
	add_executable(check \${\${PROJECT_NAME}_TEST_SOURCES})
	target_link_libraries(check \${PROJECT_NAME} \${DEPENDS_LIBRARIES})
	#параметры теста. -s = успешные выполнения теста. -r compact = формат вывода
	add_test(NAME check COMMAND check "-s" "-r" "compact" "--use-colour" "yes") 
endif()
EOF
```

Создание теста
```ShellSession
$ cat >> tests/test1.cpp <<EOF
#include "catch.hpp"
#include <print.hpp>

TEST_CASE("output values should match input values", "[file]") {
  std::string text = "hello";
  std::ofstream out("file.txt");
  
  print(text, out);
  out.close();
  
  std::string result;
  std::ifstream in("file.txt");
  in >> result;
  
  REQUIRE(result == text);
}
EOF
```

Сборка проекта и запуск теста после сборки
```ShellSession
$ cmake -H. -B_build -DCMAKE_INSTALL_PREFIX=_install -DBUILD_TESTS=ON
$ cmake --build _build
$ cmake --build _build --target test 
Running tests...
Test project /home/ilya/lab06/_build
    Start 1: check
1/1 Test #1: check ............................  Passed   0.03 sec.

100% tests passed, 0 tests faild out of 1

Total Test time (real) =  0.04 sec

```

Изменение файлов
```ShellSession
$ sed -i 's/lab05/lab06/g' README.md # замена "lab05" на "lab06"
$ sed -i 's/\(DCMAKE_INSTALL_PREFIX=_install\)/\1 -DBUILD_TESTS=ON/' .travis.yml # дополнение
$ sed -i '/cmake --build _build --target install/a\ #добавление строки после данной
- cmake --build _build --target test #запуск теста после сборки
' .travis.yml
```

Проверка  .travis.yml
```ShellSession
$ travis lint
```

Коммит
```ShellSession
$ git add .
$ git commit -m"added tests"
$ git push origin master
```

Активация проекта
```ShellSession
$ travis login --auto #Вход в travis
$ travis enable
```

Снимок экрана и сохранение в созданную папку
```ShellSession
$ mkdir artifacts
$ screencapture -T 20 artifacts/screenshot.jpg
<Command>-T
$ open https://github.com/${GITHUB_USERNAME}/lab06
```

## Report

```ShellSession
Создание отчета
$ cd ~/workspace/labs/
$ export LAB_NUMBER=06
$ git clone https://github.com/tp-labs/lab${LAB_NUMBER} tasks/lab${LAB_NUMBER}
$ mkdir reports/lab${LAB_NUMBER}
$ cp tasks/lab${LAB_NUMBER}/README.md reports/lab${LAB_NUMBER}/REPORT.md
$ cd reports/lab${LAB_NUMBER}
$ edit REPORT.md
$ gistup -m "lab${LAB_NUMBER}"
```
```










