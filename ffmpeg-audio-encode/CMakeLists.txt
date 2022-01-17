
cmake_minimum_required(VERSION 3.4.1)


file(GLOB native_files "src/main/cpp/*.cpp")

add_library(native-encode
           SHARED
           ${native_files})

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=gnu++11")
set(CMAKE_VERBOSE_MAKEFILE on)

#获取上级目录
get_filename_component(PARENT_DIR ${CMAKE_SOURCE_DIR} PATH)


#######################
#使用单个库版本
set(LIBFFMPEG_DIR ${PARENT_DIR}/ffmpeg-single)
#对于find_package找不到的外部依赖库，可以用add_library添加
# SHARED表示添加的是动态库
# IMPORTED表示是引入已经存在的动态库

add_library(avcodec SHARED IMPORTED)
#指定所添加依赖库的导入路径
set_target_properties(avcodec PROPERTIES IMPORTED_LOCATION ${LIBFFMPEG_DIR}/libs/${ANDROID_ABI}/lib/libavcodec.so)

#add_library(avdevice SHARED IMPORTED)
#set_target_properties(avdevice PROPERTIES IMPORTED_LOCATION ${LIBFFMPEG_DIR}/libs/${ANDROID_ABI}/lib/libavdevice.so)

#add_library(avfilter SHARED IMPORTED)
#set_target_properties(avfilter PROPERTIES IMPORTED_LOCATION ${LIBFFMPEG_DIR}/libs/${ANDROID_ABI}/lib/libavfilter.so)

add_library(avformat SHARED IMPORTED)
set_target_properties(avformat PROPERTIES IMPORTED_LOCATION ${LIBFFMPEG_DIR}/libs/${ANDROID_ABI}/lib/libavformat.so)

add_library(avutil SHARED IMPORTED)
set_target_properties(avutil PROPERTIES IMPORTED_LOCATION ${LIBFFMPEG_DIR}/libs/${ANDROID_ABI}/lib/libavutil.so)

add_library(swresample SHARED IMPORTED)
set_target_properties(swresample PROPERTIES IMPORTED_LOCATION ${LIBFFMPEG_DIR}/libs/${ANDROID_ABI}/lib/libswresample.so)

add_library(swscale SHARED IMPORTED)
set_target_properties(swscale PROPERTIES IMPORTED_LOCATION ${LIBFFMPEG_DIR}/libs/${ANDROID_ABI}/lib/libswscale.so)

#add_library(postproc SHARED IMPORTED)
#set_target_properties(postproc PROPERTIES IMPORTED_LOCATION ${LIBFFMPEG_DIR}/libs/${ANDROID_ABI}/lib/libpostproc.so)

#######################



# libffmpeg库头文件
include_directories(${LIBFFMPEG_DIR}/libs/${ANDROID_ABI}/include)

target_link_libraries(native-encode
                   avcodec avformat avutil swresample swscale #postproc
                   android
                   log)
