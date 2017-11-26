## Laboratory work VIII

Данная лабораторная работа посвещена изучению средств пакетирования на примере **CPack**

```ShellSession
$ open https://cmake.org/Wiki/CMake:CPackPackageGenerators
```

## Tasks

- [x] 1. Создать публичный репозиторий с названием **lab08** на сервисе **GitHub**
- [x] 2. Выполнить инструкцию учебного материала
- [x] 3. Ознакомиться со ссылками учебного материала
- [x] 4. Составить отчет и отправить ссылку личным сообщением в **Slack**

## Tutorial

```ShellSession
$ export GITHUB_USERNAME=anasteyshakoshman
$ export GITHUB_EMAIL=anastasiay.andreevnaa@gmail.com
$ alias edit=vim
$ alias gsed=sed # for *-nix system   -используется для создания альтернативных версий обычных команд, присваивая им новые названия, а также чтобы избежать необходимости запоминать длинные и сложные команды 
```

```ShellSession
$ git clone https://github.com/${GITHUB_USERNAME}/lab07 lab08
$ cd lab08
$ git remote remove origin
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab8    - добавить новый удалённый Git-репозиторий под именем-сокращением
```

Редактирование `CMakeLists.txt`, устанавливая новые версии `MAJOR`, `TWEAK` (уровень функции), `PATCH`, `MINOR`(либо уровень исправления ошибок, либо дату разработки.)
sed - потоковый редактор
```ShellSession
$ gsed -i '/project(print)/a\
set(PRINT_VERSION_STRING "v${PRINT_VERSION}")
' CMakeLists.txt
$ gsed -i '/project(print)/a\
set(PRINT_VERSION \
\${PRINT_VERSION_MAJOR}.\${PRINT_VERSION_MINOR}.\${PRINT_VERSION_PATCH}.\${PRINT_VERSION_TWEAK})
' CMakeLists.txt
$ gsed -i '/project(print)/a\
set(PRINT_VERSION_TWEAK 0)
' CMakeLists.txt
$ gsed -i '/project(print)/a\
set(PRINT_VERSION_PATCH 0)
' CMakeLists.txt
$ gsed -i '/project(print)/a\
set(PRINT_VERSION_MINOR 1)
' CMakeLists.txt
$ gsed -i '/project(print)/a\
set(PRINT_VERSION_MAJOR 0)
' CMakeLists.txt
```

Создание файлов `DESCRIPTION` и `ChangeLog.md`
```ShellSession
$ touch DESCRIPTION && edit DESCRIPTION — команда Unix, предназначенная для установки времени последнего изменения файла или доступа в текущее время. Также используется для создания пустых файлов. 
$ touch ChangeLog.md
$ export DATE="`LANG=en_US date +'%a %b %d %Y'`"   - добавление даты, имени пользователя и электронной почты
$ cat > ChangeLog.md <<EOF
* ${DATE} ${GITHUB_USERNAME} <${GITHUB_EMAIL}> 0.1.0.0
- Initial RPM release
EOF
```

Пути к файлам
```ShellSession
$ cat > CPackConfig.cmake <<EOF
include(InstallRequiredSystemLibraries)   - подключение библиотек
EOF
```

Установка значений почты, версий MAJOR MINOR PATCH TWEAK, печатания, пути к DESCRIPTION, комментария
```ShellSession
$ cat >> CPackConfig.cmake <<EOF
set(CPACK_PACKAGE_CONTACT ${GITHUB_EMAIL})
set(CPACK_PACKAGE_VERSION_MAJOR \${PRINT_VERSION_MAJOR})
set(CPACK_PACKAGE_VERSION_MINOR \${PRINT_VERSION_MINOR})
set(CPACK_PACKAGE_VERSION_PATCH \${PRINT_VERSION_PATCH})
set(CPACK_PACKAGE_VERSION_TWEAK \${PRINT_VERSION_TWEAK})
set(CPACK_PACKAGE_VERSION \${PRINT_VERSION})
set(CPACK_PACKAGE_DESCRIPTION_FILE \${CMAKE_CURRENT_SOURCE_DIR}/DESCRIPTION)
set(CPACK_PACKAGE_DESCRIPTION_SUMMARY "static c++ library for printing")
EOF
```

