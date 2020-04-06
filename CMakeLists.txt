cmake_minimum_required(VERSION 3.1.0)

project(SingleApplication)

set(CMAKE_INCLUDE_CURRENT_DIR ON)
set(CMAKE_AUTOMOC ON)

# Libary target
add_library(${PROJECT_NAME} STATIC
    singleapplication.cpp
    singleapplication_p.cpp
    )

# SingleApplication base class
target_compile_definitions(${PROJECT_NAME} PUBLIC QAPPLICATION_CLASS=${QAPPLICATION_CLASS})

# Find dependencies
find_package(Qt5Network)
if(QAPPLICATION_CLASS STREQUAL QApplication)
    find_package(Qt5 COMPONENTS Widgets REQUIRED)
elseif(QAPPLICATION_CLASS STREQUAL QGuiApplication)
    find_package(Qt5 COMPONENTS Gui REQUIRED)
else()
    find_package(Qt5 COMPONENTS Core REQUIRED)
endif()

# Link dependencies
target_link_libraries(${PROJECT_NAME} PRIVATE Qt5::Network)
if(QAPPLICATION_CLASS STREQUAL QApplication)
    target_link_libraries(${PROJECT_NAME} PRIVATE Qt5::Widgets)
elseif(QAPPLICATION_CLASS STREQUAL QGuiApplication)
    target_link_libraries(${PROJECT_NAME} PRIVATE Qt5::Gui)
else()
    target_link_libraries(${PROJECT_NAME} PRIVATE Qt5::Core)
endif()

if(WIN32)
    target_link_libraries(${PROJECT_NAME} PRIVATE advapi32)
endif()

target_include_directories(${PROJECT_NAME} PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})
