include(GNUInstallDirs)

find_package(ZLIB)
find_package(clipper)
find_package(Qhull 8 REQUIRED)

message(ZLIB_INCLUDE_DIRS = ${ZLIB_INCLUDE_DIRS})
message(ZLIB_LIBRARIES= ${ZLIB_LIBRARIES})
message(clipper_INCLUDE_DIRS = ${clipper_INCLUDE_DIRS})
message(clipper_LIBRARIES= ${clipper_LIBRARIES})
message(Qhull_INCLUDE_DIRS = ${Qhull_INCLUDE_DIRS})
message(Qhull_LIBRARIES= ${Qhull_LIBRARIES})

set(HEADER_LIST 
    "${gdstk_SOURCE_DIR}/include/gdstk/allocator.hpp"
    "${gdstk_SOURCE_DIR}/include/gdstk/array.hpp"
    "${gdstk_SOURCE_DIR}/include/gdstk/cell.hpp"
    "${gdstk_SOURCE_DIR}/include/gdstk/clipper_tools.hpp"
    "${gdstk_SOURCE_DIR}/include/gdstk/curve.hpp"
    "${gdstk_SOURCE_DIR}/include/gdstk/flexpath.hpp"
    "${gdstk_SOURCE_DIR}/include/gdstk/font.hpp"
    "${gdstk_SOURCE_DIR}/include/gdstk/gdsii.hpp"
    "${gdstk_SOURCE_DIR}/include/gdstk/gdstk.hpp"
    "${gdstk_SOURCE_DIR}/include/gdstk/gdswriter.hpp"
    "${gdstk_SOURCE_DIR}/include/gdstk/label.hpp"
    "${gdstk_SOURCE_DIR}/include/gdstk/library.hpp"
    "${gdstk_SOURCE_DIR}/include/gdstk/map.hpp"
    "${gdstk_SOURCE_DIR}/include/gdstk/oasis.hpp"
    "${gdstk_SOURCE_DIR}/include/gdstk/pathcommon.hpp"
    "${gdstk_SOURCE_DIR}/include/gdstk/polygon.hpp"
    "${gdstk_SOURCE_DIR}/include/gdstk/property.hpp"
    "${gdstk_SOURCE_DIR}/include/gdstk/raithdata.hpp"
    "${gdstk_SOURCE_DIR}/include/gdstk/rawcell.hpp"
    "${gdstk_SOURCE_DIR}/include/gdstk/reference.hpp"
    "${gdstk_SOURCE_DIR}/include/gdstk/repetition.hpp"
    "${gdstk_SOURCE_DIR}/include/gdstk/robustpath.hpp"
    "${gdstk_SOURCE_DIR}/include/gdstk/set.hpp"
    "${gdstk_SOURCE_DIR}/include/gdstk/sort.hpp"
    "${gdstk_SOURCE_DIR}/include/gdstk/style.hpp"
    "${gdstk_SOURCE_DIR}/include/gdstk/tagmap.hpp"
    "${gdstk_SOURCE_DIR}/include/gdstk/utils.hpp"
    "${gdstk_SOURCE_DIR}/include/gdstk/vec.hpp")

set(SOURCE_LIST
    cell.cpp
    clipper_tools.cpp
    curve.cpp
    flexpath.cpp
    gdsii.cpp
    label.cpp
    library.cpp
    oasis.cpp
    polygon.cpp
    property.cpp
    raithdata.cpp
    rawcell.cpp
    reference.cpp
    repetition.cpp
    robustpath.cpp
    style.cpp
    utils.cpp)

add_library(gdstk ${SOURCE_LIST} ${HEADER_LIST})

target_compile_features(gdstk PUBLIC cxx_std_11)

set_target_properties(gdstk PROPERTIES POSITION_INDEPENDENT_CODE ON)

set_target_properties(gdstk PROPERTIES PUBLIC_HEADER "${HEADER_LIST}")


target_include_directories(gdstk PUBLIC
    $<BUILD_INTERFACE:${gdstk_SOURCE_DIR}/include>
    $<BUILD_INTERFACE:${gdstk_SOURCE_DIR}/external>
    $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>
    ${clipper_INCLUDE_DIRS}
    ${ZLIB_INCLUDE_DIRS}
    ${Qhull_INCLUDE_DIRS})

target_link_libraries(gdstk
    ${ZLIB_LIBRARIES}
    ${Qhull_LIBRARIES}
    ${clipper_LIBRARIES}
    )

if(UNIX)
    target_link_libraries(gdstk m)
endif(UNIX)

source_group(
    TREE "${PROJECT_SOURCE_DIR}/include"
    PREFIX "Header Files"
    FILES ${HEADER_LIST})

install(TARGETS gdstk
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
    INCLUDES DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/gdstk
    PUBLIC_HEADER DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/gdstk)
