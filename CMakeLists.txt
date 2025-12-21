cmake_minimum_required(VERSION 3.25.0)

set(CMAKE_POLICY_DEFAULT_CMP0077 NEW)

set(PICO_BOARD pico2_w)

include(FetchContent)

FetchContent_Declare(pico_sdk
    GIT_REPOSITORY "https://github.com/raspberrypi/pico-sdk"
    GIT_TAG "2.1.1-correct-picotool"
    GIT_SUBMODULES ""
    GIT_SHALLOW FALSE
)

FetchContent_Populate(pico_sdk)

include(${pico_sdk_SOURCE_DIR}/pico_sdk_init.cmake)

project(butts VERSION 1.0.0)

set(PICO_SDK_PATH ${pico_sdk_SOURCE_DIR} CACHE PATH "")
set(PICO_SDK_PATH_OVERRIDE ${PICO_SDK_PATH} CACHE PATH "")
set(PICOTOOL_FORCE_FETCH_FROM_GIT TRUE)
set(PICO_BUILD_DOCS FALSE)
set(PICO_NO_COPRO_DIS TRUE)
#set(DPICOTOOL_FETCH_FROM_GIT_PATH "$TOOLS_DIR/picotool")
set(PIMORONI_PICO_PATH "${PROJECT_SOURCE_DIR}")
set(PIMORONI_TOOLS_DIR "${PROJECT_SOURCE_DIR}/tools")
set(USER_C_MODULES "${PROJECT_SOURCE_DIR}/boards/i75w_rp2350/usermodules.cmake")
set(MICROPY_BOARD_DIR "${PROJECT_SOURCE_DIR}/boards/i75w_rp2350")
set(MICROPY_BOARD i75w_rp2350)

FetchContent_Declare(micropython
    GIT_REPOSITORY "https://github.com/micropython/micropython"
    GIT_TAG "master"
    GIT_SUBMODULES 
        lib/cyw43-driver
        lib/lwip
        lib/mbedtls
        lib/micropython-lib
        lib/tinyusb
        lib/btstack
    GIT_SHALLOW FALSE
    SOURCE_SUBDIR "ports/rp2"
    CMAKE_ARGS
        -DCMAKE_C_COMPILER_LAUNCHER=ccache 
        -DCMAKE_CXX_COMPILER_LAUNCHER=ccache
)

FetchContent_MakeAvailable(micropython)

add_custom_target(butts DEPENDS micropython)

