project(testp2p)

message("added testp2p")

set(TESTP2P_TARGET_INCLUDE_DIRS
	${CMAKE_CURRENT_SOURCE_DIR}/../
	${AJA_LIBRARIES_ROOT}
	${AJA_LIB_NTV2_ROOT}/includes)

set(TESTP2P_SOURCES
    testp2p.cpp)

if (CMAKE_SYSTEM_NAME STREQUAL "Windows")
	# noop
elseif (CMAKE_SYSTEM_NAME STREQUAL "Darwin")
	find_library(FOUNDATION_FRAMEWORK Foundation)
	set(EXTRA_LIBRARIES ${FOUNDATION_FRAMEWORK})
elseif (CMAKE_SYSTEM_NAME STREQUAL "Linux")
	set(EXTRA_LIBRARIES dl pthread rt)
endif()

set(TESTP2P_TARGET_SOURCES
	${TESTP2P_SOURCES})

add_executable(testp2p ${TESTP2P_TARGET_SOURCES})
add_dependencies(testp2p ajantv2)
target_include_directories(testp2p PUBLIC ${TESTP2P_TARGET_INCLUDE_DIRS})
target_link_libraries(testp2p PUBLIC ajantv2 ${EXTRA_LIBRARIES})

if (AJA_INSTALL_SOURCES)
	install(FILES ${TESTP2P_SOURCES} DESTINATION ${AJA_INSTALL_INCLUDEDIR}/ajaapps/crossplatform/testp2p)
endif()
