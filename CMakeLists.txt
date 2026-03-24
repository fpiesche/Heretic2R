cmake_minimum_required(VERSION 3.22)
project("Heretic2R" VERSION 0.1 LANGUAGES C)

set(PACKAGE_NAME ${PROJECT_NAME})
set(PACKAGE_TARNAME "heretic2r")
set(PACKAGE_VERSION ${PROJECT_VERSION})
set(PACKAGE_STRING "${PROJECT_NAME} ${PROJECT_VERSION}")
set(PACKAGE_COPYRIGHT "Copyright (C) 2026")
set(PACKAGE_LICENSE "GNU General Public License, version 3")
set(APPSTREAM_APP_ID "io.github.m-x-d.Heretic2R")

set(CMAKE_C_STANDARD 99)

option(BUILD_CLIENTFX "Build client effects dynamic library" ON)
option(BUILD_GAME_CODE "Build compiled game code dynamic library" ON)
option(BUILD_PLAYER "Build player dynamic library" ON)
option(BUILD_REFGL1 "Build ref_gl1 renderer dynamic library" ON)
option(BUILD_SNDSL3 "Build SDL3 audio dynamic library" ON)

# This is used to add the platform suffix to the `game` library name
option(H2R_BUILD_ARCH "Target architecture to build (default: host architecture)" ${CMAKE_SYSTEM_PROCESSOR})

find_package(SDL3 REQUIRED)

add_subdirectory("src")

# Packaging distributions
if (UNIX AND NOT APPLE)
	install(FILES
		${CMAKE_CURRENT_SOURCE_DIR}/README.md
		${CMAKE_CURRENT_SOURCE_DIR}/LICENSE 
		DESTINATION ${CMAKE_INSTALL_DATAROOTDIR}/doc/heretic2r
		PERMISSIONS WORLD_READ GROUP_READ OWNER_READ OWNER_WRITE
	)
	install(FILES
		${CMAKE_CURRENT_SOURCE_DIR}/dist/Default_H2R.cfg
		DESTINATION ${CMAKE_INSTALL_DATAROOTDIR}/games/heretic2
		PERMISSIONS WORLD_READ GROUP_READ OWNER_READ OWNER_WRITE
	)
	install(FILES ${CMAKE_CURRENT_SOURCE_DIR}/dist/linux/${APPSTREAM_APP_ID}.svg
		DESTINATION ${CMAKE_INSTALL_DATAROOTDIR}/icons/hicolor/scalable/apps
		PERMISSIONS WORLD_READ GROUP_READ OWNER_READ OWNER_WRITE
	)
	install(FILES ${CMAKE_CURRENT_SOURCE_DIR}/dist/linux/${APPSTREAM_APP_ID}.desktop
		DESTINATION ${CMAKE_INSTALL_DATAROOTDIR}/applications
		PERMISSIONS WORLD_READ GROUP_READ OWNER_READ OWNER_WRITE
	)
	install(FILES ${CMAKE_CURRENT_SOURCE_DIR}/dist/linux/${APPSTREAM_APP_ID}.metainfo.xml
		DESTINATION ${CMAKE_INSTALL_DATAROOTDIR}/metainfo
		PERMISSIONS WORLD_READ GROUP_READ OWNER_READ OWNER_WRITE
	)
endif()

if (WIN32)
	install(FILES
		${CMAKE_CURRENT_SOURCE_DIR}/README.md
		${CMAKE_CURRENT_SOURCE_DIR}/LICENSE 
		${CMAKE_CURRENT_SOURCE_DIR}/dist/windows/SDL3.dll
		${CMAKE_CURRENT_SOURCE_DIR}/dist/Default_H2R.cfg
		DESTINATION ${CMAKE_INSTALL_PREFIX}/${CMAKE_BINARY_DIR}
	)
endif()

include(CPack)
