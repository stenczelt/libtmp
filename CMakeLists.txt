cmake_minimum_required(VERSION 3.20)
project(alma)
set(Upstream_VERSION 3.4.1)

set(CMAKE_INCLUDE_CURRENT_DIR ON)
set(CMAKE_INCLUDE_CURRENT_DIR_IN_INTERFACE ON)

add_library(alma SHARED alma.cpp)
set_property(TARGET alma PROPERTY VERSION ${Upstream_VERSION})
set_property(TARGET alma PROPERTY SOVERSION 3)
set_property(TARGET alma PROPERTY INTERFACE_alma_MAJOR_VERSION 3)
set_property(TARGET alma APPEND PROPERTY COMPATIBLE_INTERFACE_STRING alma_MAJOR_VERSION)

install(TARGETS alma EXPORT almaTargets
        LIBRARY DESTINATION lib
        ARCHIVE DESTINATION lib
        RUNTIME DESTINATION bin
        INCLUDES DESTINATION include
)
install(
        FILES alma.h
        DESTINATION include
        COMPONENT Devel
)

include(CMakePackageConfigHelpers)
write_basic_package_version_file(
        "${CMAKE_CURRENT_BINARY_DIR}/alma/almaConfigVersion.cmake"
        VERSION ${Upstream_VERSION}
        COMPATIBILITY AnyNewerVersion
)

export(EXPORT almaTargets
        FILE "${CMAKE_CURRENT_BINARY_DIR}/alma/almaTargets.cmake"
        NAMESPACE Upstream::
)
configure_file(cmake/almaConfig.cmake
        "${CMAKE_CURRENT_BINARY_DIR}/alma/almaConfig.cmake"
        COPYONLY
)

set(ConfigPackageLocation lib/cmake/alma)
install(EXPORT almaTargets
        FILE
        almaTargets.cmake
        NAMESPACE
        Upstream::
        DESTINATION
        ${ConfigPackageLocation}
)
install(
        FILES
        cmake/almaConfig.cmake
        "${CMAKE_CURRENT_BINARY_DIR}/alma/almaConfigVersion.cmake"
        DESTINATION
        ${ConfigPackageLocation}
        COMPONENT
        Devel
)
