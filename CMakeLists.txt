cmake_policy(SET CMP0042 NEW)
# assimp needs exceptions and RTTO enabled
set(FIPS_EXCEPTIONS ON CACHE BOOL "Enable C++ exceptions" FORCE)
set(FIPS_RTTI ON CACHE BOOL "Enable C++ RTTI" FORCE)
if (NOT FIPS_IMPORT)
    cmake_minimum_required(VERSION 2.8)
    get_filename_component(FIPS_ROOT_DIR "../fips" ABSOLUTE)
    include("${FIPS_ROOT_DIR}/cmake/fips.cmake")
    fips_setup()
    fips_project(fips-assimp)
else()
    # build tools and tests only in standalone mode
    set(ASSIMP_BUILD_ASSIMP_TOOLS OFF CACHE BOOL "ASSIMP tools" FORCE)
    set(ASSIMP_BUILD_TESTS OFF CACHE BOOL "ASSIMP tests" FORCE)
endif()

# build static libs (the assimp CMakeLists.txt file seems to
# infect all other libs and convert them to shared)
set(BUILD_SHARED_LIBS OFF CACHE BOOL "ASSIMP shared libs" FORCE)

add_subdirectory(assimp)

if (NOT FIPS_IMPORT)
    fips_finish()
endif()
