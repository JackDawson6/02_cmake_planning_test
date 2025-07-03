cmake_minimum_required(VERSION 3.29.0)
project(planning)
set(CMAKE_CXX_STANDARD 17)
 
# 动态库 
# CMAKE_RUNTIME_OUTPUT_DIRECTORY  只适用windows平台
# CCMAKE_LIBRARY_OUTPUT_DIRECTORY   只适用linux平台
# 静态库
# CMAKE_ARCHIVE_OUTPUT_DIRECTORY  静态库不会管理exe文件
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${PROJECT_SOURCE_DIR}/bin)   # 把动态库和exe文件都指定到bin目录下面

aux_source_directory(${PROJECT_SOURCE_DIR}/src SRC_LISTS)
add_library(common      # 将src目录下的所有.c文件，创建成一个动态库
SHARED
${SRC_LISTS}
)

# common动态库依赖include目录下的.h文件，这里需要包含一下
target_include_directories(common 
PUBLIC 
${PROJECT_SOURCE_DIR}/include
)

# set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)
add_executable(planning_main planning_main.cpp)

# 这里前边生成是可执行文件planning_main，也是依赖include下的.h文件，这里需要包含一下
target_include_directories(planning_main 
PUBLIC 
${PROJECT_SOURCE_DIR}/include
)

# 这里将生成的可执行文件planning_main链接到common动态库上     由于我们生成了动态库，可执行文件加载的时候需要用到这个动态库，所以个这个动态库要和可执行文件放在同级目录下
target_link_libraries(planning_main
PUBLIC
common 
)