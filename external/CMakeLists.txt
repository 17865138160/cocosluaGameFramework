cmake_minimum_required(VERSION 3.6)
project(external)

option(BUILD_EXT_BOX2D "Build with internal Box2D support" ON)
option(BUILD_EXT_CHIPMUNK "Build with internal chipmunk support" ON)
option(BUILD_EXT_FREETYPE2 "Build with internal freetype2 support" ON)
option(BUILD_EXT_RECAST "Build with internal recast support" ON)
option(BUILD_EXT_BULLET "Build with internal bullet support" ON)
option(BUILD_EXT_JPEG "Build with internal jpeg support" ON)
option(BUILD_EXT_OPENSSL "Build with internal openssl support" ON)
option(BUILD_EXT_WEBP "Build with internal webp support" ON)
option(BUILD_EXT_WEBSOCKETS "Build with internal websockets support" ON)
option(BUILD_EXT_TINYXML2 "Build with internal tinyxml2 support" ON)
option(BUILD_EXT_XXHASH "Build with internal xxhash support" ON)
option(BUILD_EXT_XXTEA "Build with internal xxtea support" ON)
option(BUILD_EXT_CLIPPER "Build with internal clipper support" ON)
option(BUILD_EXT_EDTAA3FUNC "Build with internal edtaa3func support" ON)
option(BUILD_EXT_CONVERTUTF "Build with internal ConvertUTF support" ON)
option(BUILD_EXT_POLY2TRI "Build with internal poly2tri support" ON)
option(BUILD_EXT_MD5 "Build with internal md5 support" ON)
option(BUILD_EXT_ZLIB "Build with internal zlib support" ON)
option(BUILD_EXT_CURL "Build with internal curl support" ON)
option(BUILD_EXT_UNZIP "Build with internal unzip support" ON)

add_library(external empty.cpp)

# OpenAL
add_subdirectory(OpenAL)
target_link_libraries(external ext_OpenAL)
# OGG
add_subdirectory(libvorbis)
target_link_libraries(external ext_libvorbis) # before ogg, make android link happy
add_subdirectory(libogg)
target_link_libraries(external ext_libogg)
# FAAD2
add_subdirectory(faad2)
target_link_libraries(external ext_libfaad)

if(BUILD_EXT_BOX2D)
    add_subdirectory(Box2D)
    target_link_libraries(external ext_box2d)
endif(BUILD_EXT_BOX2D)
if(BUILD_EXT_CHIPMUNK)
    add_subdirectory(chipmunk)
    target_link_libraries(external ext_chipmunk)
endif(BUILD_EXT_CHIPMUNK)
if(BUILD_EXT_FREETYPE2)
    add_subdirectory(freetype2)
    target_link_libraries(external ext_freetype)
endif(BUILD_EXT_FREETYPE2)
if(BUILD_EXT_RECAST)
    add_subdirectory(recast)
    target_link_libraries(external ext_recast)
endif(BUILD_EXT_RECAST)
if(BUILD_EXT_BULLET)
    add_subdirectory(bullet)
    target_link_libraries(external ext_bullet)
endif(BUILD_EXT_BULLET)
if(BUILD_EXT_JPEG)
    add_subdirectory(jpeg)
    target_link_libraries(external ext_jpeg)
endif(BUILD_EXT_JPEG)
if(BUILD_EXT_WEBP)
    add_subdirectory(webp)
    target_link_libraries(external ext_webp)
endif(BUILD_EXT_WEBP)
if(BUILD_EXT_WEBSOCKETS)
    add_subdirectory(websockets)
    target_link_libraries(external ext_websockets)
endif(BUILD_EXT_WEBSOCKETS)
if(BUILD_EXT_TINYXML2)
    add_subdirectory(tinyxml2)
    target_link_libraries(external ext_tinyxml2)
endif(BUILD_EXT_TINYXML2)
if(BUILD_EXT_XXHASH)
    add_subdirectory(xxhash)
    target_link_libraries(external ext_xxhash)
