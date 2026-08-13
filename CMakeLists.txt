cmake_minimum_required(VERSION 3.20)

project(ScriptHookV_GTA5 LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

add_library(ScriptHookV SHARED
    src/scripthook.cpp
)

target_include_directories(ScriptHookV PRIVATE
    ${CMAKE_CURRENT_SOURCE_DIR}/src
)

if(MSVC)
    target_compile_options(ScriptHookV PRIVATE
        /W3
        /EHsc
    )

    target_compile_definitions(ScriptHookV PRIVATE
        WIN32_LEAN_AND_MEAN
        NOMINMAX
    )

    # Release-style linker configuration
    target_link_options(ScriptHookV PRIVATE
        $<$<CONFIG:Release>:/OPT:REF>
        $<$<CONFIG:Release>:/OPT:ICF>
        $<$<CONFIG:Release>:/INCREMENTAL:NO>
    )
endif()

set_target_properties(ScriptHookV PROPERTIES
    OUTPUT_NAME "ScriptHookV"
    RUNTIME_OUTPUT_DIRECTORY "${CMAKE_SOURCE_DIR}/out"
)

foreach(CONFIG Debug Release RelWithDebInfo MinSizeRel)
    string(TOUPPER "${CONFIG}" CONFIG_UPPER)

    set_target_properties(ScriptHookV PROPERTIES
        RUNTIME_OUTPUT_DIRECTORY_${CONFIG_UPPER}
            "${CMAKE_SOURCE_DIR}/out"
    )
endforeach()