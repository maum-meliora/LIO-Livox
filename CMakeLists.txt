cmake_minimum_required(VERSION 3.10)

# For compatibility
cmake_policy(SET CMP0144 OLD)
cmake_policy(SET CMP0148 OLD)

project(lio_livox LANGUAGES CXX)

set(CMAKE_BUILD_TYPE "Release")
set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

set(CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR}/cmake)

find_package(catkin REQUIRED COMPONENTS
    message_generation
    geometry_msgs
    nav_msgs
    sensor_msgs
    std_msgs
    visualization_msgs
    roscpp
    rospy
    tf
    tf_conversions
    rosbag
    livox_ros_driver2
    eigen_conversions
    pcl_conversions
    pcl_ros
    message_filters
    std_srvs
)

find_package(Eigen3 REQUIRED)
find_package(PCL REQUIRED)
find_package(Ceres REQUIRED)
find_package(OpenCV REQUIRED)
find_package(SuiteSparse REQUIRED)

include_directories(include
  ${catkin_INCLUDE_DIRS}
  ${EIGEN3_INCLUDE_DIR}
  ${PCL_INCLUDE_DIRS}
  ${CERES_INCLUDE_DIRS}
  ${OpenCV_INCLUDE_DIRS}
  ${SUITESPARSE_INCLUDE_DIRS}
)

##################
## ROS messages ##
##################

catkin_package(
  CATKIN_DEPENDS
    geometry_msgs
    nav_msgs
    roscpp
    rospy
    std_msgs
    visualization_msgs
    message_runtime
    pcl_conversions
    pcl_ros
    message_filters
    std_srvs
    tf
    tf_conversions
    eigen_conversions
  DEPENDS
    PCL
    OpenCV
  INCLUDE_DIRS
    include
)

add_executable (ScanRegistration 
    src/lio/ScanRegistration.cpp
    src/lio/LidarFeatureExtractor.cpp
    src/segment/segment.cpp
    src/segment/pointsCorrect.cpp
)

target_link_libraries(ScanRegistration
    ${catkin_LIBRARIES}
    ${PCL_LIBRARIES}
    ${OpenCV_LIBRARIES}
)

add_executable (PoseEstimation 
    src/lio/PoseEstimation.cpp
    src/lio/Estimator.cpp
    src/lio/IMUIntegrator.cpp
    src/lio/ceresfunc.cpp 
    src/lio/Map_Manager.cpp
)

target_link_libraries(PoseEstimation
    ${catkin_LIBRARIES}
    ${PCL_LIBRARIES}
    ${CERES_LIBRARIES}
)

