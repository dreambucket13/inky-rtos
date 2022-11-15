# set(OUTPUT_NAME inky_pack_demo)

# add_executable(
#   ${OUTPUT_NAME}
#   inky_pack_demo.cpp
# )

# target_include_directories(${OUTPUT_NAME} PRIVATE
#         ${CMAKE_CURRENT_LIST_DIR}
#         ${CMAKE_CURRENT_LIST_DIR}/../drivers/button/
#         ${CMAKE_CURRENT_LIST_DIR}/../drivers/rgbled/
#         ${CMAKE_CURRENT_LIST_DIR}/../drivers/uc8151/
#         )

# # Pull in pico libraries that we need
# target_link_libraries(${OUTPUT_NAME} pico_stdlib hardware_spi hardware_pwm hardware_dma rgbled button uc8151 pico_graphics)

# # create map/bin/hex file etc.
# pico_add_extra_outputs(${OUTPUT_NAME})

#build RTOS blinky
add_executable(main_blinky
        main.c
        main_blinky.c
        )

target_compile_definitions(main_blinky PRIVATE
        mainCREATE_SIMPLE_BLINKY_DEMO_ONLY=1
        )

target_include_directories(main_blinky PRIVATE
        ${CMAKE_CURRENT_LIST_DIR}
        ${CMAKE_CURRENT_LIST_DIR}/App/
        ${CMAKE_CURRENT_LIST_DIR}/../FreeRTOS/Source/include/
        ${CMAKE_CURRENT_LIST_DIR}/../FreeRTOS/Common/include/
        ${CMAKE_CURRENT_LIST_DIR}/../FreeRTOS/Source/portable/ThirdParty/GCC/RP2040/include/
        )

target_link_libraries(main_blinky pico_stdlib FreeRTOS-Kernel FreeRTOS-Kernel-Heap1)
pico_add_extra_outputs(main_blinky)