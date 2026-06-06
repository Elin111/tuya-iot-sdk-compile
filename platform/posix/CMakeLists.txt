include( ${LIBRARIES_DIR}/mbedtlsFilePaths.cmake )

# Create target for POSIX implementation of reconnect logic.
add_library( platform_port
               "mbedtls_sockets_wrapper.c"
               "network_wrapper.c"
               "system_wrapper.c"
               "storage_wrapper.c"
               ${MBEDTLS_SOURCE}
               )

target_include_directories( platform_port
                              PUBLIC
                                ${MBEDTLS_INCLUDE_PUBLIC_DIRS}
                                ${INTERFACE_DIRS} )

target_link_libraries( platform_port utils_modules)

install( TARGETS platform_port
         LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
         ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
         RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
         PUBLIC_HEADER DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/tuya-iot-core-sdk/platform )
