cmake_minimum_required(VERSION 2.8)

project(Template)

set(CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/cmake/Modules/" ${CMAKE_MODULE_PATH})

set(CMAKE_BUILD_TYPE Debug)

find_package(PkgConfig)

##Optional builds n things:
option(Build_O3 "Build with O3 optimization" OFF )

if ( Build_O3 )
    set (CMAKE_C_FLAGS "${CMAKE_C_FLAGS}  -O3")
    set (CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -O3")
#    include (${CMAKE_CURRENT_SOURCE_DIR}/cmake/Modules/CheckSSEFeatures.cmake)
endif( Build_O3 )

find_package(OpenMP)
if (OPENMP_FOUND)
    set (CMAKE_C_FLAGS "${CMAKE_C_FLAGS} ${OpenMP_C_FLAGS}")
    set (CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${OpenMP_CXX_FLAGS}")
endif()

find_package( glfw3 REQUIRED )
find_package( OpenGL REQUIRED )
find_package( Eigen3 QUIET )
if( NOT Eigen3_FOUND)
  message( "Attempting to locate local eigen install as 'Eigen'")
  if(NOT EXISTS ${CMAKE_SOURCE_DIR}/Eigen)
    message( FATAL_ERROR "TODO: Locate Eigen3" )
  endif()
  
endif()

# Use PkgConfig to find glew...
#find_package( glew REQUIRED )
pkg_check_modules(glew REQUIRED glew)

set(
    CMAKE_RUNTIME_OUTPUT_DIRECTORY
    ${CMAKE_HOME_DIRECTORY}/bin
 )

add_definitions(
    -std=c++11
    )

#Handles includes (via directories)
include_directories(
    ${CMAKE_HOME_DIRECTORY}/Eigen
    )


#Handles linking (Via the libraries)
if( Eigen3_FOUND)
  link_libraries(Eigen3::Eigen)
endif()
link_libraries(
    ${OpenGL_LIBRARIES} #opengl + glu
    glfw
    ${glew_LIBRARIES}
    )

#link_directories(
#)

add_executable(
    Template
    main.cpp
    gl_helpers.cpp
    shapes.cpp
)
