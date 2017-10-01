cmake_minimum_required(VERSION 2.8.12)
project(robust_equilibrium_lib_catkin CXX)

include(ExternalProject)

find_package(catkin REQUIRED COMPONENTS qpoases_catkin)

ExternalProject_Add(
  robust-equilibrium-lib_lib
  GIT_REPOSITORY https://github.com/wxmerkt/robust-equilibrium-lib.git #https://github.com/andreadelprete/robust-equilibrium-lib.git
  GIT_TAG 9e5251ec2cdfd4cda07c995cadcba365f75069eb #master
  CMAKE_ARGS -DCMAKE_INSTALL_PREFIX=${CATKIN_DEVEL_PREFIX} -DBUILD_TESTS:BOOL=OFF
  BINARY_DIR ${CMAKE_CURRENT_BINARY_DIR}/robust-equilibrium-lib-build
  SOURCE_DIR ${CMAKE_CURRENT_BINARY_DIR}/robust-equilibrium-lib-src
  BUILD_COMMAND make
  INSTALL_COMMAND ${CMAKE_COMMAND} -E copy ${CMAKE_CURRENT_BINARY_DIR}/robust-equilibrium-lib-build/${CMAKE_SHARED_LIBRARY_PREFIX}robust-equilibrium-lib${CMAKE_SHARED_LIBRARY_SUFFIX} ${CATKIN_DEVEL_PREFIX}/lib
  UPDATE_COMMAND ""
)
## robust-equilibrium-lib does not correctly install include & lib, so we have to do this manually
ExternalProject_Add_Step(robust-equilibrium-lib_lib copy_includes
  DEPENDEES install
  COMMAND ${CMAKE_COMMAND} -E copy_directory ${CMAKE_CURRENT_BINARY_DIR}/robust-equilibrium-lib-src/include ${CATKIN_DEVEL_PREFIX}/include
)

file(MAKE_DIRECTORY ${CATKIN_DEVEL_PREFIX}/include)
catkin_package(
  INCLUDE_DIRS ${CATKIN_DEVEL_PREFIX}/include
  LIBRARIES robust-equilibrium-lib
)

## Install
install(
  DIRECTORY ${CATKIN_DEVEL_PREFIX}/include
  DESTINATION ${CMAKE_INSTALL_PREFIX}
  PATTERN ".git" EXCLUDE
)
install(
  FILES ${CMAKE_CURRENT_BINARY_DIR}/robust-equilibrium-lib-build/${CMAKE_SHARED_LIBRARY_PREFIX}robust-equilibrium-lib${CMAKE_SHARED_LIBRARY_SUFFIX}
  DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
)
