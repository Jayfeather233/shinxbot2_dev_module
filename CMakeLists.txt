cmake_minimum_required(VERSION 3.11)

project(example) # Your project name here


# libraries header files
add_definitions(-DUSE_OPENSSL)

add_definitions( -DMAGICKCORE_QUANTUM_DEPTH=16 )
add_definitions( -DMAGICKCORE_HDRI_ENABLE=1 )
find_package(ImageMagick COMPONENTS Magick++ MagickWand MagickCore REQUIRED)
include_directories(${ImageMagick_INCLUDE_DIRS})
find_package(fmt REQUIRED)
include_directories(${fmt_INCLUDE_DIRS})
include_directories("./src/shinxbot_api")
include_directories("./src/Yourfunc")


# source codes
aux_source_directory(./src/Yourfunc YF_SOURCES)
aux_source_directory(./src MAIN_SOURCES)


# some defs
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall")
set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} -Wall -g")
if (CMAKE_CXX_COMPILER_ID STREQUAL "Clang")
  add_link_options("-fuse-ld=lld")
  set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} -fstandalone-debug")
endif()


# add all into your function shared library
add_library(${PROJECT_NAME} SHARED ${YF_SOURCES} ${MAIN_SOURCES})


# link libraries
target_link_libraries(${PROJECT_NAME} PRIVATE ${CMAKE_BINARY_DIR}/../src/shinxbot_api/libutils.so)

target_link_libraries(${PROJECT_NAME} PUBLIC ${ImageMagick_LIBRARIES})

find_package(jsoncpp REQUIRED)
TARGET_LINK_LIBRARIES(${PROJECT_NAME} PUBLIC ${JSONCPP_LIBRARIES})
TARGET_LINK_LIBRARIES(${PROJECT_NAME} PUBLIC -lcurl)
TARGET_LINK_LIBRARIES(${PROJECT_NAME} PUBLIC -ljsoncpp)
target_link_libraries(${PROJECT_NAME} PUBLIC fmt::fmt)
