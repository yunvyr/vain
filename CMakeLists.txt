cmake_minimum_required(VERSION 3.23)
project(vain)

set(CMAKE_CXX_STANDARD 23)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

if(EMSCRIPTEN)
    add_compile_options(
      -fno-exceptions
      -fno-rtti
      -msimd128
      --use-port=emdawnwebgpu
    )

    add_link_options(
      -sALLOW_MEMORY_GROWTH=1
      -sMALLOC=emmalloc
      -sFILESYSTEM=0
      -sENVIRONMENT=web
      -sMODULARIZE=1
      -sEXPORT_ES6=1
      --use-port=emdawnwebgpu
    )
    include_directories(
        vain
        SYSTEM
        PRIVATE
       "${EMSCRIPTEN_SYSROOT}/../ports/emdawnwebgpu/emdawnwebgpu_pkg/webgpu/include"
       )
endif()



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
endif()
