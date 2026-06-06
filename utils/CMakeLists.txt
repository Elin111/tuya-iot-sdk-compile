# Include filepaths for source and include.
file( GLOB UTILS_SOURCES "${CMAKE_CURRENT_LIST_DIR}/*.c" )
file( GLOB UTILS_PUBLIC_HEADERS "${CMAKE_CURRENT_LIST_DIR}/*.h" )

# Add a library with the above sources
add_library( utils_modules
    ${UTILS_SOURCES}
)

set_target_properties( utils_modules PROPERTIES PUBLIC_HEADER "${UTILS_PUBLIC_HEADERS}" )

target_include_directories( utils_modules
    PUBLIC 
    ${CMAKE_CURRENT_LIST_DIR}
    ${INTERFACE_DIRS}
)

install( TARGETS utils_modules
         LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
         ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
         RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
         PUBLIC_HEADER DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/tuya-iot-core-sdk/utils )