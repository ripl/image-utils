cmake_minimum_required(VERSION 2.6.0)

set(POD_NAME common_image_utils)

# Necessary to build on OSX
find_package(OpenGL REQUIRED)

# This lets us use the pods cmake macros
include(cmake/pods.cmake)


if(${CMAKE_SYSTEM_PROCESSOR} MATCHES amd64* OR ${CMAKE_SYSTEM_PROCESSOR} MATCHES x86_64*)
    include(cmake/sse.cmake)
    if (NOT SSE3_FOUND)
        message("\n--- Image util requires sse instructions to be availabe. ${CMAKE_SYSTEM_PROCESSOR} doesn't have them.")
        message("--- Not building the image-utils\n")
        return()
    endif()
elseif(${CMAKE_SYSTEM_PROCESSOR} MATCHES i686* OR ${CMAKE_SYSTEM_PROCESSOR} MATCHES x86)
    include(cmake/sse.cmake)
    if (NOT SSE3_FOUND)
        message("\n--- Image util requires sse instructions to be availabe. ${CMAKE_SYSTEM_PROCESSOR} doesn't have them.")
        message("--- Not building the image-utils\n")
        return()
    endif()
endif ()

# include(cmake/sse.cmake)
# if (NOT SSE3_FOUND)
#   message("\n--- Image util requires sse instructions to be availabe. ${CMAKE_SYSTEM_PROCESSOR} doesn't have them.")
#   message("--- Not building the image-utils\n")
#   return()
# endif()

find_package (JPEG REQUIRED)

add_subdirectory(src)
add_subdirectory(src/renderer)


# install all python packages in the python dir
pods_install_python_packages(${CMAKE_CURRENT_SOURCE_DIR}/python)

# executable scripts:  script-name  python-module
pods_install_python_script(image-util-extract-jpgs image_utils.extract_jpgs)
