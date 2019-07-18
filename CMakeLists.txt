cmake_minimum_required(VERSION 3.3)
LIST(APPEND CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/tools/cmake/")

project(libcxml)


set(forceCpp11 true)


#set(Boost_USE_STATIC_LIBS        ON) # only find static libs
set(Boost_USE_MULTITHREADED      ON)
#set(Boost_USE_STATIC_RUNTIME    OFF)

find_package(Boost REQUIRED COMPONENTS filesystem unit_test_framework)
find_package(LibXml++ REQUIRED)

if (forceCpp11)
    set(OPTIONS ${OPTIONS} --force-cpp11)
endif()


set(PKG_CONFIG_PATH_LIST
        /usr/local/opt/libxml2/lib/pkgconfig
        /usr/local/opt/icu4c/lib/pkgconfig
        /usr/local/opt/pangomm/lib/pkgconfig
        /usr/local/opt/libffi/lib/pkgconfig
        /usr/local/opt/openssl/lib/pkgconfig)



set(PKG_CONFIG_PATH)
foreach(PATH IN ITEMS ${PKG_CONFIG_PATH_LIST})
    if ("${PKG_CONFIG_PATH}" EQUAL "")
        set(PKG_CONFIG_PATH "${PATH}")
    else()
        set(PKG_CONFIG_PATH "${PKG_CONFIG_PATH}:${PATH}")
    endif()
endforeach()


add_custom_target(configure
#        COMMAND "echo" PKG_CONFIG_PATH="${PKG_CONFIG_PATH}"
        COMMAND "${CMAKE_COMMAND}" -E env PKG_CONFIG_PATH="${PKG_CONFIG_PATH}" ./waf configure ${OPTIONS}
        WORKING_DIRECTORY "${CMAKE_SOURCE_DIR}"
        COMMENT "Configuring ${PROJECT_NAME}")

add_custom_target(build
        COMMAND "./waf" build
        WORKING_DIRECTORY "${CMAKE_SOURCE_DIR}"
        COMMENT "Building ${PROJECT_NAME}")

add_custom_target(install_package
        COMMAND "./waf" install
        WORKING_DIRECTORY "${CMAKE_SOURCE_DIR}"
        COMMENT "Installing ${PROJECT_NAME}")

add_custom_target(clean_build
        COMMAND "./waf" clean
        WORKING_DIRECTORY "${CMAKE_SOURCE_DIR}"
        COMMENT "Building ${PROJECT_NAME}")
