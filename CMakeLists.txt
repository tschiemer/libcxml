cmake_minimum_required(VERSION 3.3)
LIST(APPEND CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/tools/cmake/")

project(libcxml)


set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")


#set(Boost_USE_STATIC_LIBS        ON) # only find static libs
set(Boost_USE_MULTITHREADED      ON)
#set(Boost_USE_STATIC_RUNTIME    OFF)


set(CMAKE_INSTALL_PREFIX "${CMAKE_SOURCE_DIR}/lib")


add_subdirectory(src)
add_subdirectory(test)


set(prefix ${CMAKE_INSTALL_PREFIX})
set(includedir \${prefix}/include)
set(libdir \${prefix}/lib)
set(version 0.0.0)
set(libs "-L\${libdir} -lcxmp")
configure_file("libcxml.pc.in" "libcxml.pc" @ONLY)
install(FILES ${CMAKE_BINARY_DIR}/libcxml.pc DESTINATION lib/pkgconfig)


add_custom_target(install--${PROJECT_NAME}
        "${CMAKE_COMMAND}" --build "${CMAKE_BINARY_DIR}" --target install
        DEPENDS cxml
        COMMENT "Installing ${PROJECT_NAME}")