```ShellSession
$ cat >> CPackConfig.cmake <<EOF

set(CPACK_RESOURCE_FILE_LICENSE \${CMAKE_CURRENT_SOURCE_DIR}/LICENSE)
set(CPACK_RESOURCE_FILE_README \${CMAKE_CURRENT_SOURCE_DIR}/README.md)
EOF
```

Установка параметров: имени пакета , лицинзии, группы
```ShellSession
$ cat >> CPackConfig.cmake <<EOF

set(CPACK_RPM_PACKAGE_NAME "print-devel")
set(CPACK_RPM_PACKAGE_LICENSE "MIT")
set(CPACK_RPM_PACKAGE_GROUP "print")
set(CPACK_RPM_PACKAGE_URL "https://github.com/${GITHUB_USERNAME}/lab07")
set(CPACK_RPM_CHANGELOG_FILE \${CMAKE_CURRENT_SOURCE_DIR}/ChangeLog.md)
set(CPACK_RPM_PACKAGE_RELEASE 1)
EOF
```

Установка параметров: имя пакета домашняя страница doxy, версии cmake, релиза
```ShellSession
$ cat >> CPackConfig.cmake <<EOF

set(CPACK_DEBIAN_PACKAGE_NAME "libprint-dev")
set(CPACK_DEBIAN_PACKAGE_HOMEPAGE "https://${GITHUB_USERNAME}.github.io/lab07")
set(CPACK_DEBIAN_PACKAGE_PREDEPENDS "cmake >= 3.0")
set(CPACK_DEBIAN_PACKAGE_RELEASE 1)
EOF
```

Подключение cpack
```ShellSession
$ cat >> CPackConfig.cmake <<EOF

include(CPack)
EOF
```

Подключение CPackConfig.cmake
```ShellSession
$ cat >> CMakeLists.txt <<EOF

include(CPackConfig.cmake)
EOF
```

Замена lab07 на lab8
```ShellSession
$ gsed -i 's/lab07/lab8/g' README.md
```

```ShellSession
$ git add .  - - начать отслеживать (добавить под версионный контроль) новый файл
$ git commit -m"added cpack config"   - фиксировать изменения 
$ git push origin master    -отправить вашу ветку master на сервер origin
```

Интуграция с Travis
```ShellSession
$ travis login --auto
$ travis enable
```


CMake - кроссплатформенная система автоматизации сборки программного обеспечения из исходного кода. CMake не занимается непосредственно сборкой, a лишь генерирует файлы управления сборкой из файлов CMakeLists.txt
в состав CMake включены система автоматизированной сборки пакетов программного обеспечения CPack и модуль CMake для взаимодействия с ней. Система позволяет создавать пакеты программного обеспечения для популярных пакетных менеджеров (DEB, RPM, DMG), средства установки программного обеспечения (NSIS для Microsoft Windows), а также архивы со сборкой (TGZ, TBZ2, ZIP, самораспаковывающийся TGZ)
```ShellSession
$ cmake -H. -B_build
$ cmake --build _build
$ cd _build
$ cpack -G "TGZ"
$ cpack -G "RPM"
$ cpack -G "DEB"
$ cpack -G "NSIS"
$ cpack -G "DragNDrop"
$ cd ..
```

```ShellSession
$ cmake -H. -B_build -DCPACK_GENERATOR="TGZ"
$ cmake --build _build --target package
```

```ShellSession
$ mkdir artifacts
$ mv _build/*.tar.gz artifacts    -  для перемещения или переименования файлов 
$ tree artifacts
artifacts
└── print-..0.0-Linux.tar.gz
```

## Report

```ShellSession
$ export LAB_NUMBER=8
$ git clone https://github.com/tp-labs/lab08 tasks/lab${LAB_NUMBER}
$ mkdir reports/lab${LAB_NUMBER}
$ cp tasks/lab${LAB_NUMBER}/README.md reports/lab${LAB_NUMBER}/REPORT.md
$ cd reports/lab${LAB_NUMBER}
$ edit REPORT.md
$ gistup -m "lab${LAB_NUMBER}"
```

## Links

- [DMG](https://cmake.org/cmake/help/latest/module/CPackDMG.html)
- [DEB](https://cmake.org/cmake/help/latest/module/CPackDeb.html)
- [RPM](https://cmake.org/cmake/help/latest/module/CPackRPM.html)
- [NSIS](https://cmake.org/cmake/help/latest/module/CPackNSIS.html)

```
Copyright (c) 2017 Братья Вершинины
```
