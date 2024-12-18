# qt 基本应用

## 一个基本的 qt 应用应该有什么功能

## 本部项目代码路径

### 地址

### 说明

本部分代码的 `c++ namespace` 与 项目名称均为 `janna`

## 多目录构建

### commit id



### 目录结构

复制一份 [qt-hello](https://github.com/ghost-hello-project/xui-note/tree/main/code/qt/qt-hello)

并修改目录为如下结构

```
laolang@laolang-mint:qt-base$ tree -a
.
├── build
├── .clang-format
├── .clang-tidy
├── CMakeLists.txt
├── CMakePresets.json
├── .gitignore
├── include
│   └── janna
│       └── app
│           └── mainwindow.h
├── Makefile
├── src
│   ├── app
│   │   ├── CMakeLists.txt
│   │   ├── main.cpp
│   │   └── mainwindow.cpp
│   └── CMakeLists.txt
└── .vscode
    ├── launch.json
    ├── settings.json
    └── tasks.json

8 directories, 14 files
laolang@laolang-mint:qt-base$ 
```

### 顶级 CMakeLists.txt

此部分并无特殊配置,只是添加了一些注释

> 关于 `dist` 目录的说明
> 
> dist 目录为构建后所有发布文件所在的目录,意指: 构建后此目录可直接打包发布为绿色版软件
> 
> dist/bin : 可执行程序所在目录
> 
> 其他目录可见后续操作

```cmake
cmake_minimum_required(VERSION 3.16)

# 语言环境配置 ###########################################################################################################
set(CMAKE_AUTOUIC ON)
set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTORCC ON)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# 项目配置 ##############################################################################################################
project(janna VERSION 0.1 LANGUAGES CXX)

# 编译发布目录
set(dist_dir ${CMAKE_BINARY_DIR}/dist)

# 二进制文件目录
set(bin_dir ${dist_dir}/bin)

# qt 库
find_package(QT NAMES Qt6 Qt5 REQUIRED COMPONENTS Widgets)
find_package(Qt${QT_VERSION_MAJOR} REQUIRED COMPONENTS Widgets)

# 编译相关配置 ###########################################################################################################

# 生成 compile_commands.json
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

# 包含全局头文件
include_directories(${PROJECT_SOURCE_DIR}/include)

# 添加子目录 #############################################################################################################
add_subdirectory(src)
```

### src/app/CMakeLists.txt

此部分只是将之前的代码复制了一份,删除了不需要的代码,例如 mac 、安装等

```cmake
set(APP_SOURCES
    ${CMAKE_CURRENT_SOURCE_DIR}/main.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/mainwindow.cpp
    ${PROJECT_SOURCE_DIR}/include/janna/app/mainwindow.h
)

qt_add_executable(${PROJECT_NAME}
    MANUAL_FINALIZATION
    ${APP_SOURCES}
)

target_link_libraries(${PROJECT_NAME} PRIVATE
    Qt${QT_VERSION_MAJOR}::Widgets
)

set_target_properties(${PROJECT_NAME} PROPERTIES RUNTIME_OUTPUT_DIRECTORY ${bin_dir})

set_target_properties(${PROJECT_NAME} PROPERTIES
    ${BUNDLE_ID_OPTION}
    WIN32_EXECUTABLE TRUE
)

qt_finalize_executable(${PROJECT_NAME})

if(WIN32)
    # 构建后动作
    add_custom_command(TARGET ${PROJECT_NAME} POST_BUILD

        # 复制 qt 资源文件
        COMMAND ${CMAKE_COMMAND} -E echo "$<TARGET_FILE:${PROJECT_NAME}>"
        COMMAND ${WINDEPLOYQT6_EXEC_PATH} "$<TARGET_FILE:${PROJECT_NAME}>"
    )
endif()
```


## 最终效果
