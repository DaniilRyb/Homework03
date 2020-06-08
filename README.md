## Homework III

Представьте, что вы стажер в компании "Formatter Inc.".

### Задание 1

```sh
% cd formatter_lib

% cat > CMakeLists.txt <<EOF
cmake_minimum_required(VERSION 3.10)
project(formatter)
EOF
```
Устанавливаем стандарт языка и делаем его обязательным
```sh
% cat >> CMakeLists.txt <<EOF

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
EOF
```

```sh
% cat >> CMakeLists.txt <<EOF

add_library(formatter STATIC \${CMAKE_CURRENT_SOURCE_DIR}/formatter.cpp)
EOF
```

``` sh
% cat >> CMakeLists.txt <<EOF

include_directories(\${CMAKE_CURRENT_SOURCE_DIR})
EOF
```

```sh
% cmake -H. -B_build
Building for: Visual Studio 15 2017
-- Selecting Windows SDK version 10.0.17763.0 to target Windows 6.1.7601.
-- The C compiler identification is MSVC 19.16.27034.0
-- The CXX compiler identification is MSVC 19.16.27034.0
-- Check for working C compiler: C:/Program Files (x86)/Microsoft Visual Studio/2017/Enterprise/VC/Tools/MSVC/14.16.27023/bin/Hostx86/x86/cl.exe
-- Check for working C compiler: C:/Program Files (x86)/Microsoft Visual Studio/2017/Enterprise/VC/Tools/MSVC/14.16.27023/bin/Hostx86/x86/cl.exe - works
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Detecting C compile features
-- Detecting C compile features - done
-- Check for working CXX compiler: C:/Program Files (x86)/Microsoft Visual Studio/2017/Enterprise/VC/Tools/MSVC/14.16.27023/bin/Hostx86/x86/cl.exe
-- Check for working CXX compiler: C:/Program Files (x86)/Microsoft Visual Studio/2017/Enterprise/VC/Tools/MSVC/14.16.27023/bin/Hostx86/x86/cl.exe - works
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done
-- Generating done
-- Build files have been written to: C:/Users/Daniil-PC/Daniil_Rip/Daniil_Rip/DaniilRyb/workspace/Homework3/formatter_lib/_build
$ cmake --build _build
Microsoft (R) Build Engine версии 15.9.21+g9802d43bc3 для .NET Framework
(C) Корпорация Майкрософт (Microsoft Corporation). Все права защищены.

  Checking Build System
  Building Custom Rule C:/Users/Daniil-PC/Daniil_Rip/Daniil_Rip/DaniilRyb/workspace/Homework3/formatter_lib/CMakeLists.txt
  formatter.cpp
  formatter.vcxproj -> C:\Users\Daniil-PC\Daniil_Rip\Daniil_Rip\DaniilRyb\workspace\Homework3\formatter_lib\_build\Debug\formatter.lib
  Building Custom Rule C:/Users/Daniil-PC/Daniil_Rip/Daniil_Rip/DaniilRyb/workspace/Homework3/formatter_lib/CMakeLists.txt
```
Убеждаемся, что статическая библиотека *formatter* действительно создана.
```sh
% ls _build/Debug/formatter.a
_build/Debug/formatter.a
```


### Задание 2

```sh
% cd formatter_ex_lib

% cat > CMakeLists.txt <<EOF
cmake_minimum_required(VERSION 3.10)
project(formatter_ex)
EOF
```

```sh
% cat >> CMakeLists.txt <<EOF

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CURRENT_SOURCE_DIR /Users/Daniil_Rio/DaniilRyp/workspace/projects/Homework3)
EOF
```

```sh
% cat >> CMakeLists.txt <<EOF

add_library(formatter_ex STATIC \${CMAKE_CURRENT_SOURCE_DIR}/formatter_ex.cpp)
EOF
```

``` sh
% cat >> CMakeLists.txt <<EOF

include_directories(\${CMAKE_CURRENT_SOURCE_DIR})
EOF
```
``` sh
% cat >> CMakeLists.txt <<EOF

include_directories(\${CMAKE_CURRENT_SOURCE_DIR}/formatter_lib)
target_link_libraries(formatter_ex formatter)
EOF
```

```sh
% cmake -H. -B_build

% cmake --build _build

```

```sh
% ls _build/libformatter_ex.a
_build/libformatter_ex.a
```

