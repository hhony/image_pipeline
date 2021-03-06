cmake_minimum_required(VERSION 2.8)
project(depth_image_proc)

find_package(catkin REQUIRED cmake_modules cv_bridge eigen_conversions image_geometry image_transport message_filters nodelet sensor_msgs stereo_msgs tf2 tf2_ros)

catkin_package(
    INCLUDE_DIRS include
    LIBRARIES ${PROJECT_NAME})

find_package(Boost REQUIRED)
find_package(Eigen REQUIRED)
find_package(OpenCV)
if (OpenCV_VERSION_MAJOR VERSION_EQUAL "3")
  message(STATUS "###### ------ OpenCV 3 enabled.")
  add_definitions("-DOPENCV3=1")
endif()
include_directories(include
        SYSTEM ${BOOST_INCLUDE_DIRS}
               ${catkin_INCLUDE_DIRS}
               ${EIGEN_INCLUDE_DIRS}
)

add_library(${PROJECT_NAME} src/nodelets/convert_metric.cpp
                             src/nodelets/disparity.cpp
                             src/nodelets/point_cloud_xyz.cpp
                             src/nodelets/point_cloud_xyzrgb.cpp
                             src/nodelets/point_cloud_xyzi.cpp
                             src/nodelets/point_cloud_xyz_radial.cpp
                             src/nodelets/point_cloud_xyzi_radial.cpp
                             src/nodelets/register.cpp
)
target_link_libraries(${PROJECT_NAME} ${catkin_LIBRARIES})

install(DIRECTORY include/${PROJECT_NAME}/
    DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
    FILES_MATCHING PATTERN "*.h")

install(TARGETS ${PROJECT_NAME}
        DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
)
install(FILES nodelet_plugins.xml
        DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
)
