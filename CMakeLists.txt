 cmake_minimum_required(VERSION 3.19)

if(NOT "${METAMODULE_SDK_DIR}" STREQUAL "")
	message("METAMODULE_SDK_DIR set by CMake variable ${METAMODULE_SDK_DIR}")
elseif (DEFINED ENV{METAMODULE_SDK_DIR})
    set(METAMODULE_SDK_DIR "$ENV{METAMODULE_SDK_DIR}")
	message("METAMODULE_SDK_DIR set by environment variable ${METAMODULE_SDK_DIR}")
else()
    set(METAMODULE_SDK_DIR "${CMAKE_CURRENT_LIST_DIR}/../metamodule-plugin-sdk")
	message("METAMODULE_SDK_DIR set to default: ${METAMODULE_SDK_DIR}")
endif()

include(${METAMODULE_SDK_DIR}/plugin.cmake)

project(alteredstatemachines VERSION 2.0.0 DESCRIPTION "Altered State Machines for MetaModule" LANGUAGES C CXX ASM)

add_library(alteredstatemachines STATIC)

# Add sources from the Rack source repo
set(SOURCE_DIR ./alteredstatemachines-vcv)

target_sources(alteredstatemachines PRIVATE
    ${SOURCE_DIR}/src/plugin.cpp        
    ${SOURCE_DIR}/src/Aristotle.cpp        
    ${SOURCE_DIR}/src/Eris.cpp        
    ${SOURCE_DIR}/src/Medusa.cpp        
)

# Add includes and compile options for source repo
target_include_directories(alteredstatemachines PRIVATE ${SOURCE_DIR}/src)

# Call this to link and create the plugin file
create_plugin(
    SOURCE_LIB      alteredstatemachines                                          # The cmake target name (defined in add_target)
    PLUGIN_NAME     alteredstatemachines                                          # This must match the brand "slug" used in VCV Rack
    PLUGIN_JSON     ${SOURCE_DIR}/plugin.json              # Path to the plugin.json file used by VCV Rack
    SOURCE_ASSETS   ${CMAKE_CURRENT_LIST_DIR}/assets                 # Path to the assets/ dir containing the PNGs
    DESTINATION     ${CMAKE_CURRENT_LIST_DIR}/metamodule-plugins   # Path to where you want the plugin file output
)
