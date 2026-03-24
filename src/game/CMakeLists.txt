cmake_minimum_required(VERSION 3.22)

# This is used to add the platform suffix to the `game` library name
option(GAME_LIBRARY_ARCH_SUFFIX "Name suffix for shared game code library (default: host architecture)" ${CMAKE_HOST_SYSTEM_PROCESSOR})

set(GAME_LIBRARY_NAME "game${GAME_LIBRARY_ARCH_SUFFIX}")
file(GLOB GAMELIB_SOURCE_FILES
	${CMAKE_CURRENT_SOURCE_DIR}/*.c
	${CMAKE_CURRENT_SOURCE_DIR}/../qcommon/Message.c
    ${CMAKE_CURRENT_SOURCE_DIR}/../qcommon/p_dll.c
    ${CMAKE_CURRENT_SOURCE_DIR}/../qcommon/Skeletons.c
)

add_library(${GAME_LIBRARY_NAME} ${GAMELIB_SOURCE_FILES})
target_include_directories(${GAME_LIBRARY_NAME} PRIVATE
	${CMAKE_CURRENT_SOURCE_DIR}
	${CMAKE_CURRENT_SOURCE_DIR}/../qcommon
	${CMAKE_CURRENT_SOURCE_DIR}/../Player
)

message(STATUS "Building game code shared library ${GAME_LIBRARY_NAME}${CMAKE_SHARED_LIBRARY_SUFFIX}")

if(WIN32)
	set(GAMELIB_INSTALL_DEST ${CMAKE_INSTALL_BINDIR}/base/)
elseif(UNIX AND NOT APPLE)
	set(GAMELIB_INSTALL_DEST ${CMAKE_INSTALL_DATAROOTDIR}/games/heretic2/)
else()
	set(GAMELIB_INSTALL_DEST ${CMAKE_INSTALL_LIBDIR})
endif()

install(TARGETS ${GAME_LIBRARY_NAME}
	DESTINATION ${GAMELIB_INSTALL_DEST}
	PERMISSIONS WORLD_READ GROUP_READ OWNER_READ OWNER_WRITE
)
