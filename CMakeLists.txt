cmake_minimum_required(VERSION 3.0)
project(YourProjectName)

# Find the Nim-generated files
file(GLOB NIM_GENERATED_FILES "src/*.nim.c")

# Add the Application files and the Nim helpers files
add_application(Dialog dialog.c dialog.r ${NIM_GENERATED_FILES})

 target_include_directories(Dialog PRIVATE /usr/lib/nim)

    # make the result as small as possible
    # by removing unused code (gc-sections)
    # and by removing macsbug function names on 68K
    # (don't do this when debugging...)
    set_target_properties(Dialog PROPERTIES COMPILE_OPTIONS "-ffunction-sections;-Werror-implicit-function-declaration")
if(CMAKE_SYSTEM_NAME MATCHES Retro68)
  set_target_properties(Dialog PROPERTIES LINK_FLAGS "-Wl,-gc-sections -Wl,--mac-strip-macsbug")
else()
  set_target_properties(Dialog PROPERTIES LINK_FLAGS "-Wl,-gc-sections")
endif()
