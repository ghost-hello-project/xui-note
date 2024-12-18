# qt Hello World

## 新建一个 qt 项目
**1. 选择 widgets application**

![](/xui-note/assets/images/qt/a03_hello/001.png)


**2. 选择路径**

![](/xui-note/assets/images/qt/a03_hello/002.png)

**3. 构建系统选择 CMake**

![](/xui-note/assets/images/qt/a03_hello/003.png)

**4. 不使用 ui 文件**

![](/xui-note/assets/images/qt/a03_hello/004.png)

**5. 其他保持默认**


## 脱离 qt creator

### 查看 qt 的 cmake 构建过程

在新建项目第一次构建时,可以打开 `概要信息` 查看 cmake 的构建输出. 如果此窗口清空了, 可以删除构建目录以及 `CMakeLists.txt.user` 文件,然后导入项目后点击 `Configure Project` 即可看到

![](/xui-note/assets/images/qt/a03_hello/005.png)

简单排版一下

```linenums="1" hl_lines="1-10"
[cmake] Running /home/laolang/program/cmake/bin/cmake -S /home/laolang/code/qt/qt-hello 
-B /home/laolang/code/qt/qt-hello/build/Desktop_Qt_6_7_2-Debug 
-DCMAKE_CXX_FLAGS_INIT:STRING=-DQT_QML_DEBUG 
-DCMAKE_PROJECT_INCLUDE_BEFORE:FILEPATH=/home/laolang/code/qt/qt-hello/build/Desktop_Qt_6_7_2-Debug/.qtc/package-manager/auto-setup.cmake 
-DQT_QMAKE_EXECUTABLE:FILEPATH=/home/laolang/Qt/6.7.2/gcc_64/bin/qmake 
-DCMAKE_C_COMPILER:FILEPATH=/usr/bin/clang 
'-DCMAKE_GENERATOR:STRING=Unix Makefiles' 
-DCMAKE_BUILD_TYPE:STRING=Debug 
-DCMAKE_CXX_COMPILER:FILEPATH=/usr/bin/clang++ 
-DCMAKE_PREFIX_PATH:PATH=/home/laolang/Qt/6.7.2/gcc_64 in /home/laolang/code/qt/qt-hello/build/Desktop_Qt_6_7_2-Debug.
[cmake] -- The CXX compiler identification is Clang 18.1.3
[cmake] -- Detecting CXX compiler ABI info
[cmake] -- Detecting CXX compiler ABI info - done
[cmake] -- Check for working CXX compiler: /usr/bin/clang++ - skipped
[cmake] -- Detecting CXX compile features
[cmake] -- Detecting CXX compile features - done
[cmake] -- Performing Test CMAKE_HAVE_LIBC_PTHREAD
[cmake] -- Performing Test CMAKE_HAVE_LIBC_PTHREAD - Success
[cmake] -- Found Threads: TRUE
[cmake] -- Performing Test HAVE_STDATOMIC
[cmake] -- Performing Test HAVE_STDATOMIC - Success
[cmake] -- Found WrapAtomic: TRUE
[cmake] -- Found OpenGL: /usr/lib/x86_64-linux-gnu/libOpenGL.so
[cmake] -- Found WrapOpenGL: TRUE
[cmake] -- Found XKB: /usr/lib/x86_64-linux-gnu/libxkbcommon.so (found suitable version "1.6.0", minimum required is "0.5.0")
[cmake] -- Could NOT find WrapVulkanHeaders (missing: Vulkan_INCLUDE_DIR) 
[cmake] -- Configuring done (0.6s)
[cmake] -- Generating done (0.0s)
[cmake] -- Build files have been written to: /home/laolang/code/qt/qt-hello/build/Desktop_Qt_6_7_2-Debug
[cmake] 
[cmake] Elapsed time: 00:01.
```

在上述排版后的概要信息中,可以看到主要做了如下几件事

1. 指定了源码目录
2. 指定了编译器路径
3. 指定了 `CMAKE_PREFIX_PATH`
4. 指定了构建模式

### 命令行编译

将源码文件复制到一个新的目录

```
laolang@laolang-mint:qt-hello$ tree
.
├── CMakeLists.txt
├── main.cpp
├── mainwindow.cpp
└── mainwindow.h

1 directory, 4 files
laolang@laolang-mint:qt-hello$ 
```

然后直接命令行编译

```
cmake -S . \
-B build \
-DCMAKE_GENERATOR='Unix Makefiles' \
-DCMAKE_CXX_COMPILER=/usr/bin/g++ \
-DCMAKE_BUILD_TYPE=Debug \
-DCMAKE_PREFIX_PATH=/home/laolang/Qt/6.7.2/gcc_64 \
&& cmake --build build
```

