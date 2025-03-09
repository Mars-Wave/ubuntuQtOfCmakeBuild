cmake_minimum_required(VERSION 3.15)
project(MyOFApp)

set(CMAKE_EXPORT_COMPILE_COMMANDS ON)  # Helps IDE with code navigation

# =============================================
# 1. Configure OpenFrameworks Core
# =============================================

# set(OF_ROOT /directory/of/your/OF/fromSource/repo ) # https://github.com/openframeworks/openFrameworks/blob/master/INSTALL_FROM_GITHUB.md
set(OF_COMPILED_DIR ${OF_ROOT}/libs/openFrameworksCompiled)
set(OF_HEADERS_DIR ${OF_ROOT}/libs)


include(FetchContent)

FetchContent_Declare(
  pugixml
  GIT_REPOSITORY https://github.com/zeux/pugixml.git
  GIT_TAG v1.13  # Use the latest release tag
)
FetchContent_MakeAvailable(pugixml)

find_package(Boost REQUIRED COMPONENTS system filesystem)
find_package(OpenSSL REQUIRED)
find_package(PkgConfig REQUIRED)
pkg_check_modules(CAIRO REQUIRED cairo)

# Include OpenFrameworks directories
add_library(openframeworks STATIC IMPORTED)
set_target_properties(openframeworks PROPERTIES
    IMPORTED_LOCATION ${OF_COMPILED_DIR}/lib/linux64/libopenFrameworks.a
)

target_include_directories(openframeworks INTERFACE
    ${OF_HEADERS_DIR}/glm/include
    ${OF_HEADERS_DIR}/kiss/include
    ${OF_HEADERS_DIR}/fmod/include
    ${OF_HEADERS_DIR}/json/include
    ${OF_HEADERS_DIR}/tess2/include
    ${OF_HEADERS_DIR}/utf8/include
    ${OF_ROOT}/libs/openFrameworks
    ${OF_ROOT}/libs/openFrameworks/utils
    ${OF_ROOT}/libs/openFrameworks/3d
    ${OF_ROOT}/libs/openFrameworks/app
    ${OF_ROOT}/libs/openFrameworks/communication
    ${OF_ROOT}/libs/openFrameworks/events
    ${OF_ROOT}/libs/openFrameworks/gl
    ${OF_ROOT}/libs/openFrameworks/graphics
    ${OF_ROOT}/libs/openFrameworks/math
    ${OF_ROOT}/libs/openFrameworks/sound
    ${OF_ROOT}/libs/openFrameworks/types
    ${OF_ROOT}/libs/openFrameworks/video
    ${CAIRO_INCLUDE_DIRS}
)


# =============================================
# 2. Configure Addons
#    Collection of Addons files must comply
#    with the locations of the .hpp / .cpp
#    files. i.e: ofxGui only has in 
#    OF_ADDONS_DIR/ofxGui/src
#    but ofxVectoGraphics has in 
#    OF_ADDONS_DIR/ofxVectorGraphics/src and
#    OF_ADDONS_DIR/ofxVectorGraphics/libs
# =============================================
set(OF_ADDONS_DIR ${OF_ROOT}/addons)
target_include_directories(openframeworks INTERFACE
    ${OF_ADDONS_DIR}/ofxGui/src
    ${OF_ADDONS_DIR}/ofxVectorGraphics/libs
    ${OF_ADDONS_DIR}/ofxVectorGraphics/src
)

file(GLOB_RECURSE OFXGUI_SOURCES ${OF_ADDONS_DIR}/ofxGui/src/*)

# Correctly collect both src and libs directories for ofxVectorGraphics
file(GLOB_RECURSE OFXVECTOR_SRC_SOURCES ${OF_ADDONS_DIR}/ofxVectorGraphics/src/*.cpp)
file(GLOB_RECURSE OFXVECTOR_LIBS_SOURCES ${OF_ADDONS_DIR}/ofxVectorGraphics/libs/*.cpp)
set(OFXVECTOR_SOURCES ${OFXVECTOR_SRC_SOURCES} ${OFXVECTOR_LIBS_SOURCES})

# =============================================
# 3. Link Libraries
# =============================================
# Find static libraries
file(GLOB TESS2_LIB ${OF_HEADERS_DIR}/tess2/lib/linux64/libtess2.a)
file(GLOB KISS_LIB ${OF_HEADERS_DIR}/kiss/lib/linux64/libkiss.a)
file(GLOB PUGIXML_LIB ${OF_HEADERS_DIR}/pugixml/lib/linux64/libpugixml.a)

set(SYSTEM_LIBS
    GL GLU GLEW glut openal
    gtk-3 pthread freeimage freetype
    rtaudio sndfile udev X11 Xrandr Xxf86vm Xi
    dl mpg123 fontconfig uriparser curl
    gstreamer-1.0 gstapp-1.0 gstbase-1.0
    gstvideo-1.0 gstaudio-1.0 gstpbutils-1.0
    glib-2.0 gobject-2.0 glfw
    ${CAIRO_LIBRARIES}
)


# =============================================
# 4. Create Executable
# =============================================
file(GLOB_RECURSE APP_SOURCES ${CMAKE_CURRENT_SOURCE_DIR}/src/*.cpp)

add_executable(${PROJECT_NAME} ${APP_SOURCES} ${OFXGUI_SOURCES} ${OFXVECTOR_SOURCES})

target_link_libraries(${PROJECT_NAME}
    openframeworks
    ${TESS2_LIB}
    ${KISS_LIB}
    ${PUGIXML_LIB}
    ${SYSTEM_LIBS}
    OpenSSL::SSL OpenSSL::Crypto Boost::boost pugixml::pugixml
)

set_target_properties(${PROJECT_NAME} PROPERTIES
    RUNTIME_OUTPUT_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/bin
    CXX_STANDARD 17  # Ensure C++17 standard
)

# =============================================
# 5. Copy Data Folder
# =============================================
if(EXISTS ${CMAKE_CURRENT_SOURCE_DIR}/data)
    add_custom_command(
        TARGET ${PROJECT_NAME} POST_BUILD
        COMMAND ${CMAKE_COMMAND} -E copy_directory
        ${CMAKE_CURRENT_SOURCE_DIR}/data
        ${CMAKE_CURRENT_SOURCE_DIR}/bin/data
    )
endif()
