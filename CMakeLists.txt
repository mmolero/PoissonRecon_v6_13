cmake_minimum_required(VERSION 3.20)
project(PoissonRecon)

find_package(OpenMP)


set(CMAKE_CXX_FLAGS "${OpenMP_CXX_FLAGS} -fPIC -Wno-deprecated -std=c++11 -D_OPENMP -D__linux__")
set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} ${CMAKE_CXX_FLAGS} -g")
set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS_RELEASE} ${CMAKE_CXX_FLAGS} -Ofast -DRELEASE -funroll-loops -ffast-math")

file(GLOB_RECURSE INCLUDES
    "${PROJECT_SOURCE_DIR}/src/*.h"
    "${PROJECT_SOURCE_DIR}/src/*.cpp"
    "${PROJECT_SOURCE_DIR}/src/*.inl")

list(REMOVE_ITEM INCLUDES ${PROJECT_SOURCE_DIR}/src/PoissonRecon.cpp)
list(REMOVE_ITEM INCLUDES ${PROJECT_SOURCE_DIR}/src/SurfaceTrimmer.cpp)

add_library(poisson_recon_lib SHARED ${INCLUDES})
add_executable(poisson_recon ${PROJECT_SOURCE_DIR}/src/PoissonRecon.cpp)
target_link_libraries(poisson_recon poisson_recon_lib)

#if(${CMAKE_BUILD_TYPE} MATCHES "Debug")
#    set(POISSON_RECON_CXX_FLAGS ${CMAKE_CXX_FLAGS_DEBUG})
#else()
#    set(POISSON_RECON_CXX_FLAGS ${CMAKE_CXX_FLAGS_RELEASE})
#endif()
