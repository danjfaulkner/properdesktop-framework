# Clients of system services, one library per directory, linking only
# the toolkit's base library. They form the ProperDesktopClients package; a
# consumer asks for them by directory name:
#   find_package(ProperDesktopClients REQUIRED COMPONENTS appearance)
#   target_link_libraries(app PRIVATE ProperDesktop::AppearanceClient)

find_package(ProperDesktopToolkit REQUIRED)

set(PROPERDESKTOP_FRAMEWORK_CLIENTS "all" CACHE STRING
    "The clients/ directories to build: all, or a list of their names")

# properdesktop_add_client(<name> TARGET <Target> SOURCES ... HEADERS ... [LIBRARIES ...])
# builds libproperdesktop-<name>-client as ProperDesktop::<Target>, its headers included
# as <properdesktop/<name>/...>.
function(properdesktop_add_client name)
    cmake_parse_arguments(arg "" "TARGET" "SOURCES;HEADERS;LIBRARIES" ${ARGN})
    properdesktop_framework_add_library(properdesktop-${name}-client
        TARGET ${arg_TARGET}
        MODULE ${name}
        EXPORT ProperDesktopClient${name}Targets
        SOURCES ${arg_SOURCES}
        HEADERS ${arg_HEADERS}
        PUBLIC_LIBRARIES ProperDesktop::ToolkitBase ${arg_LIBRARIES}
    )
    properdesktop_framework_export(ProperDesktopClients ProperDesktopClient${name}Targets)
endfunction()

file(GLOB client_directories LIST_DIRECTORIES true ${CMAKE_CURRENT_SOURCE_DIR}/*)
foreach(directory ${client_directories})
    get_filename_component(name ${directory} NAME)
    if(NOT EXISTS ${directory}/CMakeLists.txt)
        continue()
    endif()
    if(PROPERDESKTOP_FRAMEWORK_CLIENTS STREQUAL "all" OR name IN_LIST PROPERDESKTOP_FRAMEWORK_CLIENTS)
        add_subdirectory(${name})
    endif()
endforeach()

properdesktop_framework_add_package(ProperDesktopClients)
