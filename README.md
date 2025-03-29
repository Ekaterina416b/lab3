# lab3
Вам поручили перейти на систему автоматизированной сборки CMake. Исходные файлы находятся в директории formatter_lib. В этой директории находятся файлы для статической библиотеки formatter. Создайте CMakeList.txt в директории formatter_lib, с помощью которого можно будет собирать статическую библиотеку formatter.
```
% cmake --version
>>cmake version 3.31.6
% git clone https://github.com/tp-labs/lab03.git
>>remote: Enumerating objects: 91, done.
remote: Counting objects: 100% (30/30), done.
remote: Compressing objects: 100% (9/9), done.
remote: Total 91 (delta 23), reused 21 (delta 21), pack-reused 61 (from 1)
Receiving objects: 100% (91/91), 1.02 MiB | 2.65 MiB/s, done.
Resolving deltas: 100% (41/41), done.
% cd ./formatter_lib/
% cat >> CMakeLists.txt <<EOF
cmake_minimum_required(VERSION 3.29.2)
project(formatter)
EOF
% cat >> CMakeLists.txt <<EOF
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
EOF
% cat >> CMakeLists.txt <<EOF
add_library(formatter STATIC \${CMAKE_CURRENT_SOURCE_DIR}/formatter.cpp)
EOF
% cat >> CMakeLists.txt <<EOF
include_directories(\${CMAKE_CURRENT_SOURCE_DIR})
EOF
% cmake -H. -B_build //настроила сборку юез компиляции
>>-- The C compiler identification is AppleClang 16.0.0.16000026
-- The CXX compiler identification is AppleClang 16.0.0.16000026
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working C compiler: /Library/Developer/CommandLineTools/usr/bin/cc - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Check for working CXX compiler: /Library/Developer/CommandLineTools/usr/bin/c++ - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done (1.9s)
-- Generating done (0.0s)
-- Build files have been written to: /Users/makbuk/lab03/formatter_lib/_build
```
Содержимое CMakeLists.txt:
<img width="533" alt="Снимок экрана 2025-03-29 в 15 33 38" src="https://github.com/user-attachments/assets/d25157d1-bfee-471a-b156-88802231aeb6" />
