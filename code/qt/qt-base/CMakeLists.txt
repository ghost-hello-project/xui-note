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
