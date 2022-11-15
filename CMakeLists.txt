cmake_minimum_required(VERSION 3.12)

# Pull in PICO SDK (must be before project)
include(pico_sdk_import.cmake)

#pull in FreeRTOS
unset(FREERTOS_KERNEL_PATH)
message(STATUS "Initial FreeRTOS Kernel Path: ${FREERTOS_KERNEL_PATH}")
include(FreeRTOS_Kernel_import.cmake)
message(STATUS "FreeRTOS Kernel Path: ${FREERTOS_KERNEL_PATH}")

#pull in Pimoroni
include(pimoroni_pico_import.cmake)

project(inky-rtos)
set(CMAKE_C_STANDARD 11)
set(CMAKE_CXX_STANDARD 17)

# Initialize the SDK
pico_sdk_init()

function(add_resource target file)
  get_filename_component(NAME ${ARGV1} NAME_WE)
  set(FILENAME ${ARGV1})

  add_custom_command(
    OUTPUT ${NAME}.o

    COMMAND ${CMAKE_COMMAND} -E copy
            C/${FILENAME}
            ${CMAKE_CURRENT_BINARY_DIR}

    COMMAND arm-none-eabi-ld -r -b binary -o ${NAME}.o ${FILENAME}
    DEPENDS ${FILENAME}
  )

  target_sources(${ARGV0} PRIVATE ${NAME}.o)
endfunction(add_resource)

# include_directories(
#   ${CMAKE_CURRENT_LIST_DIR}
#   ${CMAKE_CURRENT_LIST_DIR}/App/
# )

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -Werror")

add_subdirectory(drivers)
add_subdirectory(libraries)
add_subdirectory(App)
add_subdirectory(FreeRTOS)
