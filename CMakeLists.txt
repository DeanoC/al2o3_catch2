cmake_minimum_required(VERSION 3.12.4)
get_directory_property(hasParent PARENT_DIRECTORY)
if(NOT hasParent)
	include(FetchContent)
	FetchContent_Declare( al2o3 GIT_REPOSITORY https://github.com/DeanoC/al2o3 GIT_TAG master )
	FetchContent_GetProperties(al2o3)
	if(NOT al2o3_POPULATED)
		FetchContent_Populate(al2o3)
		add_subdirectory(${al2o3_SOURCE_DIR} ${al2o3_BINARY_DIR})
	endif()
	INIT_AL2O3(${CMAKE_CURRENT_SOURCE_DIR})
endif ()


SET_MIN_VERSIONS()

set(LibName al2o3_catch2)
project(${LibName})

set(Interface
		catch2.hpp
		)
set(Src
		dummy.cpp
		)

FetchContent_Declare(
		catchorg_catch2
		GIT_REPOSITORY https://github.com/catchorg/Catch2
		GIT_TAG master
)
FetchContent_GetProperties(catchorg_catch2)
if(NOT catchorg_catch2_POPULATED)
	FetchContent_Populate(catchorg_catch2)
	#don't add subdirectory as we just want the single header not the tests etc.
endif()

set(Deps
		al2o3_platform
		)

ADD_LIB(${LibName} "${Interface}" "${Src}" "${Deps}")
target_include_directories(${LibName} INTERFACE ${catchorg_catch2_SOURCE_DIR})

set( Tests
		runner.cpp
		)

set( TestDeps
		)
ADD_LIB_TESTS(${LibName} "${Interface}" "${Tests}" "${TestDeps}")
