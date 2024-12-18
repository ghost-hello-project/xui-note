# qt Hello World

## 本部分项目代码路径

[https://github.com/ghost-hello-project/xui-note/tree/main/code/qt/qt-hello](https://github.com/ghost-hello-project/xui-note/tree/main/code/qt/qt-hello)


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

#### linux 平台
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

#### windows 平台

`windows` 平台下修改 `CMakePresets.json`,主要是添加 `windows` 平台相关配置
```json linenums="1" hl_lines="9-41 75-82"
{
    "version": 6,
    "cmakeMinimumRequired": {
        "major": 3,
        "minor": 26,
        "patch": 0
    },
    "configurePresets": [
        {
            "name": "windows-base",
            "displayName": "windows base",
            "description": "windows 通用设置",
            "generator": "MinGW Makefiles",
            "cacheVariables": {
                "CMAKE_PREFIX_PATH": "D:/program/qt/6.5.3/mingw_64",
                "CMAKE_CXX_COMPILER": "D:/program/qt/Tools/mingw1120_64/bin/g++.exe",
                "CMAKE_CXX_FLAGS": "-Wall -Wextra",
                "WINDEPLOYQT6_EXEC_PATH": "D:/program/qt/6.5.3/mingw_64/bin/windeployqt6.exe"
            }
        },
        {
            "name": "windows-release",
            "displayName": "windows release",
            "description": "windows 平台下使用 mingw 构建 release 版本",
            "inherits": "windows-base",
            "binaryDir": "${sourceDir}/build/windows-release",
            "cacheVariables": {
                "CMAKE_BUILD_TYPE": "Release"
            }
        },
        {
            "name": "windows-debug",
            "displayName": "windows debug",
            "description": "windows 平台下使用 mingw 构建 debug 版本",
            "inherits": "windows-base",
            "binaryDir": "${sourceDir}/build/windows-debug",
            "cacheVariables": {
                "CMAKE_BUILD_TYPE": "Debug",
                "CMAKE_DEBUG_POSTFIX": "d"
            }
        },
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
            "name": "windows-release",
            "configurePreset": "windows-release"
        },
        {
            "name": "windows-debug",
            "configurePreset": "windows-debug"
        },
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

`CMakeLists.txt` 也需要修改. 由于 `windows` 平台编译后,如果不将 `qt` 相关 `dll` 路径添加到环境变量,则可执行文件无法直接运行,所以使用 `windeployqt6` 直接打包
```cmake linenums="1" hl_lines="61-70"
cmake_minimum_required(VERSION 3.26)

project(qt-hello VERSION 0.1 LANGUAGES CXX)

set(CMAKE_AUTOUIC ON)
set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTORCC ON)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

find_package(QT NAMES Qt6 Qt5 REQUIRED COMPONENTS Widgets)
find_package(Qt${QT_VERSION_MAJOR} REQUIRED COMPONENTS Widgets)

set(PROJECT_SOURCES
        main.cpp
        mainwindow.cpp
        mainwindow.h
)

if(${QT_VERSION_MAJOR} GREATER_EQUAL 6)
    qt_add_executable(qt-hello
        MANUAL_FINALIZATION
        ${PROJECT_SOURCES}
    )
# Define target properties for Android with Qt 6 as:
#    set_property(TARGET qt-hello APPEND PROPERTY QT_ANDROID_PACKAGE_SOURCE_DIR
#                 ${CMAKE_CURRENT_SOURCE_DIR}/android)
# For more information, see https://doc.qt.io/qt-6/qt-add-executable.html#target-creation
else()
    if(ANDROID)
        add_library(qt-hello SHARED
            ${PROJECT_SOURCES}
        )
# Define properties for Android with Qt 5 after find_package() calls as:
#    set(ANDROID_PACKAGE_SOURCE_DIR "${CMAKE_CURRENT_SOURCE_DIR}/android")
    else()
        add_executable(qt-hello
            ${PROJECT_SOURCES}
        )
    endif()
endif()

target_link_libraries(qt-hello PRIVATE Qt${QT_VERSION_MAJOR}::Widgets)

# Qt for iOS sets MACOSX_BUNDLE_GUI_IDENTIFIER automatically since Qt 6.1.
# If you are developing for iOS or macOS you should consider setting an
# explicit, fixed bundle identifier manually though.
if(${QT_VERSION} VERSION_LESS 6.1.0)
  set(BUNDLE_ID_OPTION MACOSX_BUNDLE_GUI_IDENTIFIER com.example.qt-hello)
endif()
set_target_properties(qt-hello PROPERTIES
    ${BUNDLE_ID_OPTION}
    MACOSX_BUNDLE_BUNDLE_VERSION ${PROJECT_VERSION}
    MACOSX_BUNDLE_SHORT_VERSION_STRING ${PROJECT_VERSION_MAJOR}.${PROJECT_VERSION_MINOR}
    MACOSX_BUNDLE TRUE
    WIN32_EXECUTABLE TRUE
)


if(WIN32)
    # 构建后动作
    add_custom_command(TARGET ${PROJECT_NAME} POST_BUILD

        # 复制 qt 资源文件
        # COMMAND ${CMAKE_COMMAND} -E copy ${PROJECT_SOURCE_DIR}/resources_build/win32/qt/Qt6Core.dll $<TARGET_FILE_DIR:${PROJECT_NAME}>/
        COMMAND ${CMAKE_COMMAND} -E echo "$<TARGET_FILE:${PROJECT_NAME}>"
        COMMAND ${WINDEPLOYQT6_EXEC_PATH} "$<TARGET_FILE:${PROJECT_NAME}>"
    )
endif()

include(GNUInstallDirs)
install(TARGETS qt-hello
    BUNDLE DESTINATION .
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
)

if(QT_VERSION_MAJOR EQUAL 6)
    qt_finalize_executable(qt-hello)
endif()
```
