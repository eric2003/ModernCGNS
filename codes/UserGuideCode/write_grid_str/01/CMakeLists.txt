cmake_minimum_required ( VERSION 3.20 )

project ( testprj )

if (NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE Release)
endif()
message(STATUS "Build directory: ${PROJECT_BINARY_DIR}")
message(STATUS "Build type: ${CMAKE_BUILD_TYPE}")

set ( PRJ_COMPILE_FEATURES )
set ( PRJ_COMPILE_DEFINITIONS )
set ( PRJ_LIBRARIES )
set ( PRJ_INCLUDE_DIRS )

list ( APPEND PRJ_COMPILE_FEATURES cxx_std_20 )

set ( CGNS_INCLUDE_DIRS $ENV{CGNS_INC} )
set ( CGNS_LIBRARIES $ENV{CGNS_LIB_SHARED_NAME} )

if ( ${CMAKE_SYSTEM_NAME} MATCHES "Windows" )
    set ( CGNS_INCLUDE_DIRS "C:/dev/cgns/include" CACHE PATH "path to CGNS headers" )
    set ( CGNS_LIBRARIES "C:/dev/cgns/lib/cgnsdll.lib" CACHE PATH "path to CGNS library" )
endif()

#message(STATUS "CGNS_INCLUDE_DIRS = ${CGNS_INCLUDE_DIRS}")

list ( APPEND PRJ_LIBRARIES ${CGNS_LIBRARIES} )
list ( APPEND PRJ_INCLUDE_DIRS ${CGNS_INCLUDE_DIRS} )
list ( APPEND PRJ_COMPILE_DEFINITIONS PRJ_ENABLE_CGNS )

message ( STATUS "MSVC=${MSVC}" )
if ( MSVC )
    set_property( DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR} PROPERTY VS_STARTUP_PROJECT ${PROJECT_NAME} )
endif()

add_executable ( ${PROJECT_NAME}
    main.cpp
)

target_include_directories ( ${PROJECT_NAME}
    PRIVATE
        ${PRJ_INCLUDE_DIRS}
)

target_link_libraries( ${PROJECT_NAME} 
    PRIVATE 
        ${PRJ_LIBRARIES}
)

target_compile_features ( ${PROJECT_NAME} 
    PRIVATE 
        ${PRJ_COMPILE_FEATURES}
)

target_compile_definitions ( ${PROJECT_NAME}
    PRIVATE
       ${PRJ_COMPILE_DEFINITIONS} 
)
