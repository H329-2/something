---
日期: 
tags:
  - cmake
  - cpp
  - 编程语言
---

## 概念

### 1.背景知识

cmake是kitware公司以及一些开源工作者在开发工具套件的过程中的衍生品

**cmake具有一些主要特点：**
1. 开源代码，使用类BSD许可发布。[http://cmake.org/HTML/Copyright.html](http://cmake.org/HTML/Copyright.html)
2. 跨平台，并可以生成native编译配置文件，在linux/Unix平台，生成makefile文件，在苹果平台生成xcode，在windows平台生成MSVC的工程文件
3. 能够管理大型项目，KDE4 就是最好的证明。
4. 简化编译构建过程和编译过程。Cmake 的工具链非常简单：cmake+make。
5. 高效虑，按照 KDE 官方说法，CMake 构建 KDE4 的 kdelibs 要比使用 autotools 来构建 KDE3.5.6 的 kdelibs 快 40%，主要是因为 Cmake 在工具链中没有 libtool。
6. 可扩展，可以为 cmake 编写特定功能的模块，扩充 cmake 功能。

## 构建一个helloworld
### 1.准备工作

cmake需要有一个文件夹作为工作空间

```shell
mkdir -p cmake_test/test1
cd cmake_test/test1
```

在文件下创建`main.cpp`和`CMakeLists.txt`

在`main.cpp`中写入

```c++
#include<iostream.h>
int main(){
	std::cout<<"hello world!"<<std::endl;
	return 0;
}
```

`CMakeLists.txt`中写入

```cmake
PROJECT(HELLO)
SET(SRC_LIST main.c)
MESSAGE(STATUS "This is BINARY dir " ${HELLO_BINARY_DIR})
MESSAGE(STATUS "This is SOURCE dir " ${HELLO_SOURCE_DIR})
ADD_EXECUTABLE(hello ${SRC_LIST})
```

### 2.开始构建

```shell
cmake .
make

#运行
./hello
```

### 3.参数

`PROJECT(projectname [CXX] [C] [Java])`：定义工程名称以及工程支持的语言，定义此指令后就定义了两个变量：`<projectname>_BINARY_DIR` 以及 `<projectname>_SOURCE_DIR`

当内部编译时，这两个变量会指向当前文件夹`test1`

`SET(VAR [VALUE] [CACHE TYPE DOCSTRING [FORCE]])`:定义各种变量

`MESSAGE([SEND_ERROR | STATUS | FATAL_ERROR] "message to display" ...)`：可以输出3种变量：
1. `SEND_ERROR`，产生错误，生成过程被跳过
2. `STATUS`，输出前缀为`--`的信息
3. `FATAL_ERROR`，立即终止所有cmake过程

`ADD_EXECUTABLE(hello ${SRC_LIST})`：将源文件添加到生成的程序中

`SET(EXECUTABLE_OUTPUT_PATH ${PROJECT_BINARY_DIR}/bin)`：设置二进制文件保存路径

`SET(LIBRARY_OUTPUT_PATH ${PROJECT_BINARY_DIR}/lib)`：设置库保存路径

`ADD_SUBDIRECTORY(src bin)`：添加子目录

```cmake
INSTALL(TARGETS myrun mylib mystaticlib
        RUNTIME DESTINATION bin
        LIBRARY DESTINATION lib
        ARCHIVE DESTINATION libstatic
)
```
将可执行文件以及库文件安装到指定路径中

```cmake
INSTALL(FILES files... DESTINATION <dir>
 [PERMISSIONS permissions...]
 [CONFIGURATIONS [Debug|Release|...]]
 [COMPONENT <component>]
 [RENAME <name>] [OPTIONAL]
)
```
将普通文件安装到指定路径中

```cmake
INSTALL(PROGRAMS files... DESTINATION <dir>
 [PERMISSIONS permissions...]
 [CONFIGURATIONS [Debug|Release|...]]
 [COMPONENT <component>]
 [RENAME <name>] [OPTIONAL]
)
```
将非目标的可执行文件安装到指定路径中

```cmake
INSTALL(DIRECTORY dirs... DESTINATION <dir>
 [FILE_PERMISSIONS permissions...]
 [DIRECTORY_PERMISSIONS permissions...]
 [USE_SOURCE_PERMISSIONS]
 [CONFIGURATIONS [Debug|Release|...]]
 [COMPONENT <component>]
 [[PATTERN <pattern> | REGEX <regex>]
 [EXCLUDE] [PERMISSIONS permissions...]] [...]
)
```
安装目录

```cmake
ADD_LIBRARY(libname [SHARED|STATIC|MODULE]
 [EXCLUDE_FROM_ALL]
 source1 source2 ... sourceN
)
```
构建库文件

`INCLUDE_DIRECTORIES([AFTER|BEFORE] [SYSTEM] dir1 dir2 ...)`：向工程添加多个特定的头文件

`LINK_DIRECTORIES(directory1 directory2 ...)`：添加非标准的共享库搜索路径

`TARGET_LINK_LIBRARIES(target library1<debug | optimized> library2 ...)`：为目标添加若干个共享库

### 4.语法规则

1. 变量使用`${}`方式取值，但是在 IF 控制语句中是直接使用变量名。
	
2. 指令(参数1 参数2...)
    
    参数使用括弧括起，参数之间使用空格或分号分开。
    
    以上面的 `ADD_EXECUTABLE` 指令为例，如果存在另外一个 func.c 源文件，就要写成：
    
    ADD_EXECUTABLE(hello main.c func.c)或者
    
    ADD_EXECUTABLE(hello main.c;func.c)
    
3. 指令是大小写无关的，参数和变量是大小写相关的。

[[从cmake开始的c++]]