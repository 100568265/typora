# 1.可执行文件



## **1.1 单个源文件**

**注意：**源文件和CMakeLists.txt需要放在相同的目录中。

创建build目录，在build目录下配置项目：

```cmake
#进入build目录，并通过指定CMakeLists.txt的位置来调用CMake
cmake ..
#该命令是跨平台的，-H表示当前目录中搜索根CMakeLists.txt文件
#-Bbuild告诉CMake在一个名为build的目录中生成所有的文件
cmake -H -Bbuild
```





## 1.2切换生成器

用**-G**切换生成器。

```cmake
mkdir -p build
cd build
cmake -G Ninja
```





## 1.3静态/动态库

先把它们编译成一个库，而不是直接编译成可执行文件：

**1.创建目标--静态库。库的名称和源码文件名相同：**

```cmake
add_library(message
	STATIC
		Message.hpp
		Message.cpp
)
```

**2.创建hello-world可执行文件的目标部分不需要修改：**

```cmake
add_executable(hello-world hello-world.cpp)
```

**3.最后，将目标库链接到可执行目标：**

```cmake
target_link_libraries(hello-world message)
```

**4.对项目进行配置和构建，库编译完成后，将连接到hello-world可执行文件中：**

```cmake
mkdir -p build
cd build
cmake ..
cmake --build .
```



**工作原理：**

`add_library(message STATIC Message.hpp Message.cpp)`：

生成必要的构建指令，将指定的源码编译到库中。生成的库的实际名由CMake添加前缀**lib**和适当的扩展名。

生成库根据第二个参数**STATIC**或**SHARED**和操作系统确定的。

`target_link_libraries(hello-world message)`:

将库链接到可执行文件。

**STATIC：**静态库，即编译文件的打包存档，以便在链接其他目标时使用

**SHARED：**可以动态链接，并在运行时加载的库。





## 1.4指定编译器

CMAKE_LANG_COMPILER变量中，其中LANG是受支持的任何语言

```c
cmake -D CMAKE_CXX_COMPILER = clang++ ..
```

通过导出环境变量CXX,CC。例如，使用这个命令使用clang++作为C++编译器：

```c
env CXX=clang++ cmake ..
```



**更多信息：**

CMAKE提供`--system-information`标志，它将把关于系统的所有信息转储到屏幕或文件中。要查看这个信息，请尝试以下操作：

```cmake
cmake --system-information information.txt
```









## 1.5条件语句

```cmake
if(USE_LIBRARY)
    # add_library will create a static library
    # since BUILD_SHARED_LIBS is OFF
    add_library(message ${_sources})
    add_executable(hello-world hello-world.cpp)
    target_link_libraries(hello-world message)
else()
    add_executable(hello-world hello-world.cpp ${_sources})
endif()
```





## 1.6显示选项

看一下前面示例中的静态/动态库示例。与其硬编码`USE_LIBRARY`为`ON`或`OFF`，现在为其设置一个默认值，同时也可以从外部进行更改：

1.用一个选项替换上一个示例的`set(USE_LIBRARY OFF)`命令。该选项将修改`USE_LIBRARY`的值，并设置其默认值为`OFF`：

```cmake
option(USE_LIBRARY "Compile sources into a library" OFF)
```

2.通过CMake的-D选项，将信息传递给CMake来切换库的行为：

```cmake
mkdir -p build
cd build
cmake -D USE_LIBRARY=ON ..

--...
--Compile sources into a library? ON
-- ...
$ cmake --build .
```

-D开关用于为CMake设置任何类型的变量：逻辑变量，路径等。



**工作原理：**

option可接受三个参数

```CMAKE
option(<option_variable> "help string" [initial value])
```

- `<option_variable>`表示该选项的变量的名称。
- `"help string"`记录选项的字符串，在CMake的终端或图形用户界面中可见。
- `[initial value]`选项的默认值，可以是`ON`或`OFF`。





## 1.7构建类型

CMake可以配置构建类型，例如：Debug，Release等。

控制生成构建系统使用的配置变量是`CMAKE_BUILD_TYPE`。该变量默认为空，CMake识别的值为:

1. **Debug**：用于在没有优化的情况下，使用带有调试符号构建库或可执行文件。
2. **Release**：用于构建的优化的库或可执行文件，不包含调试符号。
3. **RelWithDebInfo**：用于构建较少的优化库或可执行文件，包含调试符号。
4. **MinSizeRel**：用于不增加目标代码大小的优化方式，来构建库或可执行文件。

```cmake
cmake -D CMAKE_BUILD_TYPE=Debug ..
```





## 1.8编译器选项

警告标志：-Wall，-Wextra，-Wpedantic

-fPIC标志

**可见性级别：**

**INTERFACE：**给定的编译选项将只应用于指定目标，并传递给与目标相关的目标。

