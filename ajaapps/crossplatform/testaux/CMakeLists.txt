project(testaux)

message("added testaux")

set(TESTAUX_TARGET_INCLUDE_DIRS
	${CMAKE_CURRENT_SOURCE_DIR}/../
	${AJA_LIBRARIES_ROOT}
	${AJA_LIB_NTV2_ROOT}/includes)

set(TESTAUX_SOURCES
    testaux.cpp)

if (CMAKE_SYSTEM_NAME STREQUAL "Windows")
	# noop
elseif (CMAKE_SYSTEM_NAME STREQUAL "Darwin")
	find_library(FOUNDATION_FRAMEWORK Foundation)
	set(EXTRA_LIBRARIES ${FOUNDATION_FRAMEWORK})
elseif (CMAKE_SYSTEM_NAME STREQUAL "Linux")
	set(EXTRA_LIBRARIES dl pthread rt)
endif()

set(TESTAUX_TARGET_SOURCES
	${TESTAUX_SOURCES})

add_executable(testaux ${TESTAUX_TARGET_SOURCES})
add_dependencies(testaux ajantv2)
target_include_directories(testaux PUBLIC ${TESTAUX_TARGET_INCLUDE_DIRS})
target_link_libraries(testaux PUBLIC ajantv2 ${EXTRA_LIBRARIES})

if (AJA_INSTALL_SOURCES)
	install(FILES ${TESTAUX_SOURCES} DESTINATION ${AJA_INSTALL_INCLUDEDIR}/ajaapps/crossplatform/testaux)
endif()
