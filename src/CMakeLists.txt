cmake_minimum_required(VERSION 3.22)

if(BUILD_CLIENTFX)
	add_subdirectory("client effects")
endif()

if(BUILD_GAME_CODE)
	add_subdirectory("game")
endif()

if(BUILD_H2COMMON)
	add_subdirectory("H2Common")
endif()

if(BUILD_LAUNCHER)
	add_subdirectory("launcher")
endif()

if(BUILD_PLAYER)
	add_subdirectory("Player")
endif()

if(BUILD_REFGL1)
	add_subdirectory("ref_gl1")
endif()

if(BUILD_SNDSDL3)
	add_subdirectory("snd_sdl3")
endif()
