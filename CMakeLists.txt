cmake_minimum_required(VERSION 3.23)
project(vain)

set(CMAKE_CXX_STANDARD 23)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)



add_subdirectory(engine)
add_subdirectory(game)


foreach(cfg IN ITEMS DEBUG RELEASE RELWITHDEBINFO MINSIZEREL)
  set_target_properties(
    vain
    PROPERTIES
    RUNTIME_OUTPUT_DIRECTORY_${cfg} "${CMAKE_SOURCE_DIR}/bin"
  )
endforeach()



if(EMSCRIPTEN)
    set_target_properties(vain PROPERTIES SUFFIX ".js")

    add_compile_options(
      $<$<CONFIG:Release>:-O3 -flto>
      $<$<CONFIG:MinSizeRel>:-Oz -flto>
      -fno-exceptions
      -fno-rtti
      -msimd128
    )

    add_link_options(
      $<$<CONFIG:Release>:-O3 -flto>
      $<$<CONFIG:MinSizeRel>:-Oz -flto>
      -sALLOW_MEMORY_GROWTH=1
      -sMALLOC=emmalloc
      -sFILESYSTEM=0
      -sENVIRONMENT=web
      -sSTRICT=1
      -sMODULARIZE=1
      -sEXPORT_ES6=1
      -sUSE_WEBGPU=1
      "-sEXPORTED_FUNCTIONS=['_main','_my_func']"
      "-sEXPORTED_RUNTIME_METHODS=['cwrap']"
    )

endif()