```
laolang@laolang-mint:qt-hello$ cmake -S . \
-B build \
-DCMAKE_GENERATOR='Unix Makefiles' \
-DCMAKE_CXX_COMPILER=/usr/bin/g++ \
-DCMAKE_BUILD_TYPE=Debug \
-DCMAKE_PREFIX_PATH=/home/laolang/Qt/6.7.2/gcc_64 \
&& cmake --build build
-- The CXX compiler identification is GNU 13.2.0
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Check for working CXX compiler: /usr/bin/g++ - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Performing Test CMAKE_HAVE_LIBC_PTHREAD
-- Performing Test CMAKE_HAVE_LIBC_PTHREAD - Success
-- Found Threads: TRUE
-- Performing Test HAVE_STDATOMIC
-- Performing Test HAVE_STDATOMIC - Success
-- Found WrapAtomic: TRUE
-- Found OpenGL: /usr/lib/x86_64-linux-gnu/libOpenGL.so
-- Found WrapOpenGL: TRUE
-- Found XKB: /usr/lib/x86_64-linux-gnu/libxkbcommon.so (found suitable version "1.6.0", minimum required is "0.5.0")
-- Could NOT find WrapVulkanHeaders (missing: Vulkan_INCLUDE_DIR) 
-- Configuring done (0.4s)
-- Generating done (0.0s)
-- Build files have been written to: /home/laolang/github/ghost-hello-project/xui-note/code/qt/qt-hello/build
[  0%] Built target qt-hello_autogen_timestamp_deps
[ 16%] Automatic MOC and UIC for target qt-hello
[ 16%] Built target qt-hello_autogen
[ 33%] Building CXX object CMakeFiles/qt-hello.dir/qt-hello_autogen/mocs_compilation.cpp.o
[ 50%] Building CXX object CMakeFiles/qt-hello.dir/main.cpp.o
[ 66%] Building CXX object CMakeFiles/qt-hello.dir/mainwindow.cpp.o
[ 83%] Linking CXX executable qt-hello
[100%] Built target qt-hello
laolang@laolang-mint:qt-hello$ 
```

### CMakePresets.json
然后将上述命令行编译的命令行选项加到 `CMaekLists.json`,此时目录结构如下
```
laolang@laolang-mint:qt-hello$ tree
.
├── CMakeLists.txt
├── CMakePresets.json
├── main.cpp
├── mainwindow.cpp
└── mainwindow.h

1 directory, 5 files
laolang@laolang-mint:qt-hello$ 
```

其中 `CMakePresets.json` 内容
```json
{
    "version": 6,
    "cmakeMinimumRequired": {
        "major": 3,
        "minor": 26,
        "patch": 0
    },
    "configurePresets": [
        {
            "name": "linux-base",
            "displayName": "linux base",
            "description": "linux 通用设置",
            "generator": "Ninja",
            "cacheVariables": {
                "CMAKE_PREFIX_PATH": "/home/laolang/Qt/6.7.2/gcc_64",
                "CMAKE_CXX_COMPILER": "/usr/bin/g++",
                "CMAKE_CXX_FLAGS": "-Wall -Wextra"
            }
        },
        {
            "name": "linux-release",
            "displayName": "linux release",
            "description": "linux 平台下使用 gcc 构建 release 版本",
            "inherits": "linux-base",
            "binaryDir": "${sourceDir}/build/linux-release",
            "cacheVariables": {
                "CMAKE_BUILD_TYPE": "Release"
            }
        },
        {
            "name": "linux-debug",
            "displayName": "linux debug",
            "description": "linux 平台下使用 gcc 构建 debug 版本",
            "inherits": "linux-base",
            "binaryDir": "${sourceDir}/build/linux-debug",
            "cacheVariables": {
                "CMAKE_BUILD_TYPE": "Debug"
            }
        }
    ],
    "buildPresets": [
        {
            "name": "linux-release",
            "configurePreset": "linux-release"
        },
        {
            "name": "linux-debug",
            "configurePreset": "linux-debug"
        }
    ]
}
```

一条命令直接编译
```
cmake --preset=linux-release && cmake --build --preset=linux-release
```

```
laolang@laolang-mint:qt-hello$ cmake --preset=linux-release && cmake --build --preset=linux-release
Preset CMake variables:

  CMAKE_BUILD_TYPE="Release"
  CMAKE_CXX_COMPILER="/usr/bin/g++"
  CMAKE_CXX_FLAGS="-Wall -Wextra"
  CMAKE_PREFIX_PATH="/home/laolang/Qt/6.7.2/gcc_64"

-- The CXX compiler identification is GNU 13.2.0
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Check for working CXX compiler: /usr/bin/g++ - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Performing Test CMAKE_HAVE_LIBC_PTHREAD
-- Performing Test CMAKE_HAVE_LIBC_PTHREAD - Success
-- Found Threads: TRUE
-- Performing Test HAVE_STDATOMIC
-- Performing Test HAVE_STDATOMIC - Success
-- Found WrapAtomic: TRUE
-- Found OpenGL: /usr/lib/x86_64-linux-gnu/libOpenGL.so
-- Found WrapOpenGL: TRUE
-- Found XKB: /usr/lib/x86_64-linux-gnu/libxkbcommon.so (found suitable version "1.6.0", minimum required is "0.5.0")
-- Could NOT find WrapVulkanHeaders (missing: Vulkan_INCLUDE_DIR) 
-- Configuring done (0.4s)
-- Generating done (0.0s)
-- Build files have been written to: /home/laolang/github/ghost-hello-project/xui-note/code/qt/qt-hello/build/linux-release
[5/5] Linking CXX executable qt-hello
laolang@laolang-mint:qt-hello$ 
```

## 一些基本的代码