##############################################################################
# Cmake
##############################################################################

cmake_minimum_required(VERSION 3.5)
project(ecl_converters_lite)

##############################################################################
# Find Packages
##############################################################################

# Turn -isystem off. Actually like to see warnings from underlying packages
# and regardless, have run into trouble because of the ordering it induces.
set(CMAKE_NO_SYSTEM_FROM_IMPORTED TRUE)

find_package(ament_cmake_ros REQUIRED)
find_package(ecl_build REQUIRED)
find_package(ecl_config REQUIRED)

##############################################################################
# Project Configuration
##############################################################################

ecl_enable_cxx14_compiler()
ecl_enable_cxx_warnings()

##############################################################################
# Header Library
##############################################################################

add_library(${PROJECT_NAME} INTERFACE)
target_include_directories(
  ${PROJECT_NAME}
  INTERFACE
    $<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}/include>
    $<INSTALL_INTERFACE:include>
)
target_link_libraries(${PROJECT_NAME} INTERFACE ecl_config::ecl_config)

install(TARGETS ${PROJECT_NAME} EXPORT ${PROJECT_NAME}
   RUNTIME DESTINATION bin
   ARCHIVE DESTINATION lib
   LIBRARY DESTINATION lib
   INCLUDES DESTINATION include
)

##############################################################################
# Sources
##############################################################################

add_subdirectory(include)
add_subdirectory(src)

##############################################################################
# Exports
##############################################################################

# Downstream CMake3 compatibility 
ament_export_interfaces(${PROJECT_NAME})
# Downstream CMake2 compatibility
ament_export_include_directories(include)

ament_export_dependencies(ecl_config)
ament_package()
