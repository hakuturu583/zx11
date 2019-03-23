cmake_minimum_required(VERSION 2.8.12)
project(zx11)

find_package(X11 REQUIRED)

add_definitions("-Wall -g -O3")

include_directories(include)

option(USE_XEXT "USE_XEXT" ON)
if(USE_XEXT)
    add_definitions(-D__ZX11_USE_XEXT)
endif()

option(USE_XPM "USE_XPM" ON)
if(USE_XPM)
    add_definitions(-D__ZX11_USE_XPM)
endif()

option(USE_PNG "USE_PNG" ON)
if(USE_PNG)
    add_definitions(-D__ZX11_USE_PNG)
endif()

option(USE_JPEG "USE_JPEG" ON)
if(USE_JPEG)
    add_definitions(-D__ZX11_USE_JPEG)
endif()

option(USE_DIB "USE_DIB" ON)
if(USE_DIB)
    add_definitions(-D__ZX11_USE_BMP)
endif()

option(USE_MAG "USE_MAG" ON)
if(USE_MAG)
    add_definitions(-D__ZX11_USE_MAG)
endif()

option(USE_PDT "USE_PDT" ON)
if(USE_PDT)
    add_definitions(-D__ZX11_USE_PDT)
endif()

set(${PROJECT_NAME}_SRC
    src/zxutil.c
    src/zxfont.c
    src/zxinput.c
    src/zxpixel.c
    src/zximage.c
    src/zximage_pnm.c)

option(USE_WIDGET "USE_WIDGET" ON)
if(USE_WIDGET)
    set(${PROJECT_NAME}_SRC
        ${${PROJECT_NAME}_SRC}
        src/zxwidget.c
    )
endif()

add_library(${PROJECT_NAME} SHARED
    ${${PROJECT_NAME}_SRC})
target_link_libraries(${PROJECT_NAME} ${X11_LIBRARIES})

install(TARGETS ${PROJECT_NAME} LIBRARY DESTINATION lib)

install(TARGETS ${PROJECT_NAME}
    EXPORT ${PROJECT_NAME}-export
    LIBRARY        DESTINATION lib
    INCLUDES       DESTINATION include
    PUBLIC_HEADER  DESTINATION include/${PROJECT_NAME})

install(EXPORT ${PROJECT_NAME}-export
    FILE ${PROJECT_NAME}-config.cmake
    DESTINATION share/cmake/${PROJECT_NAME}
    EXPORT_LINK_INTERFACE_LIBRARIES)