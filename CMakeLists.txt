cmake_minimum_required(VERSION 3.29.6)
project(SoLoud)

enable_language(C CXX)

### IMPORTANT!!!

## SoLoud + MinGW fix:
# Changed line 38 in soloud_openal_dll.c to:
# #if defined(_MSC_VER) || defined(USING_MINGW)
# Then pass macro USING_MINGW when compiling SoLoud.

## Soloud sourcecode fix to load .dylib files:
# Line 121 change code to:
#static HMODULE oal_openDll()
#{
##if defined(__APPLE__)
#return dlopen("libopenal.1.dylib", RTLD_LAZY);
##else
#return dlopen("libopenal.so", RTLD_LAZY);
##endif
#}

# Enable the OpenAL backend and disable dynamic loading
add_definitions(-DWITH_OPENAL)

if (CMAKE_BUILD_TYPE STREQUAL "Debug")
    message(STATUS "Building ${PROJECT_NAME} in ${CMAKE_BUILD_TYPE} mode with flags: ${CMAKE_CXX_FLAGS_DEBUG}")
elseif (CMAKE_BUILD_TYPE STREQUAL "Release")
    message(STATUS "Building ${PROJECT_NAME} in ${CMAKE_BUILD_TYPE} mode with flags: ${CMAKE_CXX_FLAGS_RELEASE}")
endif ()

# SoLoud source files
set(SOLOUD_SOURCES
        src/core/soloud.cpp
        src/core/soloud_audiosource.cpp
        src/core/soloud_bus.cpp
        src/core/soloud_core_3d.cpp
        src/core/soloud_core_basicops.cpp
        src/core/soloud_core_faderops.cpp
        src/core/soloud_core_filterops.cpp
        src/core/soloud_core_getters.cpp
        src/core/soloud_core_setters.cpp
        src/core/soloud_core_voicegroup.cpp
        src/core/soloud_core_voiceops.cpp
        src/core/soloud_fader.cpp
        src/core/soloud_fft.cpp
        src/core/soloud_fft_lut.cpp
        src/core/soloud_file.cpp
        src/core/soloud_filter.cpp
        src/core/soloud_misc.cpp
        src/core/soloud_queue.cpp
        src/core/soloud_thread.cpp
        src/audiosource/wav/dr_impl.cpp
        src/audiosource/wav/soloud_wav.cpp
        src/audiosource/wav/soloud_wavstream.cpp
        src/audiosource/wav/stb_vorbis.c
        src/c_api/soloud_c.cpp
        src/backend/openal/soloud_openal.cpp
        src/backend/openal/soloud_openal_dll.c
)

# Create a static library target for SoLoud
add_library(SoLoud STATIC
        ${SOLOUD_SOURCES}
)


# Include directories for SoLoud and OpenAL
target_include_directories(SoLoud PUBLIC
        include
        ${OPENAL_INCLUDE_DIR}  # Include OpenAL headers from include dir
)

# Link SoLoud with the external OpenAL Soft dynamic library
target_link_libraries(SoLoud PUBLIC
        ${OPENAL_LIBRARY}
)

message(STATUS "Building ${PROJECT_NAME} complete")
