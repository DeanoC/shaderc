cmake_minimum_required(VERSION 3.13 FATAL_ERROR)
get_directory_property(hasParent PARENT_DIRECTORY)
if (NOT hasParent)
	set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/out_libs)
	set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/out_bin)
	set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/out_bin)
	set(LIB_BASE_PATH ${CMAKE_CURRENT_SOURCE_DIR}/src/libs/)
	include(FetchContent)
	FetchContent_Declare(al2o3 GIT_REPOSITORY https://github.com/DeanoC/al2o3 GIT_TAG master)
	FetchContent_GetProperties(al2o3)
	if (NOT al2o3_POPULATED)
		FetchContent_Populate(al2o3)
		add_subdirectory(${al2o3_SOURCE_DIR} ${al2o3_BINARY_DIR})
	endif ()
endif ()

SET_MIN_VERSIONS()
set(LibName gfx_shaderc)

project(${LibName})
set(Interface
		../shaderc/env.h
		../shaderc/shaderc.h
		../shaderc/shaderc.hpp
		../shaderc/spvc.h
		../shaderc/spvc.hpp
		../shaderc/status.h
		../shaderc/visibility.h

		)
set(Src
		shaderc.cc
		shaderc_private.h
		spvc.cc
		libshaderc_util/src/args.cc
		libshaderc_util/src/compiler.cc
		libshaderc_util/src/file_finder.cc
		libshaderc_util/src/io.cc
		libshaderc_util/src/message.cc
		libshaderc_util/src/resources.cc
		libshaderc_util/src/shader_stage.cc
		libshaderc_util/src/spirv_tools_wrapper.cc
		libshaderc_util/src/version_profile.cc
        )

set(Deps
		gfx_glslang
		gfx_spirvcross
		gfx_spirvtools
		)

ADD_LIB(${LibName} "${Interface}" "${Src}" "${Deps}")
target_include_directories(${LibName} PRIVATE src/libshaderc_util/include)