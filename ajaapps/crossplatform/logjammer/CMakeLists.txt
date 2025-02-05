project(logjammer)

message("added logjammer")

set(LOGJAMMER_TARGET_INCLUDE_DIRS
	${CMAKE_CURRENT_SOURCE_DIR}/../
	${AJA_LIBRARIES_ROOT}
	${AJA_LIB_NTV2_ROOT}/includes)

set(LOGJAMMER_SOURCES main.cpp)

if (CMAKE_SYSTEM_NAME STREQUAL "Windows")
	# noop
elseif (CMAKE_SYSTEM_NAME STREQUAL "Darwin")
	find_library(FOUNDATION_FRAMEWORK Foundation)
	set(EXTRA_LIBRARIES ${FOUNDATION_FRAMEWORK})
elseif (CMAKE_SYSTEM_NAME STREQUAL "Linux")
	set(EXTRA_LIBRARIES dl pthread rt)
endif()

set(LOGJAMMER_TARGET_SOURCES
	${LOGJAMMER_SOURCES})

add_executable(logjammer ${LOGJAMMER_TARGET_SOURCES})
add_dependencies(logjammer ajantv2)
target_include_directories(logjammer PUBLIC ${LOGJAMMER_TARGET_INCLUDE_DIRS})
target_link_libraries(logjammer PUBLIC ajantv2 ${EXTRA_LIBRARIES})

if (AJA_INSTALL_SOURCES)
	install(FILES ${LOGJAMMER_SOURCES} DESTINATION ${AJA_INSTALL_INCLUDEDIR}/ajaapps/crossplatform/logjammer)
endif()
