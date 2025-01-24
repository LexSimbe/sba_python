cmake_minimum_required(VERSION 3.1)
project(sba_python)

if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 11)
endif()

find_package(catkin QUIET)

if (catkin_FOUND)
  find_package(Eigen3 REQUIRED)
  include_directories(include ${catkin_INCLUDE_DIRS} ${EIGEN3_INCLUDE_DIRS})
  add_definitions(${EIGEN3_DEFINITIONS})
  catkin_package(
    INCLUDE_DIRS include
    )
else()
  # execute_process(COMMAND python -c
  #                 "import pybind11_cmake; print(pybind11_cmake.__path__[0])"
  #                 OUTPUT_VARIABLE pybind11_DIR OUTPUT_STRIP_TRAILING_WHITESPACE)
  include_directories(include /usr/include/eigen3/ /usr/include/ /usr/include/suitesparse/)
endif()

# find_package(pybind11 REQUIRED)
add_subdirectory(pybind11)
add_definitions(-DSBA_CHOLMOD)

pybind11_add_module(sba_cpp src/python_interface.cpp src/sba.cpp src/spa.cpp src/spa2d.cpp src/csparse.cpp src/proj.cpp src/node.cpp src/sba_file_io.cpp)
target_link_libraries(sba_cpp PRIVATE openblas lapack cholmod cxsparse)
