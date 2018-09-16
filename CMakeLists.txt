cmake_minimum_required (VERSION 3.2)
set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

project(sqlite_orm)

option(SqliteOrm_BuildTests "Build sqlite_orm unit tests" ON)
option(SqliteOrm_BuildExamples "Build sqlite_orm examples" ON)

set(SqliteOrm_INCLUDE "${CMAKE_CURRENT_SOURCE_DIR}/include")
add_library(sqlite_orm INTERFACE)
target_include_directories(sqlite_orm
  INTERFACE
    $<BUILD_INTERFACE:${CMAKE_CURRENT_LIST_DIR}/include>
    $<INSTALL_INTERFACE:include>)

# Tests
include(CTest)
if(SqliteOrm_BuildTests AND BUILD_TESTING)
    enable_testing()
    add_subdirectory(tests)
endif()

if(SqliteOrm_BuildExamples)
	add_subdirectory(examples)
endif()


# Create an install target
install(TARGETS sqlite_orm
        EXPORT sqlite_orm-config
        INCLUDES DESTINATION include)

install(EXPORT sqlite_orm-config
        FILE sqlite_orm-config.cmake
        NAMESPACE sqlite_orm::
        DESTINATION share/sqlite_orm/cmake)

install(DIRECTORY include/sqlite_orm
        DESTINATION include FILES_MATCHING PATTERN "*.h")
