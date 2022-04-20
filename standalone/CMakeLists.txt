cmake_minimum_required(VERSION 3.14...3.22)

project(GreeterStandalone LANGUAGES CXX)

# --- Import tools ----

include(../cmake/tools.cmake)

# ---- Dependencies ----

include(../cmake/CPM.cmake)

CPMAddPackage(
  GITHUB_REPOSITORY jarro2783/cxxopts
  VERSION 3.0.0
  OPTIONS "CXXOPTS_BUILD_EXAMPLES NO" "CXXOPTS_BUILD_TESTS NO" "CXXOPTS_ENABLE_INSTALL YES"
)

CPMAddPackage(
  NAME notcurses
  GITHUB_REPOSITORY dankamongmen/notcurses
  VERSION 3.0.8
  OPTIONS
    "USE_POC OFF"
    "USE_PANDOC OFF"
    "USE_DOXYGEN OFF"
    "USE_DOCTEST OFF"
    "BUILD_EXECUTABLES OFF"
    "USE_MULTIMEDIA ffmpeg"
)
if(notcurses_ADDED)
  target_include_directories(notcurses INTERFACE "${notcurses_SOURCE_DIR}/include")
endif()

CPMAddPackage(NAME Greeter SOURCE_DIR ${CMAKE_CURRENT_LIST_DIR}/..)

# ---- Create standalone executable ----

file(GLOB sources CONFIGURE_DEPENDS ${CMAKE_CURRENT_SOURCE_DIR}/source/*.cpp)

add_executable(${PROJECT_NAME} ${sources})

set_target_properties(${PROJECT_NAME} PROPERTIES CXX_STANDARD 17 OUTPUT_NAME "Greeter")

target_link_libraries(${PROJECT_NAME} Greeter::Greeter cxxopts notcurses notcurses++)
