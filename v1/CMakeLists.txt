#[[
这是一块
注释
set 的三种用法
#1 设置一个变量
#2 给一个固定的宏设定值，这里是cpp的standard
#3 指定可执行程序的输出的路径
set(HOME /home/xx/xx)
set(EXCUTABLE_OUTPUT_PATH ${HOME}/bin)
如果路径不存在会自动创建


若项目中的文件很多，我们通过file命令实现查找
file(GLOB/GLOB_RECURSE 变量名 要搜索的文件的路径和文件类型)
两种参数就是在指定目录递归与否的区别而已
file(GLOB MAIN_SRC ${CMAKE_CURRENT_SOURCE_DIR}/src/*.cpp)
file(GLOB HEAD_SRC ${CMAKE_CURRENT_SOURCE_DIR}/inc/*.cpp)
btw这个宏是存在的${CMAKE_CURRENT_SOURCE_DIR},
PROJECT_SOURCE_DIR代表CMakeLists.txt所在的目录
而CMAKE_CURRENT_SOURCE_DIR指向的是CMake目前正在处理的CMakeLists所在的目录
如果调用了project，那么两者相同，否则不同

rm * -rf删除文件夹下全部文件

aux_source_directory(${PROJECT_SOURCE_DIR} SRC) 把搜到的文件丢进SRC变量
使用aux搜索不需要指定文件类型

tree命令相对ls可以递归展示文件
tree -L 2只想看两层深度的目录
tree -d 忽略具体文件，只看文件夹结构
tree -a 显示隐藏文件
tree > structure.txt 导出到文件

复习一下重定向符>把输出存入文件，如果想在后面添加就用>>否则覆盖
这可以防止刷屏
>的终点是文件，而|的终点是另一个程序

变量和宏，如果只是找这个盒子不需要加${}除非需要对这个盒子的值进行操作
变量装着数据,而宏只是文本替换

总结一下，CMakeLists需要包含以下东西
cmake_minimum_required(VERSION 3.15)    cmake版本
project(test)   项目名字
file(GLOB SRC ${CMAKE_CURRENT_SOURCE_DIR}/src/*.cpp)    拿到源文件作为变量SRC
set(EXECUTABLE_OUTPUT_PAHT ${PROJECT_SOURCE_DIR}/bin)   设置输出路径的变量的值
include_directories(${PROJECT_SOURCE_DIR}/inc)  设置inc路径，使用这个命令而不用set修改
add_executable(${PROJECT_NAME} ${SRC})  生成二进制文件

通过cmake制作头文件：有些时候我们编译不是为了生成二进制文件，而是为了生成库文件，这分为动态库和静态库

学习到此时git的一些问题
git push -u origin CMake_note
因为之前云端没有和这个分支建立联系，虽然我们checkout到了这个分支
虽然在main分支push上去别的分支也一样会存在，但是其实你没有push任何的内容上去
当我们输入git push -u origin CMake_note
就可以在GitHub看到CMake_note had recent pushes 2 minutes ago
git的每次push都是在分支上，无法做到多分支提交
git pull == fetch + merge
pull会自动拉取分支的最新变动然后合并，而fetch并不会有合并的动作
如果让vsc的git模块管理一个没有任何commit的repo(这是工作区的第二个git仓库)这也许不会成功

如果无法操作一个工作区的多个文件夹的git仓库,这完全是因为vsc可视化的界面的问题,因为它在连接的时候混乱了
solve:ctrl + shift + p,然后输入reload window,这可以解决vsc可视化界面的很多问题

面对merge时候的冲突,你可以在编辑器里面一点点选择倾向的段落,也可以在git管理面板选择一次性全部倾向
git checkout --theirs src/mod   这样可以默认处理特定的模块,如果不输入位置就是默认全部冲突文件

生成库
动态库是有可执行权限的，而静态库没有
我们在发布的时候只需要发布库文件和inc文件
库文件和src文件的区别就是一个是二进制一个是源码，仅此而已

顺便复习一下新建云端新仓库的时候
# git remote add origin <url>
如果要替换url
git remote set-url origin <新的url>
-u是--set-upstream的缩写代表绑定     origin是一个别名(alias)代表远程仓库的url
当第一次执行git push -u origin main
之后只要执行push即可
带-的是配置(参数/选项),不带-的比如add和set-url是动作(子命令)
]]

set(CMAKE_CXX_STANDARD 17)
cmake_minimum_required(VERSION 3.28.3)
project(testv1)
# set(SRC_LIST add.cpp sub.cpp main.cpp) # 设置源文件变量SRC_LIST默认是当前文件夹

# 我在这里拼错了变量名，指定输出位置
# set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin) #设置bin路径,根据你自己的路径修改
#指定头文件的路径
include_directories(${PROJECT_SOURCE_DIR}/inc) # 他的影响是目录级别的
# target_include_directories(app PUBLIC .) # 他的影响是应用级别的,PUBLIC等决定它的衍生文件会被影响到什么程度
# 找源文件的位置设置变量
# file(GLOB SRC ${PROJECT_SOURCE_DIR}/src/*.cpp)
# list(REMOVE_ITEM SRC "${PROJECT_SOURCE_DIR}/src/sample.cpp")
# add_executable(app ${SRC})

# 可执行文件生成
# add_executable(${PROJECT_NAME} ${SRC})

# 2. 给这个单文件单独建一个可执行文件，方便你以后调试它
# add_executable(sample_test "${PROJECT_SOURCE_DIR}/src/sample.cpp")

#默认路径是${PROJECT_SOURCE_DIR}
# set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib)
# add_library(${PROJECT_NAME} STATIC ${SRC})
# add_library(${PROJECT_NAME} SHARED ${SRC})
#对于同一个目标，其实就是同一个名字，一次只能生成一个库文件

#现在我们已经生成了库文件，可以把add_library删掉了
# 开始连接库文件
file(GLOB SRC ${PROJECT_SOURCE_DIR}/main.cpp)
link_libraries(${PROJECT_NAME})# 不需要写全名字
link_directories(${PROJECT_SOURCE_DIR}/lib_a)

add_executable(${PROJECT_NAME} ${SRC})