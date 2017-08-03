cmake_minimum_required(VERSION 2.8)

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} ${CMAKE_SOURCE_DIR}/cmake/Modules)

project(login_server)
set(login_server_VERSION_MAJOR 1)
set(login_server_VERSION_MINOR 0)

set(login_server_VERSION_STRING "${login_server_MAJOR_VERSION}.${login_server_MINOR_VERSION}.${login_server_PATCH_VERSION}")


option(IPV6 "build application with IPv6 Support" ON) # ON is the default

include_directories(src)

set(LIBSYLVERANT_LIBRARY sylverant)
include_directories(${CMAKE_SOURCE_DIR}/lib/libsylverant/include)

set(LIBPSOARCHIVE_LIBRARY psoarchive)
include_directories(${CMAKE_SOURCE_DIR}/lib/libpsoarchive/include)

set(LIBMINI18N_LIBRARY mini18n)
include_directories(${CMAKE_SOURCE_DIR}/lib/libmini18n/src)


find_package(ZLIB REQUIRED)
include_directories(${ZLIB_INCLUDE_DIR})

find_package(Iconv REQUIRED)
include_directories(${ICONV_INCLUDE_DIR})

find_package(OpenSSL REQUIRED)
include_directories(${OPENSSL_INCLUDE_DIR})

IF(ICONV_SECOND_ARGUMENT_IS_CONST)
  add_definitions (-DICONV_CONST=const)
ELSE()
  add_definitions (-DICONV_CONST=)
ENDIF()

file(GLOB SOURCES src/*.c)

add_definitions (-Wall)

add_definitions (-DVERSION="${login_server_VERSION_STRING}")
add_definitions (-DHAVE_LIBMINI18N)

if(IPV6)
  add_definitions(-DENABLE_IPV6)
endif(IPV6)


add_subdirectory(lib/libsylverant)
add_subdirectory(lib/libpsoarchive)
add_subdirectory(lib/libmini18n)

add_executable(sylverant-login ${SOURCES})
target_link_libraries(sylverant-login ${ZLIB_LIBRARY} ${OPENSSL_CRYPTO_LIBRARY} ${LIBSYLVERANT_LIBRARY} ${LIBPSOARCHIVE_LIBRARY} ${LIBMINI18N_LIBRARY})
#add_dependencies(sylverant-login mini18n psoarchive sylverant)

if(${CMAKE_SYSTEM_NAME} STREQUAL "Linux")
    install(TARGETS sylverant-login RUNTIME DESTINATION bin)
    install(DIRECTORY l10n DESTINATION share/sylverant)
endif()