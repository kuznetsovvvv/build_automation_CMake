## Лабораторная №3

# 1)Задание 1:
Вам поручили перейти на систему автоматизированной сборки CMake. Исходные файлы находятся в директории formatter_lib. В этой директории находятся файлы для статической библиотеки formatter. Создайте CMakeList.txt в директории formatter_lib, с помощью которого можно будет собирать статическую библиотеку formatter.


Создаем в дирректории formatter_lib файл CMakeLists.txt

Пишем в файл:
```bash

cmake_minimum_required(VERSION 3.4)
project(app)
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
add_library(formatter_lib STATIC ${CMAKE_CURRENT_SOURCE_DIR}/formatter.cpp)
```
Далее нужно проверить, что проект собирается:
прописываем команды в директории formatter_lib:
```bash
Команда:cmake -B build

Вывод:
-- The C compiler identification is GNU 11.4.0
-- The CXX compiler identification is GNU 11.4.0
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working C compiler: /usr/bin/cc - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Check for working CXX compiler: /usr/bin/c++ - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done
-- Generating done
-- Build files have been written to: /home/misha/dev/lab3/lab03/formatter_lib/build



Команда:cmake --build build

Вывод:[ 50%] Building CXX object CMakeFiles/formatter_lib.dir/formatter.cpp.o
[100%] Linking CXX static library libformatter_lib.a
[100%] Built target formatter_lib

```


# 2)Задание 2:
У компании "Formatter Inc." есть перспективная библиотека, которая является расширением предыдущей библиотеки. Т.к. вы уже овладели навыком созданием CMakeList.txt для статической библиотеки formatter, ваш руководитель поручает заняться созданием CMakeList.txt для библиотеки formatter_ex, которая в свою очередь использует библиотеку formatter.


Также создаем файл CMakeLists.txt в директории formatter_ex_lib:


```bash
cmake_minimum_required(VERSION 3.4)
project(formatter_ex_lib)
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
```
Прописываем стандартные команды в файл;

```bash
add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/../formatter_lib formatter_lib_dir)
```
Добавляем папку formatter_lib в formatter_lib_dir

```bash
target_include_directories(formatter_ex_lib PUBLIC
${CMAKE_CURRENT_SOURCE_DIR}/../formatter_lib
)
```
Открываем formatter_ex_lib доступ к директории formatter_lib

```bash
target_link_libraries(formatter_ex_lib formatter_lib)
```
Привязываем библиотеку formatter_lib к библиотеке formatter_ex_lib

Далее прописываем команды 
```bash
Команда:cmake -B build

Вывод:
-- The C compiler identification is GNU 11.4.0
-- The CXX compiler identification is GNU 11.4.0
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working C compiler: /usr/bin/cc - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Check for working CXX compiler: /usr/bin/c++ - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done
-- Generating done
-- Build files have been written to: /home/misha/dev/lab3/lab03/formatter_ex_lib/build


Команда:cmake --build build

Вывод:[ 25%] Building CXX object formatter_lib_dir/CMakeFiles/formatter_lib.dir/formatter.cpp.o
[ 50%] Linking CXX static library libformatter_lib.a
[ 50%] Built target formatter_lib
[ 75%] Building CXX object CMakeFiles/formatter_ex_lib.dir/formatter_ex.cpp.o
[100%] Linking CXX static library libformatter_ex_lib.a
[100%] Built target formatter_ex_lib
```

# 3)Задание 3:
Конечно же ваша компания предоставляет примеры использования своих библиотек. Чтобы продемонстрировать как работать с библиотекой formatter_ex, вам необходимо создать два CMakeList.txt для двух простых приложений:

   1. hello_world, которое использует библиотеку formatter_ex;
   2. solver, приложение которое испольует статические библиотеки formatter_ex и solver_lib.
---

1.В дирректории hello_world_application создаем CMakeLists.txt, чтобы привязать библиотеку formatter_ex_lib к файлу hello_world


```bash
cmake_minimum_required(VERSION 3.4)

project(hello_world)
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/../formatter_ex_lib formatter_ex_lib_dir)

add_executable(hello_world ${CMAKE_CURRENT_SOURCE_DIR}/hello_world.cpp)
# На этот раз вместо библиотеки создаём исполняемый файл. Синтаксис тот же.
target_include_directories(hello_world PUBLIC
${CMAKE_CURRENT_SOURCE_DIR}/../formatter_ex_lib
)

target_link_libraries(hello_world formatter_ex_lib)

```

Далее собираем файлы
```bash
Команда:cmake -B build

Вывод:
-- The C compiler identification is GNU 11.4.0
-- The CXX compiler identification is GNU 11.4.0
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working C compiler: /usr/bin/cc - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Check for working CXX compiler: /usr/bin/c++ - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done
-- Generating done
-- Build files have been written to: /home/misha/dev/lab3/lab03/hello_world_application/build


Команда:cmake --build build

Вывод:
[ 16%] Building CXX object formatter_ex_lib_dir/formatter_lib_dir/CMakeFiles/formatter_lib.dir/formatter.cpp.o
[ 33%] Linking CXX static library libformatter_lib.a
[ 33%] Built target formatter_lib
[ 50%] Building CXX object formatter_ex_lib_dir/CMakeFiles/formatter_ex_lib.dir/formatter_ex.cpp.o
[ 66%] Linking CXX static library libformatter_ex_lib.a
[ 66%] Built target formatter_ex_lib
[ 83%] Building CXX object CMakeFiles/hello_world.dir/hello_world.cpp.o
[100%] Linking CXX executable hello_world
[100%] Built target hello_world

```

