cmake_minimum_required(VERSION 3.15)
project(vain)

set(CMAKE_CXX_STANDARD 23)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

add_subdirectory(engine)
add_subdirectory(game)

add_executable(vain ${ENGINE_SOURCES})
target_include_directories(vain PRIVATE ${CMAKE_CURRENT_SOURCE_DIR})


if(EMSCRIPTEN)
    set_target_properties(vain PROPERTIES SUFFIX ".js")

    target_link_options(app PRIVATE
        "-sUSE_WEBGPU=1"
    )


    target_compile_options(vain INTERFACE
      $<$<CONFIG:Release>:-O3 -flto>
      $<$<CONFIG:MinSizeRel>:-Oz -flto>
      -fno-exceptions
      -fno-rtti
      -msimd128
    )

    target_link_options(vain INTERFACE
      $<$<CONFIG:Release>:-O3 -flto>
      $<$<CONFIG:MinSizeRel>:-Oz -flto>
      -sALLOW_MEMORY_GROWTH=1
      -sMALLOC=emmalloc
      -sFILESYSTEM=0
      -sENVIRONMENT=web
      -sSTRICT=1
      -sMODULARIZE=1
      -sEXPORT_ES6=1
      "-sEXPORTED_FUNCTIONS=['_main','_my_func']"
      "-sEXPORTED_RUNTIME_METHODS=['cwrap']"
    )

endif()
