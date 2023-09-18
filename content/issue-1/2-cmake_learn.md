### CMake
- CMake是一个跨平台的项目构建工具。

### 设置工程名
```cmake
project("demo")
```
### 定义变量
```cmake
set(SRC_LIST ${SOURCE})
```

### 搜索文件
```cmake
aux_source_directory(${PROJECT_SOURCE_DIR}/src/ SOURCE_FILES) #1
file(GLOB CLAC_LIST ${CMAKE_CURRENT_SOURCE_DIR}/calc/*.cpp) #1
```

### 编译、链接静态库
```cmake
add_library(calc STATIC ${CLAC_LIST})
link_directories(${PROJECT_SOURCE_DIR}/lib)
link_libraries(calc)
```

### 编译、链接动态库
```cmake
set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib)
add_library(calc SHARED ${CLAC_LIST})
target_link_libraries(${TARGET_NAME} calc)
```

### 预定义宏

|       宏       |   描述                 |
| :------------- | :----------------------: |
| PROJECT\_SOURCE\_DIR  |  使用cmake命令后紧跟的目录，一般是工程的根目录 |
| PROJECT\_BINARY\_DIR  |  执行cmake命令的目录                      |
| CMAKE\_CURRENT\_SOURCE\_DIR |   当前处理的CMakeLists.txt所在的路径  |
| CMAKE\_CURRENT\_BINARY\_DIR |   target 编译目录                   |
| EXECUTABLE\_OUTPUT\_PATH   |   重新定义目标二进制可执行文件的位置     |
| LIBRARY\_OUTPUT\_PATH	     |   重新定义目标链接库文件的位置          |
| PROJECT\_NAME	           |   返回通过PROJECT定义的项目名称        |
| CMAKE\_BINARY\_DIR	       |   项目构建路径，假设在build目录进行的构建，那么得到build目录的路径 |


### 自定义宏
```cmake
# 自定义 DEBUG 宏
add_definitions(-DDEBUG)

#ifdef DEBUG
    printf("debug...\n");
#endif
```

### 资源链接

<https://subingwen.cn/cmake/CMake-primer/>
