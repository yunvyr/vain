cmake_minimum_required(VERSION 3.23)

file(
  GLOB_RECURSE
  VAIN_SOURCES
  CONFIGURE_DEPENDS
  "${CMAKE_CURRENT_SOURCE_DIR}/*.cpp"
)

add_executable(vain ${VAIN_SOURCES})

target_link_libraries(vain PRIVATE engine)
