cmake_minimum_required(VERSION 3.7)
project(Demo CXX)
set(CMAKE_CXX_STANDARD 11)
SET(CMAKE_CXX_STANDARD_REQUIRED ON)
SET(CMAKE_POSITION_INDEPENDENT_CODE ON)
SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 -Wall -g -O2 ")
# 解决各种找不到xxx.make文件
set(CMAKE_PREFIX_PATH "E:/go/vcpkg/installed/x64-windows" ${CMAKE_PREFIX_PATH})

# 指定头文件
include_directories(${PROJECT_SOURCE_DIR}/client/include)
# 链接静态库link_xxx()
link_directories(${PROJECT_SOURCE_DIR}/client/lib)  # 指定库文件路径 空格分隔
link_libraries(iotdb_session.lib
        parse.lib
        thriftmd.lib
        tutorialgencpp.lib)  # 或libstatic1.a 指定库文件 空格分隔


#find_package(jsoncpp REQUIRED)
#find_package(PThreads4W REQUIRED)
find_package(OpenSSL REQUIRED)
find_package(Thrift REQUIRED)

# 源文件就是main.cpp，其他源程序使用静态库，静态库会被打包到可执行程序
add_executable(app SessionExample.cpp
        write_plugin.c)

#target_link_libraries(app JsonCpp::JsonCpp)
#target_link_libraries(app PThreads4W::PThreads4W)
target_link_libraries(app OpenSSL::SSL)
target_link_libraries(app OpenSSL::Crypto)
target_link_libraries(app thrift::thrift thriftz::thriftz)