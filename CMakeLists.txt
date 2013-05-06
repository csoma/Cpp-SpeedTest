# CMake file

cmake_minimum_required(VERSION 2.6.0 FATAL_ERROR)

message("\nCMake started")
project(SpeedTest)

# Debug mode
SET(CMAKE_BUILD_TYPE "Debug")
add_definitions(-DJSON_DEBUG -DAPP_DEBUG)

# Pick static libraries first (if available)
IF(WIN32)
    SET(CMAKE_FIND_LIBRARY_SUFFIXES .lib .a ${CMAKE_FIND_LIBRARY_SUFFIXES})
ELSE(WIN32)
    SET(CMAKE_FIND_LIBRARY_SUFFIXES .a ${CMAKE_FIND_LIBRARY_SUFFIXES})
ENDIF(WIN32)


# Use C++11 features ------------------------------------------
IF (CMAKE_GENERATOR MATCHES Xcode)
	set(CMAKE_XCODE_ATTRIBUTE_GCC_VERSION "com.apple.compilers.llvm.clang.1_0")
	set(CMAKE_XCODE_ATTRIBUTE_CLANG_CXX_LANGUAGE_STANDARD "c++0x")
	set(CMAKE_XCODE_ATTRIBUTE_CLANG_CXX_LIBRARY "libc++")
	set(MY_COMPILE_FLAGS "-std=c++0x -stdlib=libc++ -Wall -Wno-reorder")
ELSE()
	set(MY_COMPILE_FLAGS "-std=c++0x -Wall -Wno-reorder")
ENDIF()


################# Components #######################
message("")


# CppDB ----------------
message("Adding CppDB")
message(STATUS "Documentation: http://cppcms.com/sql/cppdb/")
option(DISABLE_SQLITE "Disable sqlite3 backend" ON)
option(DISABLE_PQ     "Disable postgresql backend" OFF)
option(DISABLE_MYSQL  "Disable mysql backend" ON)
option(DISABLE_ODBC	  "Disable odbc backend into the libcppdb" ON)
option(PQ_BACKEND_INTERNAL 	"Link postgresql backend into the libcppdb" ON)
# Should we use EXCLUDE_FROM_ALL at the end of add_subdirectory?
add_subdirectory("${PROJECT_SOURCE_DIR}/component/CppDB")

find_path(PQ_PATH libpq-fe.h PATH_SUFFIXES postgresql pgsql)
set(CPPDB_INCLUDE_DIR "${PROJECT_SOURCE_DIR}/component/CppDB" ${PQ_PATH})
find_library(PQ_LIB pq)
# pq hast to come after the cppdb!
set(CPPDB_LIBRARIES cppdb-static ${PQ_LIB})
message(STATUS "CppDB libraries: [${CPPDB_LIBRARIES}]")
set(COMPONENT_INC  ${COMPONENT_INC}  ${CPPDB_INCLUDE_DIR})
set(COMPONENT_LIBS ${COMPONENT_LIBS} ${CPPDB_LIBRARIES})
message("CppDB was added\n")


# Poco ----------------
message("Adding Poco")
message(STATUS "Documentation: http://pocoproject.org/docs/")
option(ENABLE_TESTS "Build of Poco tests & samples" OFF)
option(POCO_STATIC  "Poco as STATIC library" ON)
# ON would be great for POCO_UNBUNDLED !
option(POCO_UNBUNDLED "Link Poco dependencies as external libs" OFF)
add_subdirectory("${PROJECT_SOURCE_DIR}/component/Poco")

set(POCO_ALL_LIBRARIES PocoNet PocoUtil)
message(STATUS "Poco libraries: [${POCO_ALL_LIBRARIES}]")
set(COMPONENT_INC  ${COMPONENT_INC}
  "${PROJECT_SOURCE_DIR}/component/Poco/Foundation/include"
  "${PROJECT_SOURCE_DIR}/component/Poco/Net/include"
  "${PROJECT_SOURCE_DIR}/component/Poco/Util/include"
)
message("Poco was added\n")


# Mongoose -------------
message("Adding LibMongoose")
ADD_LIBRARY(libmongoose STATIC
 component/mongoose/mongoose.h
 component/mongoose/mongoose.c
)
set(MONGOOSE_ALL_LIBRARIES  "libmongoose")
message("LibMongoose was added\n")


################# End of Components #######################

# To makes sure the settings are correct
message(STATUS "All include folders: ${ALL_INCLUDE}")
message(STATUS "All libraries: ${ALL_LIBS}")

IF(WIN32)
  SET(ALL_LIBS ws2_32.lib ${COMPONENT_LIBS})
ELSE()
  SET(ALL_LIBS pthread dl ${COMPONENT_LIBS})
ENDIF(WIN32)

# INCLUDE folders
set(ALL_INCLUDE
 ${PROJECT_BINARY_DIR}
 ${PROJECT_SOURCE_DIR}/component
 ${PROJECT_SOURCE_DIR}/src
 ${COMPONENT_INC}
)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${MY_COMPILE_FLAGS}")
set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} ${MY_COMPILE_FLAGS}")
set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS_RELEASE} ${MY_COMPILE_FLAGS}")
set(CMAKE_CXX_FLAGS_RELWITHDEBINFO "${CMAKE_CXX_FLAGS_RELWITHDEBINFO} ${MY_COMPILE_FLAGS}")


# ------------- Poco Test
message("Poco Test")
include_directories(${ALL_INCLUDE})
SET(P1_SOURCES
 "src/poco-test.cpp"
)
IF(WIN32 OR APPLE)
  SET(P1_SOURCES ${P1_SOURCES} ${HEADERS})
ENDIF()
add_executable(Test-Poco WIN32 ${P1_SOURCES} )
target_link_libraries(Test-Poco ${ALL_LIBS} ${POCO_ALL_LIBRARIES})


# ------------- Mongoose Test
message("Mongoose Test")
include_directories(${ALL_INCLUDE})
SET(P2_SOURCES
 "src/mongoose-test.cpp"
)
IF(WIN32 OR APPLE)
  SET(P2_SOURCES ${P2_SOURCES} ${HEADERS})
ENDIF()
add_executable(Test-Mongoose WIN32 ${P2_SOURCES} )
target_link_libraries(Test-Mongoose ${ALL_LIBS} ${MONGOOSE_ALL_LIBRARIES})


# -------------
message("\nCMake Done\n")
