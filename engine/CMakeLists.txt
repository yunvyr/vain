cmake_minimum_required(VERSION 3.23)

file(
  GLOB_RECURSE
  ENGINE_SOURCES
  CONFIGURE_DEPENDS
  "${CMAKE_CURRENT_SOURCE_DIR}/*.cpp"
)

add_library(engine STATIC ${ENGINE_SOURCES})


target_sources(engine
    PUBLIC
        FILE_SET cxx_headers
        TYPE HEADERS
        BASE_DIRS ${CMAKE_SOURCE_DIR}
        FILES
            Engine.hpp
)
