# Generated Cmake Pico project file

cmake_minimum_required(VERSION 3.13)

set(CMAKE_C_STANDARD 11)
set(CMAKE_CXX_STANDARD 17)

# Initialise pico_sdk from installed location
# (note this can come from environment, CMake cache etc)
if(NOT DEFINED PICO_SDK_PATH)
    set(PICO_SDK_PATH "/home/seven/pico/pico-sdk")
endif()
if(NOT DEFINED PICO_BOARD)
    set(PICO_BOARD pico2)
endif()
# Pull in Raspberry Pi Pico SDK (must be before project)
include(pico_sdk_import.cmake)


set(PROJECT_BASE_NAME "infoNES")
set(PROJECT_FINAL_NAME "${PROJECT_BASE_NAME}_${PICO_BOARD}")

project(${PROJECT_FINAL_NAME} C CXX ASM)

# Initialise the Raspberry Pi Pico SDK
pico_sdk_init()

# Add executable. Default name is the project name, version 0.1

add_subdirectory(drivers/fatfs)
add_subdirectory(drivers/sdcard)

add_executable(${PROJECT_FINAL_NAME} InfoNES.cpp K6502.cpp InfoNES_Mapper.cpp InfoNES_pAPU.cpp tar.cpp audio.c menu.cpp RomLister.cpp FrensHelpers.cpp main.cpp )

pico_set_program_name(${PROJECT_FINAL_NAME} "${PROJECT_FINAL_NAME}")
pico_set_program_version(${PROJECT_FINAL_NAME} "0.1")

pico_enable_stdio_uart(${PROJECT_FINAL_NAME} 1)
pico_enable_stdio_usb(${PROJECT_FINAL_NAME} 1)

# Add the standard library to the build
target_link_libraries(${PROJECT_FINAL_NAME} pico_stdlib)

set(SD_SPI "spi1" CACHE STRING "Specify the SPI bus for the SD card")
set(SD_CS "13" CACHE STRING "Specify the Chip Select GPIO pin for the SD card")
set(SD_SCK "14" CACHE STRING "Specify de Clock GPIO pin for the SD card")
set(SD_MOSI "15" CACHE STRING "Select the Master Out Slave In GPIO pin for the SD card")
set(SD_MISO "12" CACHE STRING "Select the Master In Slave Out GPIO pin for the SD card")

set(LCD_CONTROLLER "ILI9341" CACHE STRING "Select the LCD controller type")
#set(LCD_CONTROLLER "ST7789" CACHE STRING "Select the LCD controller type")

set(LCD_SPI "spi0")
set(LCD_DC "7")
set(LCD_CS "4")
set(LCD_CLK "2")
set(LCD_MOSI "3")
set(LCD_RST "8")
set(LCD_BL "-1")
set(LCD_MISO "-1")

set(PIN_UP "17")
set(PIN_DN "19")
set(PIN_LT "16")
set(PIN_RT "18")
set(PIN_SL "22")
set(PIN_ST "26")
set(PIN_A "21")
set(PIN_B "20")

message("SD card SPI         : ${SD_SPI}")
message("SD card CS          : ${SD_CS}")
message("SD card SCK         : ${SD_SCK}")
message("SD card MOSI        : ${SD_MOSI}")
message("SD card MISO        : ${SD_MISO}")
message("LCD CONTROLLER      : ${LCD_CONTROLLER}")
message("LCD SPI             : ${LCD_SPI}")
message("LCD DC              : ${LCD_DC}")
message("LCD CS              : ${LCD_CS}")
message("LCD CLK             : ${LCD_CLK}")
message("LCD MOSI            : ${LCD_MOSI}")
message("LCD RST             : ${LCD_RST}")
message("LCD BL              : ${LCD_BL}")
message("LCD MISO            : ${LCD_MISO}")
message("BUTTON UP           : ${PIN_UP}")
message("BUTTON DN           : ${PIN_DN}")
message("BUTTON LT           : ${PIN_LT}")
message("BUTTON RT           : ${PIN_RT}")
message("BUTTON SL           : ${PIN_SL}")
message("BUTTON ST           : ${PIN_ST}")
message("BUTTON A            : ${PIN_A}")
message("BUTTON B            : ${PIN_B}")
target_compile_definitions(${PROJECT_FINAL_NAME} PRIVATE
    SDCARD_SPI_BUS=${SD_SPI}
    SDCARD_PIN_SPI0_CS=${SD_CS}
    SDCARD_PIN_SPI0_SCK=${SD_SCK}
    SDCARD_PIN_SPI0_MOSI=${SD_MOSI}
    SDCARD_PIN_SPI0_MISO=${SD_MISO}
    DISPLAY_SPI_PORT=${LCD_SPI}
    DISPLAY_PIN_DC=${LCD_DC}
    DISPLAY_PIN_CS=${LCD_CS}
    DISPLAY_PIN_CLK=${LCD_CLK}
    DISPLAY_PIN_MOSI=${LCD_MOSI}
    DISPLAY_PIN_RST=${LCD_RST}
    DISPLAY_PIN_BL=${LCD_BL}
    DISPLAY_PIN_MISO=${LCD_MISO}
    PIN_A=${PIN_A}
    PIN_B=${PIN_B}
    PIN_ST=${PIN_ST}
    PIN_SL=${PIN_SL}
    PIN_LT=${PIN_LT}
    PIN_RT=${PIN_RT}
    PIN_UP=${PIN_UP}
    PIN_DN=${PIN_DN}
)
if(LCD_CONTROLLER STREQUAL "ILI9341")
    add_compile_definitions(ILI9341)
elseif(LCD_CONTROLLER STREQUAL "ST7789")
    add_compile_definitions(ST7789)
endif()

# Add any user requested libraries
target_link_libraries(${PROJECT_FINAL_NAME}
        pico_stdlib
	pico_multicore
        hardware_spi
        hardware_dma
        hardware_pio
	hardware_i2c
	hardware_interp
    	hardware_timer
    	hardware_clocks
    	hardware_pwm
    	hardware_flash
    sdcard
    fatfs        
        )

pico_add_extra_outputs(${PROJECT_FINAL_NAME})

