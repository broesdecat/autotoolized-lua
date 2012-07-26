cmake_minimum_required(VERSION 2.8)
project (lua CXX)

SET(BUILD_SHARED_LIBS ON)

if(${CMAKE_BUILD_TYPE} MATCHES "Debug")
	set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -O0 -ggdb")	
endif()
	if(${CMAKE_BUILD_TYPE} MATCHES "Release")
	set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -O3")
endif()

set(CMAKE_MODULE_PATH ${CMAKE_SOURCE_DIR}/cmake)

enable_testing()

option(STATIC_LUA "build static library" OFF)

message(STATUS "static lua: ${STATIC_LUA}")

option(LUA_USE_DLOPEN "Enable dlopen support.")
option(LUA_USE_ISATTY "Enable isatty support.")
option(LUA_USE_READLINE "Enable readline support.")
option(LUA_USE_POPEN "Enable lua_popen support.")
option(LUA_USE_ULONGJMP "Try using _longjmp/_setjmp (more efficient)")
option(LUA_USE_APICHECK "Enable API checks.")
option(LUA_USE_MKSTEMP "Use mkstemp.")
option(LUA_ANSI "Disable non-ansi features." OFF)

#
# Lua version
#
set(LUA_VERSION_MAJOR 5)
set(LUA_VERSION_MINOR 1)
set(LUA_VERSION_PATCH 4)
set(LUA_VERSION
				"${LUA_VERSION_MAJOR}.${LUA_VERSION_MINOR}.${LUA_VERSION_PATCH}")
set(LUA_SOVERSION
				"${LUA_VERSION_MAJOR}")
#
# Lua package info.
#
set(CPACK_PACKAGE_VERSION_MAJOR ${lua_VERSION_MAJOR})
set(CPACK_PACKAGE_VERSION_MINOR ${lua_VERSION_MINOR})
set(CPACK_PACKAGE_VERSION_PATCH ${LUA_VERSION_PATCH})
set(CPACK_RESOURCE_FILE_LICENSE "${CMAKE_SOURCE_DIR}/COPYRIGHT")
set(CPACK_RESOURCE_FILE_README "${CMAKE_SOURCE_DIR}/README")
set(CPACK_PACKAGE_VENDOR "Lua.org, PUC-Rio.")
set(CPACK_SOURCE_GENERATOR "TGZ")
set(CPACK_SOURCE_IGNORE_FILES
"/\\\\.;/\\\\.git.*/;~$;build/;CMakeFiles/;CMakeCache;Testing/;cmake_install;CPack;Dart;Makefile$")
set(CPACK_SOURCE_PACKAGE_FILE_NAME
	"${CMAKE_PROJECT_NAME}-${LUA_VERSION_MAJOR}.${LUA_VERSION_MINOR}.${LUA_VERSION_PATCH}")
# MUST be after CPACK_* variables.
include(CPack)

set(COMMON_CFLAGS)
set(COMMON_LDFLAGS " -lm ")

#
# Detect system type
#
if(CMAKE_SYSTEM_NAME STREQUAL "Linux")
	set(LUA_USE_LINUX TRUE)
	set(COMMON_CFLAGS "${COMMON_CFLAGS} -DLUA_USE_LINUX")
	MESSAGE(STATUS "Lua in linux mode")
	set(COMMON_LDFLAGS "${COMMON_LDFLAGS} -Wl,-E -ldl")
else(CMAKE_SYSTEM_NAME STREQUAL "Linux")
	if(APPLE)
		set(LUA_USE_MACOSX TRUE)
		set(COMMON_CFLAGS "${COMMON_CFLAGS} -DLUA_USE_LINUX")
#		set(COMMON_LDFLAGS "${COMMON_LDFLAGS} -lreadline ")
		# use this on Mac OS X 10.3-
		#set(COMMON_CFLAGS "${COMMON_CFLAGS} -DLUA_USE_MACOSX")
	elseif(WIN32)
		set(LUA_WIN TRUE)
		set(COMMON_CFLAGS "${COMMON_CFLAGS} -DLUA_BUILD_AS_DLL")
	elseif(UNIX)
		set(LUA_USE_POSIX TRUE)
		set(COMMON_CFLAGS "${COMMON_CFLAGS} -DLUA_USE_POSIX")
	else(APPLE)
		set(LUA_ANSI TRUE)
		set(COMMON_CFLAGS "${COMMON_CFLAGS} -DLUA_ANSI")
	endif(APPLE)
endif(CMAKE_SYSTEM_NAME STREQUAL "Linux")

add_subdirectory(src)
