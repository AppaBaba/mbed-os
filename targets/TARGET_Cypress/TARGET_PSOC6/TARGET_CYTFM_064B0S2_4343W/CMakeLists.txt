# Copyright (c) 2020 ARM Limited. All rights reserved.
# SPDX-License-Identifier: Apache-2.0

include(${MBED_PATH}/targets/TARGET_Cypress/scripts/mbed_set_post_build_cypress.cmake)

if("BSP_DESIGN_MODUS" IN_LIST MBED_TARGET_LABELS)
    target_include_directories(mbed-core
        INTERFACE
            COMPONENT_BSP_DESIGN_MODUS/GeneratedSource
    )

    target_sources(mbed-core
        INTERFACE
            COMPONENT_BSP_DESIGN_MODUS/GeneratedSource/cycfg.c
            COMPONENT_BSP_DESIGN_MODUS/GeneratedSource/cycfg_clocks.c
            COMPONENT_BSP_DESIGN_MODUS/GeneratedSource/cycfg_peripherals.c
            COMPONENT_BSP_DESIGN_MODUS/GeneratedSource/cycfg_pins.c
            COMPONENT_BSP_DESIGN_MODUS/GeneratedSource/cycfg_qspi_memslot.c
            COMPONENT_BSP_DESIGN_MODUS/GeneratedSource/cycfg_routing.c
            COMPONENT_BSP_DESIGN_MODUS/GeneratedSource/cycfg_system.c
    )
endif()

if("CM4" IN_LIST MBED_TARGET_LABELS)
    set(SYSTEM_SOURCE device/COMPONENT_CM4/system_psoc6_cm4.c)

    target_include_directories(mbed-core
        INTERFACE
            device/COMPONENT_CM4
    )

    target_sources(mbed-core
        INTERFACE
            device/COMPONENT_CM4/device_definition.c
    )

    if(${MBED_TOOLCHAIN} STREQUAL "ARM")
        set(LINKER_FILE device/COMPONENT_CM4/TOOLCHAIN_ARM/cyb06xxa_cm4_dual.sct)
        set(STARTUP_FILE device/COMPONENT_CM4/TOOLCHAIN_ARM/startup_psoc6_02_cm4.S)
    elseif(${MBED_TOOLCHAIN} STREQUAL "GCC_ARM")
        set(LINKER_FILE device/COMPONENT_CM4/TOOLCHAIN_GCC_ARM/cyb06xxa_cm4_dual.ld)
        set(STARTUP_FILE device/COMPONENT_CM4/TOOLCHAIN_GCC_ARM/startup_psoc6_02_cm4.S)
    endif()
endif()

set_property(
    GLOBAL PROPERTY
    MBED_TARGET_LINKER_FILE
        ${CMAKE_CURRENT_SOURCE_DIR}/${LINKER_FILE}
)

target_include_directories(mbed-core
    INTERFACE
        .
        device
        partition
)

target_sources(mbed-core
    INTERFACE
        PeripheralPins.c
        cybsp.c
        cytfm_flash_info.c
        ${SYSTEM_SOURCE}
        ${STARTUP_FILE}
)

mbed_post_build_psoc6_sign_image(
    "tfm_s.hex"
    "CYTFM_064B0S2_4343W"
    "policy_multi_CM0_CM4_tfm.json"
    "multi_image"
    "1"
    "16"
    ${MBED_PATH}/targets/TARGET_Cypress/TARGET_PSOC6/TARGET_CYTFM_064B0S2_4343W/COMPONENT_TFM_S_FW/tfm_s.hex
)