endif(BUILD_EXT_XXHASH)
if(BUILD_EXT_XXTEA)
    add_subdirectory(xxtea)
    target_link_libraries(external ext_xxtea)
endif(BUILD_EXT_XXTEA)
if(BUILD_EXT_CLIPPER)
    add_subdirectory(clipper)
    target_link_libraries(external ext_clipper)
endif(BUILD_EXT_CLIPPER)
if(BUILD_EXT_EDTAA3FUNC)
    add_subdirectory(edtaa3func)
    target_link_libraries(external ext_edtaa3func)
endif(BUILD_EXT_EDTAA3FUNC)
if(BUILD_EXT_CONVERTUTF)
    add_subdirectory(ConvertUTF)
    target_link_libraries(external ext_convertUTF)
endif(BUILD_EXT_CONVERTUTF)
if(BUILD_EXT_POLY2TRI)
    add_subdirectory(poly2tri)
    target_link_libraries(external ext_poly2tri)
endif(BUILD_EXT_POLY2TRI)
if(BUILD_EXT_MD5)
    add_subdirectory(md5)
    target_link_libraries(external ext_md5)
endif(BUILD_EXT_MD5)
if(BUILD_EXT_CURL)
    add_subdirectory(curl)
    target_link_libraries(external ext_curl)
endif(BUILD_EXT_CURL)
if(BUILD_EXT_OPENSSL)
    add_subdirectory(openssl)
    target_link_libraries(external ext_ssl)
    target_link_libraries(external ext_crypto)
endif(BUILD_EXT_OPENSSL)
add_subdirectory(png)
target_link_libraries(external ext_png)

# use lua/js specific libs by property to prevent conflict
if(BUILD_LUA_LIBS)
    add_subdirectory(lua/luajit)
    add_subdirectory(lua/tolua)
    add_subdirectory(lua/cjson)
    add_subdirectory(lua/lfs)
    add_subdirectory(lua/lpack)
    add_subdirectory(lua/lpeg)
    add_subdirectory(lua/lsproto)
    add_subdirectory(lua/lsqlite3)
    add_subdirectory(lua/luasocket)
    add_subdirectory(lua/lunqlite)
    add_subdirectory(lua/lzlib)
    add_subdirectory(lua/protobuf)
    add_subdirectory(lua/lua-protobuf)
    set_property(TARGET external APPEND PROPERTY 
        CC_LUA_DEPEND ext_luajit ext_tolua ext_cjson ext_lfs ext_lpack ext_lpeg ext_lsproto ext_lsqlite3 ext_luasocket ext_lunqlite ext_lzlib ext_protobuf ext_lua-protobuf
    )
endif()

if(ANDROID)
    add_subdirectory(android-specific/cpufeatures)
    target_link_libraries(external ext_cpufeatures)
endif()

if(WINDOWS)
    add_subdirectory(glew)
    target_link_libraries(external ext_glew)
endif()

if(WINDOWS OR MACOSX OR LINUX)
    add_subdirectory(glfw3)
    target_link_libraries(external ext_glfw3)
endif()

if(BUILD_EXT_ZLIB)
    add_subdirectory(zlib)
    target_link_libraries(external ext_zlib)
endif(BUILD_EXT_ZLIB)

# unzip depend on zlib
if(BUILD_EXT_UNZIP)
    add_subdirectory(unzip)
    target_link_libraries(external ext_unzip)
endif(BUILD_EXT_UNZIP)

# put "external" into External folder, too
set_target_properties(external
    PROPERTIES
    ARCHIVE_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/lib"
    LIBRARY_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/lib"
    FOLDER "External"
)

if(MACOSX OR IOS)
    add_subdirectory(glsl-optimizer)
    target_link_libraries(external 
    ext_glcpp_library
    ext_glsl_optimizer
    ext_libmesa
    )
endif()
