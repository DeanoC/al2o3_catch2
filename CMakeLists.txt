set(LibName al2o3_catch2)
project(${LibName})

set(Interface
		catch2.hpp
		)
set(Src
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
