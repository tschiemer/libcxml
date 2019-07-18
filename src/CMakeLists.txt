
set(SOURCE_FILES
        cxml.cc
        cxml.h)


find_package(Boost REQUIRED COMPONENTS filesystem)
find_package(LibXml++ REQUIRED)


include_directories(${LibXML++_INCLUDE_DIRS} ${Boost_INCLUDE_DIRS})

add_library(cxml ${SOURCE_FILES})
target_link_libraries(cxml ${LibXML++_LIBRARIES} ${Boost_LIBRARIES})

set(LIBS ${LIBS} cxml)

install(TARGETS cxml LIBRARY DESTINATION lib ARCHIVE DESTINATION lib)
install(FILES cxml.h DESTINATION include)
