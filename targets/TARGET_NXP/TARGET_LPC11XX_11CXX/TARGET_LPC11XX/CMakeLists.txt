# Copyright (c) 2021 ARM Limited. All rights reserved.
# SPDX-License-Identifier: Apache-2.0

include(${MBED_PATH}/targets/TARGET_NXP/scripts/mbed_set_post_build_nxp.cmake)

add_library(mbed-lpc11xx INTERFACE)

target_include_directories(mbed-lpc11xx
    INTERFACE
        .
)

target_sources(mbed-lpc11xx
    INTERFACE
        device/system_LPC11xx.c
)

target_link_libraries(mbed-lpc11xx INTERFACE mbed-lpc11xx-11cxx)

mbed_post_build_lpc_patch_vtable()
