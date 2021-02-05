cmake_minimum_required(VERSION 3.0.2)
project(stereo)

SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")


#find_package(catkin REQUIRED cv_bridge dynamic_reconfigure image_geometry image_proc image_transport message_filters nodelet sensor_msgs stereo_msgs)
#find_package(Boost REQUIRED COMPONENTS thread)
find_package(catkin REQUIRED COMPONENTS
  cv_bridge
  image_transport
  message_filters
  roscpp
  rospy
  std_msgs
  pcl_conversions
  pcl_ros roscpp
  sensor_msgs
  visualization_msgs
)

find_package(PCL 1.7 REQUIRED) # you probably have this around somewhere

# add these lines afterwards
if(DEFINED PCL_LIBRARIES)
    list(REMOVE_ITEM PCL_LIBRARIES "vtkproj4")
endif()


if(cv_bridge_VERSION VERSION_GREATER "1.12.0")
  add_compile_options(-std=c++11)
endif()

# Dynamic reconfigure support
# generate_dynamic_reconfigure_options(cfg/Disparity.cfg)

catkin_package(
  CATKIN_DEPENDS image_geometry cv_bridge #image_proc sensor_msgs stereo_msgs
  #INCLUDE_DIRS include
  LIBRARIES ${PROJECT_NAME}
)

include_directories(include ${catkin_INCLUDE_DIRS})


find_package(OpenCV REQUIRED)
include_directories(${catkin_INCLUDE_DIRS} ${OpenCV_INCLUDE_DIRS} ${PCL_INCLUDE_DIRS})

# See note in image_proc/CMakeLists.txt
add_definitions(-DOPENCV_TRAITS_ENABLE_DEPRECATED)



## Declare a C++ executable
add_executable(stereo_node src/stereo_node.cpp)
add_executable(calib_camera src/calib_camera.cpp)
add_executable(stereo_tf_broadcaster src/stereo_tf_broadcaster.cpp)
add_executable(draw_boxes src/draw_boxes.cpp)



## Specify libraries to link a library or executable target against
target_link_libraries(stereo_node ${catkin_LIBRARIES}  ${OpenCV_LIBRARIES} ${PCL_LIBRARIES})
target_link_libraries(calib_camera ${catkin_LIBRARIES}  ${OpenCV_LIBRARIES})
target_link_libraries(stereo_tf_broadcaster ${catkin_LIBRARIES} ${OpenCV_LIBRARIES} ${PCL_LIBRARIES})
target_link_libraries(draw_boxes ${catkin_LIBRARIES} ${OpenCV_LIBRARIES})


# Nodelet library
#add_library(${PROJECT_NAME} src/libstereo_image_proc/rectification_processing.cpp src/libstereo_image_proc/processor.cpp  src/libstereo_image_proc/points2_nodelet_proc.cpp  src/libstereo_image_proc/process_disparity.cpp src/libstereo_image_proc/finish_processing.cpp src/libstereo_image_proc/point_processing.cpp src/libstereo_image_proc/points2_processing.cpp  src/nodelets/disparity.cpp src/nodelets/point_cloud2.cpp)
#target_link_libraries( 
#	${PROJECT_NAME} 
#	${catkin_LIBRARIES}
#       ${OpenCV_LIBRARIES}
#)
#add_dependencies(${PROJECT_NAME} ${PROJECT_NAME}_gencfg)
#install(TARGETS ${PROJECT_NAME}
#        ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
#        LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
#        RUNTIME DESTINATION ${CATKIN_GLOBAL_BIN_DESTINATION}
#)
#install(FILES nodelet_plugins.xml
#        DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
#)

# Standalone node
#add_executable(stereoimageproc_exe src/nodes/stereo_image_proc.cpp)
#target_link_libraries(stereoimageproc_exe stereo_image_proc)
#SET_TARGET_PROPERTIES(stereoimageproc_exe PROPERTIES OUTPUT_NAME stereo_image_proc)
#install(TARGETS stereoimageproc_exe
#        DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
#)

# install the launch file
#install(DIRECTORY launch
#        DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/
#)

# install the include directory
#install(DIRECTORY include/${PROJECT_NAME}/
#        DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
#)


