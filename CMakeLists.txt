#
# project: tech42
#
cmake_minimum_required(VERSION 3.24)
project(tech42 C)
set(CMAKE_C_STANDARD 17)

# include the fips main cmake file
get_filename_component(FIPS_ROOT_DIR "../fips" ABSOLUTE)
include("${FIPS_ROOT_DIR}/cmake/fips.cmake")

# include_directories(src)
fips_setup()
add_subdirectory(code)
