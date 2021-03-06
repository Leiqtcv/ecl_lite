##############################################################################
# Cmake
##############################################################################

cmake_minimum_required(VERSION 3.5)
project(ecl_time_lite)

##############################################################################
# Find Packages
##############################################################################

# Turn -isystem off. Actually like to see warnings from underlying packages
# and regardless, have run into trouble because of the ordering it induces.
set(CMAKE_NO_SYSTEM_FROM_IMPORTED TRUE)

find_package(ament_cmake_ros REQUIRED)
find_package(ecl_build REQUIRED)
find_package(ecl_config REQUIRED)
find_package(ecl_errors REQUIRED)

##############################################################################
# Project Configuration
##############################################################################

ament_package_xml()
ecl_enable_cxx14_compiler()
ecl_enable_cxx_warnings()

file(MAKE_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/include)

# This configures ECL_PLATFORM_TIME_LIBRARIES and
# other ECL_PLATFORM_HAS_XXX flags
ecl_detect_timers()

##############################################################################
# Sources
##############################################################################

add_subdirectory(include)
add_subdirectory(src)

##############################################################################
# Exports
##############################################################################

# Downstream CMake3 compatibility 
ament_export_interfaces(HAS_LIBRARY_TARGET ${PROJECT_NAME})
# Downstream CMake2 compatibility
ament_export_include_directories(include)
ament_export_libraries(${PROJECT_NAME})

ament_export_dependencies(
    ecl_config
    ecl_errors
)
ament_package()
