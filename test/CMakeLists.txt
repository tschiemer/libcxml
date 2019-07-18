
set(SOURCE_FILES
        tests.cc
        ref/a.xml)

find_package(Boost REQUIRED COMPONENTS unit_test_framework)
find_package(LibXml++ REQUIRED)

include_directories(${CMAKE_SOURCE_DIR}/src ${LibXML++_INCLUDE_DIRS} ${Boost_INCLUDE_DIRS})

add_executable(test-libcxml ${SOURCE_FILES})
target_link_libraries(test-libcxml cxml ${Boost_LIBRARIES})
