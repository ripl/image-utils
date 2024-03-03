if(${CMAKE_SYSTEM_PROCESSOR} MATCHES arm*)
    add_compile_options(
        -march=armv8-a+fp+simd
        -std=gnu99
        -fPIC)#+crypto+crc)    
elseif(${CMAKE_SYSTEM_PROCESSOR} MATCHES amd64* OR ${CMAKE_SYSTEM_PROCESSOR} MATCHES x86_64*)
    add_compile_options(
        -std=gnu99
        -fPIC
        -msse
        -msse2
        -msse3)
elseif(${CMAKE_SYSTEM_PROCESSOR} MATCHES i686* OR ${CMAKE_SYSTEM_PROCESSOR} MATCHES x86)
    add_compile_options(
        -std=gnu99
        -fPIC
        -msse
        -msse2
        -msse3)
endif ()


#add_definitions(-std=gnu99 -fPIC -msse -msse2 -msse3)

# add_definitions(
#     -std=gnu99
#     -fPIC
#     # -msse
#     # -msse2
#     # -msse3
# )

# add_compile_options(-march=armv8-a+fp+simd)#+crypto+crc)

file(GLOB c_files *.c)
file(GLOB h_files *.h)
add_library(image-utils SHARED ${c_files})

include_directories(${JPEG_INCLUDE_DIR})
target_link_libraries(image-utils ${JPEG_LIBRARIES})

# set the library API version.  Increment this every time the public API
# changes.
set_target_properties(image-utils PROPERTIES SOVERSION 1)

# make the shared library public
pods_install_libraries(image-utils)
#make the header public
pods_install_headers(${h_files} DESTINATION image_utils)

pods_install_pkg_config_file(image-utils
    LIBS -limage-utils 
    REQUIRES 
    VERSION 0.0.1)
