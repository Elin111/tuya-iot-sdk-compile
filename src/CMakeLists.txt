# Include filepaths for source and include.
include( ${LIBRARIES_DIR}/coreJSON/jsonFilePaths.cmake )
include( ${LINKSDK_DIRS}/tuyaFilePaths.cmake )

# Add a library with the above sources
add_library( link_core
    ${LINK_SDK_SOURCES}
    ${JSON_SOURCES}
)

file( GLOB LINK_CORE_PUBLIC_HEADERS
      "${ROOT_DIR}/include/*.h"
      "${ROOT_DIR}/interface/*.h" )
set_target_properties( link_core PROPERTIES PUBLIC_HEADER "${LINK_CORE_PUBLIC_HEADERS}" )

target_include_directories( link_core
    PUBLIC 
    ${LINK_SDK_INCLUDE_PUBLIC_DIRS}
    ${INTERFACE_DIRS}
    ${JSON_INCLUDE_PUBLIC_DIRS}
    ${CMAKE_CURRENT_LIST_DIR}
)

target_link_libraries( link_core
    utils_modules
    platform_port
    middleware_implementation
)

install( TARGETS link_core
         LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
         ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
         RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
         PUBLIC_HEADER DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/tuya-iot-core-sdk )

# # Output
# file(GLOB OUTPUT_INCLUDE ${LINK_SDK_INCLUDE_PUBLIC_DIRS}/*.h)
# file(GLOB OUTPUT_INCLUDE ${OUTPUT_INCLUDE} ${INTERFACE_DIRS}/*.h)
# file(GLOB OUTPUT_INTERNAL_INC ${JSON_INCLUDE_PUBLIC_DIRS}/*.h)

# file(COPY ${LINK_SDK_SOURCES} DESTINATION ${PROJECT_SOURCE_DIR}/output/tuya_linksdk_c/src)
# file(COPY ${JSON_SOURCES} DESTINATION ${PROJECT_SOURCE_DIR}/output/tuya_linksdk_c/src)

# file(COPY ${OUTPUT_INCLUDE} DESTINATION ${PROJECT_SOURCE_DIR}/output/tuya_linksdk_c/include)
# file(COPY ${OUTPUT_INTERNAL_INC} DESTINATION ${PROJECT_SOURCE_DIR}/output/tuya_linksdk_c/src/internal_inc)