Далее у нас появляется исполняемый файл hello_world в папке build. 
```bash
Команда:build/hello_world

Вывод:
-------------------------
hello, world!
-------------------------
```

2. Собираем библиотеку solver_lib и исправляем код в файле solver.cpp:

прописываем библиотеку:

```bash
#include <cmath>
```

Проводим сборку:

Создаем в дирректории solver_lib файл CMakeLists.txt

Пишем в файл:
```bash

cmake_minimum_required(VERSION 3.4)
project(solver_lib)
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
add_library(solver_lib ${CMAKE_CURRENT_SOURCE_DIR}/solver.cpp)
```


Создаем в дирректории solver_application файл CMakeLists.txt

Пишем в файл:
```bash

cmake_minimum_required(VERSION 3.4)
project(solver)
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/../formatter_ex_lib formatter_ex_lib_dir)
add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/../solver_lib solver_lib_dir)
add_executable(solver ${CMAKE_CURRENT_SOURCE_DIR}/equation.cpp)
target_include_directories(formatter_ex_lib PUBLIC
${CMAKE_CURRENT_SOURCE_DIR}/../formatter_ex_lib
${CMAKE_CURRENT_SOURCE_DIR}/../solver_lib
)
target_link_libraries(solver formatter_ex_lib solver_lib)
```
Мы подключаем две библиотеки:
formatter_ex_lib
solver_lib
к исполняемому проекту solver

```bash
Команда:cmake -B build

Вывод:
-- The C compiler identification is GNU 11.4.0
-- The CXX compiler identification is GNU 11.4.0
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working C compiler: /usr/bin/cc - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Check for working CXX compiler: /usr/bin/c++ - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done
-- Generating done
-- Build files have been written to: /home/misha/dev/lab3/lab03/solver_application/build



Команда:cmake --build build

Вывод:
[ 12%] Building CXX object solver_lib_dir/CMakeFiles/solver_lib.dir/solver.cpp.o
/home/misha/dev/lab3/lab03/solver_lib/solver.cpp: In function ‘void solve(float, float, float, float&, float&)’:
/home/misha/dev/lab3/lab03/solver_lib/solver.cpp:14:21: error: ‘sqrtf’ is not a member of ‘std’; did you mean ‘sqrt’?
   14 |     x1 = (-b - std::sqrtf(d)) / (2 * a);
      |                     ^~~~~
      |                     sqrt
/home/misha/dev/lab3/lab03/solver_lib/solver.cpp:15:21: error: ‘sqrtf’ is not a member of ‘std’; did you mean ‘sqrt’?
   15 |     x2 = (-b + std::sqrtf(d)) / (2 * a);
      |                     ^~~~~
      |                     sqrt
gmake[2]: *** [solver_lib_dir/CMakeFiles/solver_lib.dir/build.make:76: solver_lib_dir/CMakeFiles/solver_lib.dir/solver.cpp.o] Ошибка 1
gmake[1]: *** [CMakeFiles/Makefile2:215: solver_lib_dir/CMakeFiles/solver_lib.dir/all] Ошибка 2
gmake: *** [Makefile:91: all] Ошибка 2
```
Вылезла ошибка из-за того, что в .cpp файле sqrtf не принадлежит библиотеке std. Исправляем в файле solver.cpp эту ошибку и повторяем:

```bash
Команда:cmake --build build

Вывод:Consolidate compiler generated dependencies of target solver_lib
[ 12%] Building CXX object solver_lib_dir/CMakeFiles/solver_lib.dir/solver.cpp.o
[ 25%] Linking CXX static library libsolver_lib.a
[ 25%] Built target solver_lib
[ 37%] Building CXX object formatter_ex_lib_dir/formatter_lib_dir/CMakeFiles/formatter_lib.dir/formatter.cpp.o
[ 50%] Linking CXX static library libformatter_lib.a
[ 50%] Built target formatter_lib
[ 62%] Building CXX object formatter_ex_lib_dir/CMakeFiles/formatter_ex_lib.dir/formatter_ex.cpp.o
[ 75%] Linking CXX static library libformatter_ex_lib.a
[ 75%] Built target formatter_ex_lib
[ 87%] Building CXX object CMakeFiles/solver.dir/equation.cpp.o
[100%] Linking CXX executable solver
[100%] Built target solver


```

Теперь собираем файл:
```bash 
Команда:build/solver

Вывод после ввода 3 коэффициентов:
3 3 3
-------------------------
error: discriminant < 0
-------------------------
```
Получаем решение уравнения.

