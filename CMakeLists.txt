cmake_minimum_required(VERSION 3.20)
project(alma)
set(alma_VERSION 3.4.2)

# library settings
set(alma_SOURCES src/alma.cpp)
set(alma_PUBLIC_HEADERS include/alma.h)

# create a library
add_library(alma SHARED ${alma_SOURCES})
#target_include_directories(alma PUBLIC include)
target_include_directories(alma PUBLIC
        # for compatibility with install setup
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
        $<INSTALL_INTERFACE:include>
)

set_property(TARGET alma PROPERTY VERSION ${alma_VERSION})
set_property(TARGET alma PROPERTY SOVERSION 3)
set_property(TARGET alma PROPERTY INTERFACE_alma_MAJOR_VERSION 3)
set_property(TARGET alma APPEND PROPERTY COMPATIBLE_INTERFACE_STRING alma_MAJOR_VERSION)

install(TARGETS alma
        EXPORT almaTargets
        LIBRARY DESTINATION lib
        ARCHIVE DESTINATION lib
        RUNTIME DESTINATION bin
)
install(
        FILES ${alma_PUBLIC_HEADERS}
        DESTINATION include
        COMPONENT Devel
)


include(CMakePackageConfigHelpers)
write_basic_package_version_file(
        "${CMAKE_CURRENT_BINARY_DIR}/alma/almaConfigVersion.cmake"
        VERSION ${alma_VERSION}
        COMPATIBILITY AnyNewerVersion
)

install(EXPORT almaTargets
        FILE almaTargets.cmake
        NAMESPACE Alma::
        DESTINATION cmake
)
install(
        FILES
        cmake/almaConfig.cmake
        "${CMAKE_CURRENT_BINARY_DIR}/alma/almaConfigVersion.cmake"
        DESTINATION cmake
        COMPONENT Devel
)