### Задание 3

```sh
% cd hello_world_application

% cat > CMakeLists.txt <<EOF
cmake_minimum_required(VERSION 3.10)
project(hello_world)
EOF
```

```sh
% cat >> CMakeLists.txt <<EOF

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CURRENT_SOURCE_DIR /Users/Daniil_Rip/DaniilRyb/workspace/projects/Homework3)
EOF
```

```sh
% cat >> CMakeLists.txt <<EOF

add_executable(hello_world \${CMAKE_CURRENT_SOURCE_DIR}/hello_world_application/hello_world.cpp)
EOF
```

```sh
% cat >> CMakeLists.txt <<EOF

include_directories(\${CMAKE_CURRENT_SOURCE_DIR}/formatter_lib \${CMAKE_CURRENT_SOURCE_DIR}/formatter_ex_lib)

find_library(formatter NAMES libformatter.a PATHS \${CMAKE_CURRENT_SOURCE_DIR}/formatter_lib)
find_library(formatter_ex NAMES libformatter_ex.a PATHS \${CMAKE_CURRENT_SOURCE_DIR}/formatter_ex_lib)
EOF
```
и
```sh
% cat >> CMakeLists.txt <<EOF

target_link_libraries(hello_world \${formatter} \${formatter_ex})
EOF
```

```sh

% cmake -H. -B_build

% cmake --build _build

```

```sh
% _build/hello_world &&
-------------------------
hello, world!
-------------------------

```


```sh
% cd solver_application

% cat > CMakeLists.txt <<EOF
cmake_minimum_required(VERSION 3.10)
project(solver)
EOF
```

```sh
% cat >> CMakeLists.txt <<EOF

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CURRENT_SOURCE_DIR /Users/Daniil_Rip/DaniilRyb/workspace/projects/Homework3)
EOF
```

```sh
% cat >> CMakeLists.txt <<EOF

add_library(solver_lib STATIC \${CMAKE_CURRENT_SOURCE_DIR}/solver_lib/solver.cpp)
EOF
```

``` sh
% cat >> CMakeLists.txt <<EOF

include_directories(${CMAKE_CURRENT_SOURCE_DIR}/solver_lib ${CMAKE_CURRENT_SOURCE_DIR}/formatter_lib ${CMAKE_CURRENT_SOURCE_DIR}/formatter_ex_lib)
EOF
```

```sh
% cmake -H. -B_build

% cmake --build _build

```

```sh
% cat >> CMakeLists.txt <<EOF

add_executable(solver \${CMAKE_CURRENT_SOURCE_DIR}/solver_application/equation.cpp)
EOF
```

```sh
% cat >> CMakeLists.txt <<EOF

find_library(formatter NAMES libformatter.a PATHS \${CMAKE_CURRENT_SOURCE_DIR}/formatter_lib)
find_library(formatter_ex NAMES libformatter_ex.a PATHS \${CMAKE_CURRENT_SOURCE_DIR}/formatter_ex_lib)
find_library(solver_lib NAMES libsolver_lib.a PATHS \${CMAKE_CURRENT_SOURCE_DIR}/solver_lib)
EOF
```

```sh
% cat >> CMakeLists.txt <<EOF

target_link_libraries(solver \${formatter} \${formatter_ex} \${solver_lib})
EOF
```

```sh
#
% cmake -H. -B_build
% cmake --build _build

```

```sh
$ cmake --build _build 

$ cmake --build _build --target solver_lib # Сборка только статической библиотеки
[100%] Built target solver_lib
$ cmake --build _build --target solver # Сборка solver

```
Запускаем проект
```sh
% _build/solver && echo
1 5 6
-------------------------
x1 = -3.000000
-------------------------
-------------------------
x2 = -2.000000
-------------------------

```

**Удачной стажировки!**

## Links
- [Основы сборки проектов на С/C++ при помощи CMake](https://eax.me/cmake/)
- [CMake Tutorial](http://neerc.ifmo.ru/wiki/index.php?title=CMake_Tutorial)
- [C++ Tutorial - make & CMake](https://www.bogotobogo.com/cplusplus/make.php)
- [Autotools](http://www.gnu.org/software/automake/manual/html_node/Autotools-Introduction.html)
- [CMake](https://cgold.readthedocs.io/en/latest/index.html)

```
Copyright (c) 2015-2020 The ISC Authors
```
