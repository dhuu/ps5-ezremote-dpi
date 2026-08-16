cmake_minimum_required(VERSION 3.5)

project(ezremote-dpi)

# ---------------------------------------------------------------
# PS5 Firmware Target Version
# Supported firmware range: up to 13.40 (0x1340)
# Set PS5SDK_FW to the hex representation of the target firmware.
# Examples:
#   11.00 = 0x1100
#   12.00 = 0x1200
#   13.00 = 0x1300
#   13.40 = 0x1340  <-- current max supported
# ---------------------------------------------------------------
if(NOT DEFINED ENV{PS5SDK_FW})
  set(ENV{PS5SDK_FW} "0x1340")
endif()
add_compile_definitions(PS5_TARGET_FIRMWARE=$ENV{PS5SDK_FW})

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Wno-keyword-macro -fPIC -fPIE -march=znver2 -Wall -fstack-protector-all")
set(CMAKE_CXX_FLAGS "${CMAKE_C_FLAGS} -DDEBUG -gfull -gdwarf-2 -O0 -pedantic -pedantic-errors")
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -O0 ")

include_directories(
  source
)

add_executable(${PROJECT_NAME}.elf
  source/main.cpp
)

target_link_libraries(${PROJECT_NAME}.elf
  SceSysCore
  kernel_sys
  SceAppInstUtil
)
