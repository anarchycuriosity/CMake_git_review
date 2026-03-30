# include_directories()
#  target_link_directories()
# target_link_libraries()
add_definitions(-DDEBUG) # 通过宏的方式可以控制仓库中某些代码是可用还是不可用
cmake_minimum_required(VERSION 3.28.3)
project(DefineTest)
set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)
set(SRC ${PROJECT_SOURCE_DIR}/Dtest.cpp)
add_executable(${PROJECT_NAME} ${SRC})

# 我们可以把调试代码都这样注释起来
# 如果需要它输出就添加定义宏，否则不添加
# 就像一个开关