find_package(PkgConfig REQUIRED)
pkg_check_modules(BOT2_VIS bot2-vis)
if(NOT BOT2_VIS_FOUND)
    message("\n\n")
    message("bot2-vis not found.  Not building cam thumb renderer")
    message("\n")
    return()
endif()

add_definitions(-std=gnu99)
add_library(image-utils-renderer SHARED
    renderer_cam_thumb.c)

target_link_libraries(image-utils-renderer ${OPENGL_LIBRARIES})

set(REQUIRED_LIBS image-utils bot2-vis bot2-param-client bot2-frames)

pods_use_pkg_config_packages(image-utils-renderer ${REQUIRED_LIBS})

pods_install_headers(renderer_cam_thumb.h DESTINATION image_utils)
pods_install_libraries(image-utils-renderer)
pods_install_pkg_config_file(image-utils-renderer
    CFLAGS
    LIBS -limage-utils-renderer
    REQUIRES ${REQUIRED_LIBS}
    VERSION 0.0.1)