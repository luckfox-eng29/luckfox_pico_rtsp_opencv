cmake_minimum_required(VERSION 3.10)

set(SDK_PATH $ENV{LUCKFOX_SDK_PATH})
if(NOT SDK_PATH)
    message(WARNING "Please Set Luckfox-pico SDK Path. Such as:export LUCKFOX_SDK_PATH=/home/user/luckfox-pico")
    return()
endif()


set(CMAKE_C_COMPILER "${SDK_PATH}/tools/linux/toolchain/arm-rockchip830-linux-uclibcgnueabihf/bin/arm-rockchip830-linux-uclibcgnueabihf-gcc"
)
set(CMAKE_CXX_COMPILER "${SDK_PATH}/tools/linux/toolchain/arm-rockchip830-linux-uclibcgnueabihf/bin/arm-rockchip830-linux-uclibcgnueabihf-g++"
)

project(luckfox_rtsp_opencv)


link_directories(${CMAKE_CURRENT_SOURCE_DIR}/lib)
#add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/3rdparty/ 3rdparty.out)

set(CMAKE_INSTALL_RPATH "$ORIGIN/lib")
add_definitions(-DRV1106_1103)


set(OpenCV_DIR "${CMAKE_CURRENT_SOURCE_DIR}/lib/cmake/opencv4")
find_package(OpenCV REQUIRED)

set(THREADS_PREFER_PTHREAD_FLAG ON)
find_package(Threads REQUIRED)


add_executable(${PROJECT_NAME}  
               ${CMAKE_CURRENT_SOURCE_DIR}/src/main.cc
               ${CMAKE_CURRENT_SOURCE_DIR}/src/luckfox_mpi.cc 
               )

add_compile_options(-g -Wall
                    -DISP_HW_V30 -DRKPLATFORM=ON -DARCH64=OFF
                    -DROCKIVA -DUAPI2
                    -D_LARGEFILE_SOURCE -D_LARGEFILE64_SOURCE -D_FILE_OFFSET_BITS=64 
                    )

set(CMAKE_EXE_LINKER_FLAGS  "${CMAKE_EXE_LINKER_FLAGS} -Wl,-rpath-link,${CMAKE_CURRENT_SOURCE_DIR}/lib:/usr/lib")

target_link_libraries(${PROJECT_NAME}  
                    ${OpenCV_LIBS}
                    rockiva
                    sample_comm
                    rockit
                    rockchip_mpp
                    rkaiq
                    pthread
                    m
                    dl
                    rtsp
                    rga
                    Threads::Threads
                    )


target_include_directories(${PROJECT_NAME} PRIVATE                        
                           ${CMAKE_CURRENT_SOURCE_DIR}
                           ${CMAKE_CURRENT_SOURCE_DIR}/3rdparty/rknpu2/include
                           ${OpenCV_INCLUDE_DIRS}
                           ${CMAKE_CURRENT_SOURCE_DIR}/common 
                           ${CMAKE_CURRENT_SOURCE_DIR}/common/isp3.x   
                           ${CMAKE_CURRENT_SOURCE_DIR}/include
                           ${CMAKE_CURRENT_SOURCE_DIR}/include/rkaiq
                           ${CMAKE_CURRENT_SOURCE_DIR}/include/rkaiq/uAPI2
                           ${CMAKE_CURRENT_SOURCE_DIR}/include/rkaiq/common
                           ${CMAKE_CURRENT_SOURCE_DIR}/include/rkaiq/xcore
                           ${CMAKE_CURRENT_SOURCE_DIR}/include/rkaiq/algos
                           ${CMAKE_CURRENT_SOURCE_DIR}/include/rkaiq/iq_parser
                           ${CMAKE_CURRENT_SOURCE_DIR}/include/rkaiq/iq_parser_v2
                           ${CMAKE_CURRENT_SOURCE_DIR}/include/rkaiq/smartIr 
                          )
                    
set(CMAKE_INSTALL_PREFIX "${CMAKE_CURRENT_SOURCE_DIR}/luckfox_rtsp_opencv_demo")
install(TARGETS ${PROJECT_NAME} DESTINATION ${CMAKE_INSTALL_PREFIX})
                          
