cmake_minimum_required(VERSION 3.12)
project(libgou
	VERSION 0.1
	DESCRIPTION "Support library for the ODROID-GO Ultra")

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} ${CMAKE_CURRENT_SOURCE_DIR}/cmake)

include(GNUInstallDirs)
find_package(PkgConfig REQUIRED)

set(prefix "${CMAKE_INSTALL_PREFIX}")
add_library(gou SHARED
	src/audio.cpp
	src/context3d.cpp
	src/display.cpp
	src/input.cpp
	src/surface.cpp)

pkg_search_module(ALSA REQUIRED alsa)
pkg_search_module(EGL REQUIRED egl)
pkg_search_module(EVDEV REQUIRED libevdev)
pkg_search_module(PNG REQUIRED libpng)
pkg_search_module(OpenAL REQUIRED openal)
target_link_libraries(gou PUBLIC
	${OpenAL_LIBRARIES}
	${EGL_LIBRARIES}
	${EVDEV_LIBRARIES}
	-lpthread
	-lm
	${PNG_LIBRARIES}
	${ALSA_LIBRARIES})

target_include_directories(gou PRIVATE src/)
set(GOU_HEADERS
	src/audio.h
	src/display.h
	src/ge2d_func.h
	src/input.h
	src/surface.h
	src/context3d.h
	src/ge2d_cmd.h
	src/ge2d.h
	src/ion.h
)

configure_file(gou.pc.in gou.pc @ONLY)
install(TARGETS gou LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR})
install(FILES ${GOU_HEADERS} DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/gou)
install(FILES ${CMAKE_BINARY_DIR}/gou.pc DESTINATION ${CMAKE_INSTALL_DATAROOTDIR}/pkgconfig)
