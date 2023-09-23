cmake_minimum_required(VERSION 3.5)
project(Map_Compression)

SET(CMAKE_BUILD_TYPE "Debug")
SET(CMAKE_CXX_FLAGS "-std=c++17 -mfma -O3")
SET(CMAKE_CXX_FLAGS_DEBUG "$ENV{CXXFLAGS} -O0 -Wall -g -ggdb -pthread")
MESSAGE("Build type: " ${CMAKE_BUILD_TYPE})

add_definitions(-DCOMPILEDWITHC11)

set(OpenCV_DIR /usr/local/include/opencv4/)
find_package(OpenCV 4.2 REQUIRED)

SET(EIGEN3_INCLUDE_DIR /usr/local/include/eigen3)
find_package(Eigen3 REQUIRED)

SET(ORTOOLS_DIR /usr/local/include/ortools) 
find_package(ortools CONFIG REQUIRED)

SET(Pangolin_DIR /usr/local/include/pangolin)
find_package(Pangolin REQUIRED)

include_directories(
${PROJECT_SOURCE_DIR}
${PROJECT_SOURCE_DIR}/include
${EIGEN3_INCLUDE_DIR}
${Pangolin_INCLUDE_DIRS}
)

set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${PROJECT_SOURCE_DIR}/lib)

add_library(${PROJECT_NAME} SHARED
src/System.cc
src/Tracking.cc
src/LocalMapping.cc
src/LoopClosing.cc
src/ORBextractor.cc
src/ORBmatcher.cc
src/FrameDrawer.cc
src/Converter.cc
src/MapPoint.cc
src/KeyFrame.cc
src/Map.cc
src/MapDrawer.cc
src/Optimizer.cc
src/PnPsolver.cc
src/Frame.cc
src/KeyFrameDatabase.cc
src/Sim3Solver.cc
src/Initializer.cc
src/Viewer.cc
)

target_link_libraries(${PROJECT_NAME}
${OpenCV_LIBS}
${EIGEN3_LIBS}
${Pangolin_LIBRARIES}
${PROJECT_SOURCE_DIR}/Thirdparty/DBoW2/lib/libDBoW2.so
${PROJECT_SOURCE_DIR}/Thirdparty/g2o/lib/libg2o.so
        ortools
        pthread
)

# Build examples

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${PROJECT_SOURCE_DIR}/Examples/RGB-D)

add_executable(rgbd_tum
Examples/RGB-D/rgbd_tum.cc)
target_link_libraries(rgbd_tum ${PROJECT_NAME})

add_executable(rgbd_tum_savemap
        Examples/RGB-D/rgbd_tum_savemap.cc)
target_link_libraries(rgbd_tum_savemap ${PROJECT_NAME})

add_executable(rgbd_tum_loadmap
        Examples/RGB-D/rgbd_tum_loadmap.cc)
target_link_libraries(rgbd_tum_loadmap ${PROJECT_NAME})

add_executable(rgbd_tum_localize
        Examples/RGB-D/rgbd_tum_localize.cc)
target_link_libraries(rgbd_tum_localize ${PROJECT_NAME})

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${PROJECT_SOURCE_DIR}/Examples/Stereo)

add_executable(stereo_kitti
Examples/Stereo/stereo_kitti.cc)
target_link_libraries(stereo_kitti ${PROJECT_NAME})

add_executable(stereo_euroc
Examples/Stereo/stereo_euroc.cc)
target_link_libraries(stereo_euroc ${PROJECT_NAME})

add_executable(stereo_euroc_localize
        Examples/Stereo/stereo_euroc_localize.cc)
target_link_libraries(stereo_euroc_localize ${PROJECT_NAME})

add_executable(stereo_euroc_savemap
        Examples/Stereo/stereo_euroc_savemap.cc)
target_link_libraries(stereo_euroc_savemap ${PROJECT_NAME})

add_executable(stereo_kitti_savemap
        Examples/Stereo/stereo_kitti_savemap.cc)
target_link_libraries(stereo_kitti_savemap ${PROJECT_NAME})

add_executable(stereo_kitti_localize
        Examples/Stereo/stereo_kitti_localize.cc)
target_link_libraries(stereo_kitti_localize ${PROJECT_NAME})

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${PROJECT_SOURCE_DIR}/Examples/Monocular)

add_executable(mono_tum
Examples/Monocular/mono_tum.cc)
target_link_libraries(mono_tum ${PROJECT_NAME})

add_executable(mono_kitti
Examples/Monocular/mono_kitti.cc)
target_link_libraries(mono_kitti ${PROJECT_NAME})

add_executable(mono_euroc
Examples/Monocular/mono_euroc.cc)
target_link_libraries(mono_euroc ${PROJECT_NAME})

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${PROJECT_SOURCE_DIR}/Examples/Tools)
add_executable(slam_compress_map
        Examples/Tools/slam_compress_map.cc)
target_link_libraries(slam_compress_map ${PROJECT_NAME})