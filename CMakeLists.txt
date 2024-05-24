cmake_minimum_required(VERSION 3.10)

set(CMAKE_INSTALL_PREFIX "/usr")

project(droidian-fpd)

set(CMAKE_INCLUDE_CURRENT_DIR ON)
set(CMAKE_AUTOMOC ON)

find_package(Qt5 COMPONENTS Core DBus Quick REQUIRED)

set(COMMON_SOURCES src/fpdinterface.cpp)
set(COMMON_HEADERS src/fpdinterface.h)

add_executable(droidian-fpd-client ${COMMON_SOURCES} src/fpdclient.cpp)
target_link_libraries(droidian-fpd-client Qt5::Core Qt5::DBus)
target_include_directories(droidian-fpd-client PRIVATE src)

set(ARCH_TRIPLET "")
execute_process(
    COMMAND bash -c "awk 'BEGIN { FS = \"[ ()-]\"; \"bash --version\" | getline; OFS=\"-\"; if ( /bash/ ) print $9,$11,$12 }'"
    OUTPUT_VARIABLE ARCH_TRIPLET
    OUTPUT_STRIP_TRAILING_WHITESPACE
)

install(TARGETS droidian-fpd-client fpdlistener
        RUNTIME DESTINATION bin
        LIBRARY DESTINATION lib
        PERMISSIONS OWNER_EXECUTE OWNER_WRITE OWNER_READ GROUP_EXECUTE GROUP_READ WORLD_EXECUTE WORLD_READ)

install(FILES droidian-fpd-client.conf DESTINATION share/droidian-fpd-client
        PERMISSIONS OWNER_WRITE OWNER_READ GROUP_READ WORLD_READ)
