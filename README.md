# lab3
1)Вам поручили перейти на систему автоматизированной сборки CMake. Исходные файлы находятся в директории formatter_lib. В этой директории находятся файлы для статической библиотеки formatter. Создайте CMakeList.txt в директории formatter_lib, с помощью которого можно будет собирать статическую библиотеку formatter.
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
% cd _build 
% make
>>[ 50%] Building CXX object CMakeFiles/formatter.dir/formatter.cpp.o
[100%] Linking CXX static library libformatter.a
[100%] Built target formatter
```

Содержимое CMakeLists.txt: 

<img width="533" alt="Снимок экрана 2025-03-29 в 15 33 38" src="https://github.com/user-attachments/assets/d25157d1-bfee-471a-b156-88802231aeb6" />

2)У компании "Formatter Inc." есть перспективная библиотека, которая является расширением предыдущей библиотеки. Т.к. вы уже овладели навыком созданием CMakeList.txt для статической библиотеки formatter, ваш руководитель поручает заняться созданием CMakeList.txt для библиотеки formatter_ex, которая в свою очередь использует библиотеку formatter.
```
cat > CMakeLists.txt << 'EOF'
cmake_minimum_required(VERSION 3.10)
project(formatter_ex)
EOF
cat >> CMakeLists.txt << 'EOF'
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
EOF
cat >> CMakeLists.txt << 'EOF'
add_library(formatter_ex STATIC formatter_ex.cpp)
EOF
cat >> CMakeLists.txt << 'EOF'
target_include_directories(formatter_ex PUBLIC
EOF
cat >> CMakeLists.txt << 'EOF'
    ${CMAKE_CURRENT_SOURCE_DIR}
    ${CMAKE_CURRENT_SOURCE_DIR}/../formatter_lib
)
EOF

cat >> CMakeLists.txt << 'EOF'
add_subdirectory(../formatter_lib ${CMAKE_BINARY_DIR}/formatter)
target_link_libraries(formatter_ex formatter)
EOF
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
-- Configuring done (0.9s)
-- Generating done (0.0s)
-- Build files have been written to: /Users/makbuk/lab03/formatter_ex_lib/_build
% cmake -H. -B_build
% cd _build
% make
>>[ 25%] Building CXX object formatter/CMakeFiles/formatter.dir/formatter.cpp.o
[ 50%] Linking CXX static library libformatter.a
[ 50%] Built target formatter
[ 75%] Building CXX object CMakeFiles/formatter_ex.dir/formatter_ex.cpp.o
[100%] Linking CXX static library libformatter_ex.a
[100%] Built target formatter_ex
```
Содержимое CMakeLists.txt: 

<img width="443" alt="Снимок экрана 2025-03-29 в 16 47 00" src="https://github.com/user-attachments/assets/b86a799d-c10d-4296-b433-fbd6ee496617" />

3)Конечно же ваша компания предоставляет примеры использования своих библиотек. Чтобы продемонстрировать как работать с библиотекой formatter_ex, вам необходимо создать два CMakeList.txt для двух простых приложений:

hello_world, которое использует библиотеку formatter_ex;
solver, приложение которое испольует статические библиотеки formatter_ex и solver_lib.
```
% cd hello_world_application 
% cat >> CMakeLists.txt << 'EOF'
cmake_minimum_required(VERSION 3.10)
project(hello_world)
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
add_subdirectory(../formatter_ex_lib ${CMAKE_BINARY_DIR}/formatter_ex)
EOF
% cat >> CMakeLists.txt << 'EOF'
add_executable(hello_world hello_world.cpp)
EOF
% cat >> CMakeLists.txt << 'EOF'
target_link_libraries(hello_world formatter_ex)
EOF
% cat >> CMakeLists.txt << 'EOF'
target_include_directories(hello_world PRIVATE
    ${CMAKE_CURRENT_SOURCE_DIR}/../formatter_ex_lib
    ${CMAKE_CURRENT_SOURCE_DIR}/../formatter_lib
)
EOF
hello_world_application % cmake -H. -B_build
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
-- Configuring done (1.0s)
-- Generating done (0.0s)
-- Build files have been written to: /Users/makbuk/lab03/hello_world_application/_build
% cd _build
% make
>>[ 16%] Building CXX object formatter/CMakeFiles/formatter.dir/formatter.cpp.o
[ 33%] Linking CXX static library libformatter.a
[ 33%] Built target formatter
[ 50%] Building CXX object formatter_ex/CMakeFiles/formatter_ex.dir/formatter_ex.cpp.o
[ 66%] Linking CXX static library libformatter_ex.a
[ 66%] Built target formatter_ex
[ 83%] Building CXX object CMakeFiles/hello_world.dir/hello_world.cpp.o
[100%] Linking CXX executable hello_world
[100%] Built target hello_world
% ./hello_world
>>
-------------------------
hello, world!
-------------------------
makbuk@MacBook-Air-makbuk _build % 

