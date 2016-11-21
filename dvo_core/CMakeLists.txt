cmake_minimum_required(VERSION 2.8.3)
project(dvo_core)

#set the default path for built executables to the "bin" directory
# set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)
#set the default path for built libraries to the "lib" directory
# set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib)

# enable sse3
set(CMAKE_CXX_FLAGS "-msse3 -O3 -march=native")

find_package(catkin REQUIRED COMPONENTS
)

find_package(sophus REQUIRED)

find_package(OpenCV REQUIRED)
find_package(VTK REQUIRED)
find_package(PCL REQUIRED)

catkin_package(
  INCLUDE_DIRS include
  LIBRARIES dvo_core
  CATKIN_DEPENDS
  DEPENDS system_lib sophus
)

include_directories(
  ${VTK_INCLUDE_DIRS}
  ${Eigen_INCLUDE_DIRS}
  ${PCL_INCLUDE_DIRS}
  include
)

add_library(${PROJECT_NAME} 
  src/core/interpolation.cpp
  src/core/intrinsic_matrix.cpp
  src/core/least_squares.cpp
  src/core/math_sse.cpp
  src/core/rgbd_image.cpp
  src/core/rgbd_image_sse.cpp
  src/core/point_selection.cpp
  src/core/surface_pyramid.cpp
  src/core/weight_calculation.cpp
    
  src/util/histogram.cpp
   
  src/visualization/camera_trajectory_visualizer.cpp
    
  src/dense_tracking.cpp
  src/dense_tracking_impl.cpp
  src/dense_tracking_config.cpp
)

target_link_libraries(${PROJECT_NAME}
  ${catkin_LIBRARIES}
  ${Eigen_LIBRARIES}
  ${VTK_LIBRARIES}
  ${PCL_INCLUDE_DIRS}
)

add_library(dvo_visualization
  src/visualization/async_point_cloud_builder.cpp
  src/visualization/point_cloud_aggregator.cpp
  src/visualization/pcl_camera_trajetory_visualizer.cpp
)

target_link_libraries(dvo_visualization
  ${PROJECT_NAME} 
)

#rosbuild_add_executable(sse_test
#    src/sse_test.cpp
#    src/core/math_sse.cpp
#)
