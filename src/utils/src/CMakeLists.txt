SET(DICE_UTILS_SOURCES
  DICe_ImageIO.cpp
  DICe_Rawi.cpp
  ../../cine/DICe_Cine.cpp
)
IF(DICE_ENABLE_NETCDF)
SET(DICE_UTILS_SOURCES
  ${DICE_UTILS_SOURCES}
  ../../netcdf/DICe_NetCDF.cpp
)
ENDIF()

SET(DICE_UTILS_HEADERS
  DICe_ImageIO.h
  DICe_Rawi.h
  ../../cine/DICe_Cine.h
)

IF(DICE_ENABLE_NETCDF)
SET(DICE_UTILS_HEADERS
  ${DICE_UTILS_HEADERS}
  ../../netcdf/DICe_NetCDF.h
)
ENDIF()

include_directories(
    .
    ../../core
    ../../base
    ../../cine
    ../../netcdf
    ${DICE_HEADER_DIRS}
    ${Boost_INCLUDE_DIRS}
    ${DICE_TRILINOS_HEADERS}
    ${TIFF_INCLUDE_DIR}
    ${NetCDF_DIR}
)

SET(DICE_UTILS_LIBRARIES tiff teuchoscore)
if(DICE_HAS_JPEG)
  SET(DICE_UTILS_LIBRARIES ${DICE_UTILS_LIBRARIES} jpeg)
endif()
if(DICE_HAS_PNG)
  SET(DICE_UTILS_LIBRARIES ${DICE_UTILS_LIBRARIES} png)
endif()
if(DICE_ENABLE_NETCDF)
  SET(DICE_UTILS_LIBRARIES ${DICE_UTILS_LIBRARIES} netcdf)
ENDIF()

# Specify target & source files to compile it from
add_library(
    diceutils
    ${DICE_UTILS_SOURCES}
    ${DICE_UTILS_HEADERS}
)

target_link_libraries(diceutils ${DICE_UTILS_LIBRARIES} ${Boost_LIBRARIES})
target_compile_definitions(diceutils PRIVATE -D DICE_LIB_EXPORTS_MODE)
MESSAGE(STATUS "diceutils library will be output to location: ${DICE_LIB_INSTALL_PREFIX}")
set_target_properties(diceutils PROPERTIES
  LIBRARY_OUTPUT_DIRECTORY "${DICE_OUTPUT_PREFIX}/lib"
  ARCHIVE_OUTPUT_DIRECTORY "${DICE_OUTPUT_PREFIX}/lib"
  RUNTIME_OUTPUT_DIRECTORY "${DICE_OUTPUT_PREFIX}/bin"
)

install(FILES ${DICE_UTILS_HEADERS}
  DESTINATION ${CMAKE_INSTALL_PREFIX}/include
)

install(TARGETS diceutils
  DESTINATION ${CMAKE_INSTALL_PREFIX}/lib
)


