cmake_minimum_required(VERSION 3.3)
project(libcxml)


#### Configuration

set(targetWindows false)
set(enableDebug false)
set(static false)
set(disableTests false)
set(forceCpp11 true) #note libxml++ requires C++11


#### /Configuration



#### Platform dependent configuration

set(PKG_CONFIG_PATH_LIST
        /usr/local/opt/libxml2/lib/pkgconfig
        /usr/local/opt/icu4c/lib/pkgconfig
        /usr/local/opt/pangomm/lib/pkgconfig
        /usr/local/opt/libffi/lib/pkgconfig
        /usr/local/opt/openssl/lib/pkgconfig)

#### /Platform dependent configuration


if (targetWindows)
    set(OPTIONS ${OPTIONS} --target-windows)
endif()
if (enableDebug)
    set(OPTIONS ${OPTIONS} --enabled-debug)
endif()
if (static)
    set(OPTIONS ${OPTIONS} --static)
endif()
if (disableTests)
    set(OPTIONS ${OPTIONS} --disable-tests)
endif()
if (forceCpp11)
    set(OPTIONS ${OPTIONS} --force-cpp11)
endif()


set(PKG_CONFIG_PATH)
foreach(PATH IN ITEMS ${PKG_CONFIG_PATH_LIST})
    if ("${PKG_CONFIG_PATH}" EQUAL "")
        set(PKG_CONFIG_PATH "${PATH}")
    else()
        set(PKG_CONFIG_PATH "${PKG_CONFIG_PATH}:${PATH}")
    endif()
endforeach()


add_custom_target(waf-configure
        COMMAND "${CMAKE_COMMAND}" -E env PKG_CONFIG_PATH="${PKG_CONFIG_PATH}" ./waf configure ${OPTIONS}
        WORKING_DIRECTORY "${CMAKE_SOURCE_DIR}"
        COMMENT "Configuring ${PROJECT_NAME}")

add_custom_target(waf-build
        COMMAND "./waf" build
        WORKING_DIRECTORY "${CMAKE_SOURCE_DIR}"
        COMMENT "Building ${PROJECT_NAME}")

add_custom_target(waf-install
        COMMAND "./waf" install
        WORKING_DIRECTORY "${CMAKE_SOURCE_DIR}"
        COMMENT "Installing ${PROJECT_NAME}")

add_custom_target(waf-clean
        COMMAND "./waf" clean
        WORKING_DIRECTORY "${CMAKE_SOURCE_DIR}"
        COMMENT "Building ${PROJECT_NAME}")
