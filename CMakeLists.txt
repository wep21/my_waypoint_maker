cmake_minimum_required(VERSION 2.8.3)
project(my_waypoint_maker)


find_package(autoware_build_flags REQUIRED)

find_package(autoware_msgs REQUIRED)

find_package(vector_map REQUIRED)
find_package(catkin REQUIRED COMPONENTS
        gnss
        roscpp
        std_msgs
        geometry_msgs
        nav_msgs
        tf
        waypoint_follower
        lane_planner
        autoware_msgs
        vector_map
        )

find_package(Boost REQUIRED)

catkin_package(
        CATKIN_DEPENDS gnss
        roscpp
        std_msgs
        geometry_msgs
        nav_msgs
        tf
        waypoint_follower
        vector_map
        lane_planner
        autoware_msgs
)

SET(CMAKE_CXX_FLAGS "-O2 -g -Wall ${CMAKE_CXX_FLAGS}")

include_directories(
        ${catkin_INCLUDE_DIRS}
        ${vector_map_INCLUDE_DIRS}
        ${autoware_msgs_INCLUDE_DIRS}
        ${Boost_INCLUDE_DIRS}
)

add_executable(my_waypoint_loader nodes/my_waypoint_loader/waypoint_loader_core.cpp nodes/my_waypoint_loader/waypoint_loader_node.cpp)
target_link_libraries(my_waypoint_loader ${catkin_LIBRARIES})
add_dependencies(my_waypoint_loader
        ${catkin_EXPORTED_TARGETS})

install(TARGETS
        my_waypoint_loader
        ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
        LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
        RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
        )

install(DIRECTORY launch/
        DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/launch
        PATTERN ".svn" EXCLUDE)
