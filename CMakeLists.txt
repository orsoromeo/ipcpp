cmake_minimum_required(VERSION 2.8.3)
project(ipcpp)

find_package(catkin REQUIRED
  roscpp
  )

find_package(PkgConfig)

pkg_check_modules(EIGEN3 eigen3 REQUIRED)

catkin_package(
  INCLUDE_DIRS include
 LIBRARIES ${PROJECT_NAME}
  CATKIN_DEPENDS roscpp
)

include_directories(${EIGEN3_INCLUDE_DIRS})
include_directories(include ${catkin_INCLUDE_DIRS})

## Declare a cpp library
add_library(${PROJECT_NAME}
        src/ipcpp.cpp)

target_link_libraries(${PROJECT_NAME} ${catkin_LIBRARIES})

set_target_properties(${PROJECT_NAME} PROPERTIES COMPILE_FLAGS "-std=c++0x -o3")

# Install header files
install(DIRECTORY include/${PROJECT_NAME}/
        DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION})

# Install xml plugin config
install(FILES package.xml
DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION})

# Install libraries
install(TARGETS ${PROJECT_NAME}
  ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
  )

enable_testing()
# example problem solved with IPOPT
add_executable(${PROJECT_NAME}-example
        src/ipcpp.cpp
        )
target_link_libraries(${PROJECT_NAME}-example
        PRIVATE
        ${PROJECT_NAME}
        glpk
        )
add_test(${PROJECT_NAME}-example ${PROJECT_NAME}-example)
