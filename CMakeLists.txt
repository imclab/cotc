project(CityOfTheCondemned)
cmake_minimum_required(VERSION 2.6)
cmake_policy(VERSION 2.6)
set(PROJECT_VERSION_MAJOR "0")
#execute_process(COMMAND "git log --pretty=oneline | wc -l"
#                OUTPUT_VARIABLE PROJECT_VERSION_REVISION)
#set(PROJECT_VERSION ${PROJECT_VERSION_MAJOR}.${PROJECT_VERSION_REVISION})

set(EXENAME "7drl")

# Avoid source tree pollution
if(CMAKE_SOURCE_DIR STREQUAL CMAKE_BINARY_DIR)
	message(FATAL_ERROR "In-source builds are not permitted. Make a separate folder for building:\nmkdir build; cd build; cmake ..\nBefore that, remove the files already created:\nrm -rf CMakeCache.txt CMakeFiles")
endif(CMAKE_SOURCE_DIR STREQUAL CMAKE_BINARY_DIR)

# Add a sensible build type default and warning because empty means no optimization and no debug info.
if(NOT CMAKE_BUILD_TYPE)
	message("WARNING: CMAKE_BUILD_TYPE is not defined!\n         Defaulting to CMAKE_BUILD_TYPE=Release.\n         Use ccmake to set a proper value.")
	SET(CMAKE_BUILD_TYPE Release CACHE STRING "Choose the type of build, options are: None Debug Release RelWithDebInfo MinSizeRel." FORCE)
endif(NOT CMAKE_BUILD_TYPE)

# Set default compile flags for GCC
if(CMAKE_COMPILER_IS_GNUCXX)
	message(STATUS "GCC detected, adding compile flags")
	set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -pthread -std=c++98 -pedantic -Wall -Wno-missing-field-initializers")
endif(CMAKE_COMPILER_IS_GNUCXX)


list(APPEND LIBS ncurses)

# Get revision number from Git
set(VERSION_H ${CMAKE_CURRENT_BINARY_DIR}/version.h)
add_custom_command(
	OUTPUT ${VERSION_H}
	COMMAND ${CMAKE_CURRENT_SOURCE_DIR}/generate-version.sh
	DEPENDS ${CMAKE_CURRENT_SOURCE_DIR}/*
)
include_directories("${CMAKE_CURRENT_BINARY_DIR}")

file(GLOB SOURCE_FILES "*.cc")
file(GLOB HEADER_FILES "*.hh")

set(SOURCES ${SOURCE_FILES} ${HEADER_FILES} ${VERSION_H})

add_executable(${EXENAME} ${SOURCES})
target_link_libraries(${EXENAME} ${LIBS})