```
Содержимое CMakeLists.txt:

<img width="555" alt="Снимок экрана 2025-03-30 в 15 20 46" src="https://github.com/user-attachments/assets/83c53d9f-4479-4634-b3a9-bcbb2d90e53c" />

```
solver_lib % cat > CMakeLists.txt << 'EOF'
cmake_minimum_required(VERSION 3.10)
project(solver_lib)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

add_library(solver_lib STATIC solver.cpp)

target_include_directories(solver_lib PUBLIC
    ${CMAKE_CURRENT_SOURCE_DIR}
)
EOF
% cmake -H. -B_build
% cd _build
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
-- Configuring done (1.1s)
-- Generating done (0.0s)
-- Build files have been written to: /Users/makbuk/lab03/solver_lib/_build
% make
>>[ 50%] Building CXX object CMakeFiles/solver_lib.dir/solver.cpp.o
[100%] Linking CXX static library libsolver_lib.a
[100%] Built target solver_lib
solver_application % cat > CMakeLists.txt << 'EOF'
cmake_minimum_required(VERSION 3.10)
project(solver)
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
add_subdirectory(../formatter_ex_lib ${CMAKE_BINARY_DIR}/formatter_ex)
add_subdirectory(../solver_lib ${CMAKE_BINARY_DIR}/solver_lib)
add_executable(solver equation.cpp)
target_link_libraries(solver formatter_ex solver_lib)
target_include_directories(solver PRIVATE
    ${CMAKE_CURRENT_SOURCE_DIR}/../formatter_lib
    ${CMAKE_CURRENT_SOURCE_DIR}/../formatter_ex_lib
    ${CMAKE_CURRENT_SOURCE_DIR}/../solver_lib
)
EOF
% cmake -H. -B_build
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
-- Configuring done (1.0s)
-- Generating done (0.0s)
-- Build files have been written to: /Users/makbuk/lab03/solver_application/_build
% cd _build
 % make
>>[ 12%] Building CXX object solver_lib/CMakeFiles/solver_lib.dir/solver.cpp.o
[ 25%] Linking CXX static library libsolver_lib.a
[ 25%] Built target solver_lib
[ 37%] Building CXX object formatter/CMakeFiles/formatter.dir/formatter.cpp.o
[ 50%] Linking CXX static library libformatter.a
[ 50%] Built target formatter
[ 62%] Building CXX object formatter_ex/CMakeFiles/formatter_ex.dir/formatter_ex.cpp.o
[ 75%] Linking CXX static library libformatter_ex.a
[ 75%] Built target formatter_ex
[ 87%] Building CXX object CMakeFiles/solver.dir/equation.cpp.o
[100%] Linking CXX executable solver
[100%] Built target solver
_build % ./solver
1 -5 6
>>
-------------------------
x1 = 2.000000
-------------------------
-------------------------
x2 = 3.000000
-------------------------
```
Содержимое CMakeLists.txt:

<img width="506" alt="Снимок экрана 2025-03-30 в 18 17 44" src="https://github.com/user-attachments/assets/62ac691c-e2da-4355-9ca4-f0fa01b6b6f2" />


