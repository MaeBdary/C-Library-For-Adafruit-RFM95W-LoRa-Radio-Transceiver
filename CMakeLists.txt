cmake_minimum_required(VERSION 3.13)

set(CMAKE_C_STANDARD 11)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)



# == DO NOT EDIT THE FOLLOWING LINES for the Raspberry Pi Pico VS Code Extension to work ==
if(WIN32)
    set(USERHOME $ENV{USERPROFILE})
else()
    set(USERHOME $ENV{HOME})
endif()
set(sdkVersion 2.2.0)
set(toolchainVersion 14_2_Rel1)
set(picotoolVersion 2.2.0)
set(picoVscode ${USERHOME}/.pico-sdk/cmake/pico-vscode.cmake)
if (EXISTS ${picoVscode})
    include(${picoVscode})
endif()
# ====================================================================================
set(PICO_BOARD pico_w CACHE STRING "Board type")


include(pico_sdk_import.cmake)

project(LoRaExampleTester C CXX ASM)


pico_sdk_init()



add_executable(LoRaExampleTester 
    LoRaExampleTester.c
    LoRa.c
)

pico_set_program_name(LoRaExampleTester "LoRaExampleTester")
pico_set_program_version(LoRaExampleTester "0.1")


pico_enable_stdio_uart(LoRaExampleTester 0)
pico_enable_stdio_usb(LoRaExampleTester 1)


target_link_libraries(LoRaExampleTester
        pico_stdlib
        hardware_spi
        hardware_gpio
        hardware_sync
        hardware_timer
        LoRa
        )


target_include_directories(LoRaExampleTester PRIVATE
        ${CMAKE_CURRENT_LIST_DIR}
)


target_link_libraries(LoRaExampleTester 
        hardware_spi
        )

set_source_files_properties(pico_lora_c.cpp PROPERTIES LANGUAGE CXX)
target_compile_features(LoRaExampleTester PRIVATE cxx_std_17)


pico_add_extra_outputs(LoRaExampleTester)

