project(gbvs)
cmake_minimum_required(VERSION 3.0)

set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

find_package(OpenCV REQUIRED)
if(NOT ${OpenCV_FOUND})
  message(FATAL_ERROR "Did not find OpenCV")
else()
  message(STATUS "Found OpenCV at: ${OpenCV_DIR}")
endif(NOT ${OpenCV_FOUND})

find_package(Boost REQUIRED COMPONENTS program_options system thread)
if(NOT ${Boost_FOUND})
  message(FATAL_ERROR "Did not find Boost")
else()
  message(STATUS "Found Boost libs at: ${Boost_LIBRARY_DIRS}")
  message(STATUS "Found Boost include dir at: ${Boost_INCLUDE_DIRS}")
endif(NOT ${Boost_FOUND})

include_directories(src/
  ${OpenCV_INCLUDE_DIR}
  ${Boost_INCLUDE_DIRS}
)

add_library(${PROJECT_NAME} SHARED
  src/fftw++.cc
  src/fftw++.h
  src/GBVS.cpp
  src/GBVS.h
)
target_link_libraries(${PROJECT_NAME}
  ${OpenCV_LIBS}
)

add_executable(gbvs_main src/main.cpp)
target_link_libraries(gbvs_main 
  ${PROJECT_NAME}
  ${OpenCV_LIBS}
  Boost::program_options
  Boost::system
  Boost::thread
)