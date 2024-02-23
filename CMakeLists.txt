cmake_minimum_required(VERSION 3.10)

set(CMAKE_C_COMPILER "/home/cro/Sdk/luckfox-pico/tools/linux/toolchain/arm-rockchip830-linux-uclibcgnueabihf/bin/arm-rockchip830-linux-uclibcgnueabihf-gcc"
)
set(CMAKE_CXX_COMPILER "/home/cro/Sdk/luckfox-pico/tools/linux/toolchain/arm-rockchip830-linux-uclibcgnueabihf/bin/arm-rockchip830-linux-uclibcgnueabihf-g++"
)

project(luckfox_rtsp_opencv)

include_directories(${CMAKE_CURRENT_SOURCE_DIR}/common 
                    ${CMAKE_CURRENT_SOURCE_DIR}/common/isp3.x   
                    ${CMAKE_CURRENT_SOURCE_DIR}/media_out/include
                    ${CMAKE_CURRENT_SOURCE_DIR}/media_out/include/rkaiq
                    ${CMAKE_CURRENT_SOURCE_DIR}/media_out/include/rkaiq/uAPI2
                    ${CMAKE_CURRENT_SOURCE_DIR}/media_out/include/rkaiq/common
                    ${CMAKE_CURRENT_SOURCE_DIR}/media_out/include/rkaiq/xcore
                    ${CMAKE_CURRENT_SOURCE_DIR}/media_out/include/rkaiq/algos
                    ${CMAKE_CURRENT_SOURCE_DIR}/media_out/include/rkaiq/iq_parser
                    ${CMAKE_CURRENT_SOURCE_DIR}/media_out/include/rkaiq/iq_parser_v2
                    ${CMAKE_CURRENT_SOURCE_DIR}/media_out/include/rkaiq/smartIr)

link_directories(${CMAKE_CURRENT_SOURCE_DIR}/lib
                 ${CMAKE_CURRENT_SOURCE_DIR}/media_out/lib 
                )

#add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/3rdparty/ 3rdparty.out)


set(CMAKE_INSTALL_RPATH "$ORIGIN/lib")
add_definitions(-DRV1106_1103)


#Opencv 4
set(OpenCV_DIR "${CMAKE_CURRENT_SOURCE_DIR}/lib/cmake/opencv4")
find_package(OpenCV REQUIRED)
include_directories(${OpenCV_INCLUDE_DIRS})

add_executable(${PROJECT_NAME}  
               ${CMAKE_CURRENT_SOURCE_DIR}/src/rtsp_opencv.cc
               )

add_compile_options(-g -Wall
                    -DISP_HW_V30 -DRKPLATFORM=ON -DARCH64=OFF
                    -DROCKIVA -DUAPI2
                    -D_LARGEFILE_SOURCE -D_LARGEFILE64_SOURCE -D_FILE_OFFSET_BITS=64 
                    )

set(CMAKE_EXE_LINKER_FLAGS  "${CMAKE_EXE_LINKER_FLAGS} -Wl,-rpath-link,${CMAKE_CURRENT_SOURCE_DIR}/lib:/usr/lib")

set(THREADS_PREFER_PTHREAD_FLAG ON)
find_package(Threads REQUIRED)
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


target_link_libraries(${PROJECT_NAME}     
                      ${LIBRKNNRT}
                     )

target_include_directories(${PROJECT_NAME} PRIVATE                        
                           ${CMAKE_CURRENT_SOURCE_DIR}
                           ${CMAKE_CURRENT_SOURCE_DIR}/3rdparty/rknpu2/include
                           ${OpenCV_LIBS}
                          )
                    
set(CMAKE_INSTALL_PREFIX "${CMAKE_CURRENT_SOURCE_DIR}/luckfox_rtsp_opencv_demo")
install(TARGETS ${PROJECT_NAME} DESTINATION ${CMAKE_INSTALL_PREFIX})
                          