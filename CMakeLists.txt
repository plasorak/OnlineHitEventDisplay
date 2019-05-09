cmake_minimum_required(VERSION 3.0 FATAL_ERROR)
message(${CMAKE_CXX_COMPILER})
# You need to tell CMake where to find the ROOT installation. This can be done in a number of ways:
#   - ROOT built with classic configure/make use the provided $ROOTSYS/etc/cmake/FindROOT.cmake
#   - ROOT built with CMake. Add in CMAKE_PREFIX_PATH the installation prefix for ROOT
list(APPEND CMAKE_PREFIX_PATH $ENV{ROOTSYS})

#---Locate the ROOT package and defines a number of variables (e.g. ROOT_INCLUDE_DIRS)
find_package(ROOT REQUIRED COMPONENTS RIO Net XMLIO)
find_program(LSB_RELEASE_EXEC lsb_release)
find_package(TBB REQUIRED)

include(${ROOT_USE_FILE})
string(CONCAT CMAKE_CXX_FLAGS ${CMAKE_CXX_FLAGS} " -Wall -Wextra -Wpedantic -Werror")
#grabs the distribution
execute_process(COMMAND ${LSB_RELEASE_EXEC} -is
  OUTPUT_VARIABLE LSB_RELEASE_ID_SHORT
  OUTPUT_STRIP_TRAILING_WHITESPACE
  )


find_library(LIBPTMP NAMES ptmp PATHS $ENV{PTMP_INSTALL_DIR}/lib64)
include_directories($ENV{PTMP_INC})

# if the distribution is scientific linux, then setup boost in a different way (not using the cmake file from boost)
string(FIND "${LSB_RELEASE_ID_SHORT}" "Scientific" _cmp)
if (_cmp GREATER -1)
  set(Boost_NO_BOOST_CMAKE ON)
else()
  string(CONCAT CMAKE_CXX_FLAGS ${CMAKE_CXX_FLAGS} " -std=c++1y")
endif()


add_executable(OnlineHitEventDisplay app/display.cc)
target_link_libraries(OnlineHitEventDisplay ptmp)
target_link_libraries(OnlineHitEventDisplay ${ROOT_LIBRARIES})