**PUBLIC：**编译选项将应用于指定目标和使用它的目标。

**PRIVATE：**编译选项会应用于给定的目标，不会传递给与目标相关的目标。



```cmake
#1.设置CMake的最低版本:
cmake_minimum_required(VERSION 3.5 FATAL_ERROR)
#2.声明项目名称和语言:
project(recipe-08 LANGUAGES CXX)
#3.然后，打印当前编译器标志。CMake将对所有C++目标使用这些:
message("C++ compiler flags: ${CMAKE_CXX_FLAGS}")
#4.为目标准备了标志列表，其中一些将无法在Windows上使用:
list(APPEND flags "-fPIC" "-Wall")
if(NOT WIN32)
  list(APPEND flags "-Wextra" "-Wpedantic")
endif()
#5添加了一个新的目标——geometry库，并列出它的源依赖关系:
add_library(geometry
  STATIC
    geometry_circle.cpp
    geometry_circle.hpp
    geometry_polygon.cpp
    geometry_polygon.hpp
    geometry_rhombus.cpp
    geometry_rhombus.hpp
    geometry_square.cpp
    geometry_square.hpp
  )
#6.为这个库目标设置了编译选项：
target_compile_options(geometry
  PRIVATE
    ${flags}
  )
#7.然后，将生成compute-areas可执行文件作为一个目标:
add_executable(compute-areas compute-areas.cpp)
#8.还为可执行目标设置了编译选项:
target_compile_options(compute-areas
  PRIVATE
    "-fPIC"
  )
#9.最后，将可执行文件链接到geometry库:
target_link_libraries(compute-areas geometry)
```





## 1.9设语言标准

```cmake
add_executable(animal-farm animal-farm.cpp)
set_target_properties(animal-farm
  PROPERTIES
    CXX_STANDARD 14
    CXX_EXTENSIONS OFF
    CXX_STANDARD_REQUIRED ON
  )
```

- **CXX_STANDARD**会设置我们想要的标准。

- **CXX_EXTENSIONS**告诉CMake，只启用`ISO C++`标准的编译器标志，而不使用特定编译器的扩展。

- **CXX_STANDARD_REQUIRED**指定所选标准的版本。如果这个版本不可用，CMake将停止配置并出现错误。





# 2.检测环境



## 2.1操作系统

根据检测到的操作系统信息打印消息：

```cmake
if(CMAKE_SYSTEM_NAME STREQUAL "Linux")
	message(STATUS "Configuring on/for Linux")
elseif(CMAKE_SYSTEM_NAME STREQUAL "Windows")
    message(STATUS "Configuring on/for Windows")
else()
    message(STATUS "Configuring on/for ${CMAKE_SYSTEM_NAME}")
endif()
```

CMake为目标操作系统定义了`CMAKE_SYSTEM_NAME`，因此不需要使用定制命令来查询此消息。

**CMake使用操作系统变量：**

```cmake
${CMAKE_SYSTEM_NAME}
```

**Linux上查询操作系统：**

```shell
uname -s
```





## 2.2处理与平台相关的源代码











# 3.交叉编译

**具体实施：**

1.创建一个文件夹，其中包括**hello-world.cpp**和**CMakeLists.txt**。

2.再创建一个`toolchain.cmake`文件，其内容为：

```cmake
# the name of the target operating system
set(CMAKE_SYSTEM_NAME Windows)
#设置交叉编译器根路径
set(CMAKE_FIND_ROOT_PATH /path/to/target/environment)
#指定编译器
set(CMAKE_CXX_COMPILER i686-w64-mingw32-g++)
# adjust the default behaviour of the find commands:
# search headers and libraries in the target environment
set(CMAKE_FIND_ROOT_PATH_MODE_INCLUDE ONLY)
set(CMAKE_FIND_ROOT_PATH_MODE_LIBRARY ONLY)
# search programs in the host environment
set(CMAKE_FIND_ROOT_PATH_MODE_PROGRAM NEVER)
```

3.将`CMAKE_CXX_COMPILER`设置为对应的编译器(路径)。

4.然后，通过将`CMAKE_TOOLCHAIN_FILE`指向工具链文件，从而配置代码。

```cmake
$ mkdir -p build
$ cd build
$ cmake -D CMAKE_TOOLCHAIN_FILE=toolchain.cmake ..
-- The CXX compiler identification is GNU 5.4.0
-- Check for working CXX compiler: /home/user/mxe/usr/bin/i686-w64-mingw32.static-g++
-- Check for working CXX compiler: /home/user/mxe/usr/bin/i686-w64-mingw32.static-g++ -- works
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done
-- Generating done
-- Build files have been written to: /home/user/cmake-recipes/chapter-13/recipe-01/cxx-example/build
```

5.构建可执行文件：

```cmake
$ cmake --build .
```

