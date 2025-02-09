# Include every source file in source directory
file(GLOB_RECURSE SOURCES RELATIVE ${CMAKE_CURRENT_LIST_DIR} *.cpp *.hpp)
source_group(TREE ${CMAKE_CURRENT_LIST_DIR} PREFIX "Sources" FILES ${SOURCES})

add_library(rdb SHARED ${SOURCES})

target_link_libraries(rdb PRIVATE
    gmod::common
    gmod::helpers_extended
    gmod::lua_shared
    sourcesdk::common
    sourcesdk::tier0
    sourcesdk::tier1
)

if(WIN32)
    target_link_libraries(rdb PRIVATE ws2_32)
endif()

target_include_directories(rdb PRIVATE 
    ${CMAKE_CURRENT_LIST_DIR}
    ${CMAKE_BINARY_DIR}/include
    ${CMAKE_CURRENT_LIST_DIR}/../LRDB/include
    ${CMAKE_CURRENT_LIST_DIR}/../asio/asio/include
    ${CMAKE_CURRENT_LIST_DIR}/../picojson
)

target_compile_definitions(rdb PRIVATE -D_WIN32_WINNT=0x0601)

set_gmod_suffix_prefix(rdb)

# Autoinstall
set(AUTOINSTALL "" CACHE PATH "Autoinstall path")
if(IS_DIRECTORY ${AUTOINSTALL})
    autoinstall(rdb ${AUTOINSTALL})
endif()
