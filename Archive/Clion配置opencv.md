---
aliases: []
area: 环境配置
project: 
date: 2023-04-27 20:07
---
---
#### Description
1. 下载编译好的 opencv
    [GitHub - huihut/OpenCV-MinGW-Build: 👀 MinGW 32bit and 64bit version of OpenCV compiled on Windows. Including OpenCV 3.3.1, 3.4.1, 3.4.1-x64, 3.4.5, 3.4.6, 3.4.7, 3.4.8-x64, 3.4.9, 4.0.0-alpha-x64, 4.0.0-rc-x64, 4.0.1-x64, 4.1.0, 4.1.0-x64, 4.1.1-x64, 4.5.0-with-contrib, 4.5.2-x64](https://github.com/huihut/OpenCV-MinGW-Build)
2. CmakeLists
    ```cmake
    cmake_minimum_required(VERSION 3.16)
    project(opencv_test)
    
    # find_package(OpenCV REQUIRED)
    # set(CMAKE_PREFIX_PATH E:/CPP/OpenCV-MinGW-Build)
    
    set(OpenCV_DIR E:/CPP/OpenCV-MinGW-Build/x64/mingw/lib)
    find_package(OpenCV 4 REQUIRED)
    
    include_directories(${OpenCV_INCLUDE_DIRS})
    
    link_libraries(${OpenCV_LIBS})
    
    # final
    add_executable(opencv_test main.cpp)
    ```
3. 在 Clion 运行环境变量中添加 `bin` 目录
    ![[Pasted image 20230427201326.png]]
---
#### Source
