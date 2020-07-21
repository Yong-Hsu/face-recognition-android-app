cmake_minimum_required(VERSION 3.4.1)

#include头文件目录
include_directories(src/main/cpp/include
                    src/main/cpp/)

#source directory源文件目录
file(GLOB FACE_SRC src/main/cpp/*.h
                    src/main/cpp/*.cpp)
set(FACE_COMPILE_CODE ${FACE_SRC})


#添加ncnn库
add_library(libncnn STATIC IMPORTED )
set_target_properties(libncnn
  PROPERTIES IMPORTED_LOCATION
  ${CMAKE_SOURCE_DIR}/src/main/jniLibs/${ANDROID_ABI}/libncnn.a)

#编译为动态库
add_library(Face SHARED ${FACE_COMPILE_CODE})

#添加工程所依赖的库
find_library(  log-lib log )
find_library( # Sets the name of the path variable.
        android-lib

        # Specifies the name of the NDK library that
        # you want CMake to locate.
        android )

# 解决undefined reference 的地方，加上了上面这个find_lib的块，

target_link_libraries( Face
                       libncnn
                       jnigraphics
                       z
                       ${log-lib}
                       ${android-lib} )
