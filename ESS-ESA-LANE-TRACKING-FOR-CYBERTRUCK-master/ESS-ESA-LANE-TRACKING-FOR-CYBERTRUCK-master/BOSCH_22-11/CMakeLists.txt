cmake_minimum_required(VERSION 3.16)

project(LaneKeepingMPC LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS ON)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

find_package(Threads REQUIRED)
find_package(OpenCV REQUIRED)
find_package(Eigen3 REQUIRED NO_MODULE)
find_package(OsqpEigen REQUIRED)
find_package(PkgConfig REQUIRED)
pkg_check_modules(LIBSERIAL REQUIRED IMPORTED_TARGET libserial)

add_executable(lane_detector
    src/communication.cpp
    src/LaneDetector.cpp
    src/logger.cpp
    src/logic.cpp
    src/main.cpp
    src/MpcController.cpp
    src/Trans_UDP.cpp
)

target_include_directories(lane_detector
    PRIVATE
        ${CMAKE_CURRENT_SOURCE_DIR}/include
        ${OpenCV_INCLUDE_DIRS}
)

target_link_libraries(lane_detector
    PRIVATE
        ${OpenCV_LIBS}
        Eigen3::Eigen
        OsqpEigen::OsqpEigen
        PkgConfig::LIBSERIAL
        Threads::Threads
)

if(CMAKE_CXX_COMPILER_ID MATCHES "GNU|Clang")
    target_compile_options(lane_detector PRIVATE -Wall -Wextra)
endif()

set_target_properties(lane_detector PROPERTIES
    RUNTIME_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/bin"
)