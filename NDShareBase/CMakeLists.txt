cmake_minimum_required(VERSION 2.8)
PROJECT(NDShareBase)


set(HeadFileDirList "")  
#�ݹ��������Ŀ��ָ���ļ����µ�����.h�ļ����ļ�·��;  
file(GLOB SRCH_LIST_1 "common/*.h")
file(GLOB SRCH_LIST_2 "commonImpl/*.h")
file(GLOB SRCH_LIST_3 "linux/*.h")
set(SRCH_LIST ${SRCH_LIST_1} ${SRCH_LIST_2} ${SRCH_LIST_3}) 
foreach(HeadFile ${SRCH_LIST})  
    #ȥ���ļ���,��ȡ·��;
    string(REGEX REPLACE "(.*)/.*\\.h$" "\\1" HeadFileDir ${HeadFile})
    #����·��;
    include_directories(${HeadFileDir})
    #ƴ��·��,Ȼ������CMAKE_INCLUDE_PATH����ʹ��find_path��������Ŀ¼;
    set(HeadFileDirList "${HeadFileDirList}${HeadFileDir} ")
endforeach()

include_directories(${CMAKE_CURRENT_SOURCE_DIR}/commonImpl)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/common/mysql)

#����cpp�ļ�;
SET(SRCS ${CMAKE_CURRENT_SOURCE_DIR}/linux/NDShareBaseLinuxAll.cpp)


MESSAGE(STATUS "this is BINARY dir: " ${NDShareBase_BINARY_DIR})
MESSAGE(STATUS "this is SOURCE dir: " ${NDShareBase_SOURCE_DIR})
MESSAGE(STATUS "this is CURRENT dir: " ${CMAKE_CURRENT_SOURCE_DIR})


IF(UNIX)
ADD_DEFINITIONS(-DLINUX -g -O0)
SET(CMAKE_CXX_COMPILER g++)
SET(CMAKE_CXX_FLAGS -Wall)
SET(LIBRARY_OUTPUT_PATH ${CMAKE_CURRENT_SOURCE_DIR}/../DebugBin/lib)

LINK_DIRECTORIES(${CMAKE_CURRENT_SOURCE_DIR}/dep/lib)

ADD_LIBRARY(NDShareBase STATIC ${SRCS})

TARGET_LINK_LIBRARIES(NDShareBase GMemPool mysqlclient)

ENDIF(UNIX)

