cmake_minimum_required(VERSION 3.0.0)
project(cpp-catch2-demo VERSION 0.1.0)

include(CTest)
enable_testing()

add_executable(cpp-catch2-demo main.cpp)

set(CPACK_PROJECT_NAME ${PROJECT_NAME})
set(CPACK_PROJECT_VERSION ${PROJECT_VERSION})
include(CPack)

find_package(Catch2 CONFIG REQUIRED)
target_link_libraries(cpp-catch2-demo PRIVATE Catch2::Catch2)

add_custom_target(run
  COMMAND ${PROJECT_NAME}
  DEPENDS ${PROJECT_NAME}
  WORKING_DIRECTORY ${CMAKE_PROJECT_DIR}
)