## Homework

Представьте, что вы стажер в компании "Formatter Inc.".
### Задание 1
Вам поручили перейти на систему автоматизированной сборки **CMake**.
Исходные файлы находятся в директории [formatter_lib](formatter_lib).
В этой директории находятся файлы для статической библиотеки *formatter*.
Создайте `CMakeList.txt` в директории [formatter_lib](formatter_lib),
с помощью которого можно будет собирать статическую библиотеку *formatter*.

1. В директории formatter_lib создадим CMakeLists.txt
```sh
cmake_minimum_required(VERSION 3.4)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

project(formatter)

add_library(formatter STATIC formatter.cpp)
```
2. Команда cmake_minimum_required указывает подходящую минимальную версию Cmake.
3. Команды set(CMAKE_CXX_STANDARD 11),set(CMAKE_CXX_STANDARD_REQUIRED ON) устанавливают значения стандартных переменных в Cmake
4. Команда project(formatter) создает проект formatter, к которому можно подключать библиотеки, исполняемы файлы и т.д.
5. Команда add_library(formatter STATIC formatter.cpp) создает статическую библиотеку из указываемых файлов.

### Задание 2
У компании "Formatter Inc." есть перспективная библиотека,
которая является расширением предыдущей библиотеки. Т.к. вы уже овладели
навыком созданием `CMakeList.txt` для статической библиотеки *formatter*, ваш 
руководитель поручает заняться созданием `CMakeList.txt` для библиотеки 
*formatter_ex*, которая в свою очередь использует библиотеку *formatter*.

1. В директории formatter_ex_lib создадим CMakeLists.txt.
```sh
cmake_minimum_required(VERSION 3.4)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

project(formatter_ex)

include_directories(../formatter_lib)
add_subdirectory(../formatter_lib formatter)

add_library(formatter_ex STATIC formatter_ex.cpp)

target_link_libraries(formatter_ex formatter)
```
2. Команда include_directories(../formatter_lib) указывает директорию заголовочных файлов библиотеки formatter.
3. Команда add_subdirectory(../formatter_lib formatter) включает в сборку директорию с библиотекой formatter.
4.  Команда target_link_libraries(formatter_ex formatter) связывает библиотеку formatter с formatter_ex.
5. Остальные команды аналогичны из первого задания.
### Задание 3
Конечно же ваша компания предоставляет примеры использования своих библиотек.
Чтобы продемонстрировать как работать с библиотекой *formatter_ex*,
вам необходимо создать два `CMakeList.txt` для двух простых приложений:
* *hello_world*, которое использует библиотеку *formatter_ex*;
* *solver*, приложение которое испольует статические библиотеки *formatter_ex* и *solver_lib*.

1. В директории hello_world_application создадим CMakeLists.txt.
```sh
cmake_minimum_required(VERSION 3.4)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

project(hello_world)

add_executable(hello_world hello_world.cpp)

include_directories(../formatter_ex_lib)
add_subdirectory(../formatter_ex_lib formatter_ex)

target_link_libraries(hello_world formatter_ex)
```
2. Команды add_executable(hello_world hello_world.cpp) добавляет в проект исполняемые файлы.
3. Остальные команды включают в данный проект библиотеку formatter_ex и связывают его с нашим проектом hello_world.
4. Для демонстрации введем необходимые команды.
```sh 
cmake -H. -B_build
cmake --build _build
cmake --build _build --target hello_world
./_build/hello_world
```
5. В директории solver_lib создадим CMakeLists.txt,аналогичный файлу для библиотеки formatter.
```sh
cmake_minimum_required(VERSION 3.4)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

project(solver_lib)

add_library(solver_lib STATIC solver.cpp)
```
6. В директории solver_application создадим CMakeLists.txt.
```sh
cmake_minimum_required(VERSION 3.4)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

project(solver)

add_executable(solver equation.cpp)

include_directories(../formatter_ex_lib ../solver_lib)

add_subdirectory(../formatter_ex_lib formatter_ex)
add_subdirectory(../solver_lib solver_lib)

target_link_libraries(solver formatter_ex solver_lib)
```
7. Этот файл аналогичен файлу для демонстрации приложения hello_world, но в данном проекте мы используем две библиотеки,поэтому указываем две директории заголовочных файлов и добавляем две директории с библиотеками.
8. Для демонстрации введем необходимые команды.
```sh
cmake -H. -B_build
cmake --build _build
cmake --build _build --target solver
./_build/solver
```
**Удачной стажировки!**

## Links
- [Основы сборки проектов на С/C++ при помощи CMake](https://eax.me/cmake/)
- [CMake Tutorial](http://neerc.ifmo.ru/wiki/index.php?title=CMake_Tutorial)
- [C++ Tutorial - make & CMake](https://www.bogotobogo.com/cplusplus/make.php)
- [Autotools](http://www.gnu.org/software/automake/manual/html_node/Autotools-Introduction.html)
- [CMake](https://cgold.readthedocs.io/en/latest/index.html)

```
Copyright (c) 2015-2021 The ISC Authors
```
