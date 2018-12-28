cmake_minimum_required(VERSION 3.5.0)
project(nucleo32-stm32l432kc-base C ASM)

SET(MCU_TARGET STM32L432xx)
SET(ARCH -march=armv7e-m)
SET(TUNE -mtune=cortex-m4)
SET(FLOAT-ABI -mfloat-abi=hard)
SET(FPU -mfpu=fpv4-sp-d16)
SET(SPECS -specs=nano.specs)
SET(WARN_FLAGS "-Wall -Wextra")
SET(LINKER_SCRIPT ${CMAKE_SOURCE_DIR}/STM32L432KCUx_FLASH.ld)


SET(HEX_FILE ${PROJECT_NAME}.hex)
SET(BIN_FILE ${PROJECT_NAME}.bin)

SET(CMAKE_SYSTEM_NAME Generic)
SET(CMAKE_SYSTEM_VERSION 1)


SET(COMMON_FLAGS "-mthumb ${ARCH} ${TUNE} ${FLOAT-ABI} ${FPU} -ffunction-sections -fdata-sections -MMD -MP ${WARN_FLAGS} ${SPEC}")

if (CMAKE_BUILD_TYPE MATCHES Debug)
    add_definitions(-DDEBUG=1)
    SET(COMMON_FLAGS "${COMMON_FLAGS} -g -gdwarf-2")
elseif (CMAKE_BUILD_TYPE MATCHES Release)
    SET(COMMON_FLAGS "${COMMON_FLAGS} -O3")
endif ()

SET(CMAKE_C_COMPILER arm-none-eabi-gcc)
SET(CMAKE_CXX_FLAGS "${COMMON_FLAGS} -std=c++14")
SET(CMAKE_C_FLAGS "${COMMON_FLAGS} -std=c11")
SET(CMAKE_EXE_LINKER_FLAGS " -lm -lnosys -fstack-usage -Wl,-gc-sections -Xlinker -Map=${PROJECT_NAME}.map -T ${LINKER_SCRIPT}")
SET(CMAKE_ASM_FLAGS "${COMMON_FLAGS}")
SET(CMAKE_OBJCOPY arm-none-eabi-objcopy)
SET(CMAKE_SIZE arm-none-eabi-size)
SET(ROOT_SOURCES ${CMAKE_SOURCE_DIR})
SET(INCLUDES_PATH ${ROOT_SOURCES}/Inc)

add_definitions(-D${MCU_TARGET} -DUSE_HAL_DRIVER)
set(SOURCES
        ${ROOT_SOURCES}/Src/gpio.c
        ${ROOT_SOURCES}/Src/i2c.c
        ${ROOT_SOURCES}/Src/main.c
        ${ROOT_SOURCES}/Src/spi.c
        ${ROOT_SOURCES}/Src/stm32l4xx_hal_msp.c
        ${ROOT_SOURCES}/Src/stm32l4xx_it.c
        ${ROOT_SOURCES}/Src/system_stm32l4xx.c
        ${ROOT_SOURCES}/Src/tim.c
        ${ROOT_SOURCES}/Src/usart.c
        )

set(HAL_SOURCES
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_hal_adc.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_hal_adc_ex.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_hal.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_hal_can.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_hal_comp.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_hal_cortex.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_hal_crc.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_hal_crc_ex.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_hal_cryp.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_hal_cryp_ex.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_hal_dac.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_hal_dac_ex.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_hal_dcmi.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_hal_dfsdm.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_hal_dfsdm_ex.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_hal_dma2d.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_hal_dma.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_hal_dma_ex.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_hal_dsi.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_hal_firewall.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_hal_flash.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_hal_flash_ex.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_hal_flash_ramfunc.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_hal_gfxmmu.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_hal_gpio.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_hal_hash.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_hal_hash_ex.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_hal_hcd.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_hal_i2c.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_hal_i2c_ex.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_hal_irda.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_hal_iwdg.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_hal_lcd.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_hal_lptim.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_hal_ltdc.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_hal_ltdc_ex.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_hal_nand.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_hal_nor.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_hal_opamp.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_hal_opamp_ex.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_hal_ospi.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_hal_pcd.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_hal_pcd_ex.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_hal_pwr.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_hal_pwr_ex.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_hal_qspi.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_hal_rcc.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_hal_rcc_ex.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_hal_rng.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_hal_rtc.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_hal_rtc_ex.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_hal_sai.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_hal_sai_ex.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_hal_sd.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_hal_sd_ex.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_hal_smartcard.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_hal_smartcard_ex.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_hal_smbus.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_hal_spi.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_hal_spi_ex.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_hal_sram.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_hal_swpmi.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_hal_tim.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_hal_tim_ex.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_hal_tsc.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_hal_uart.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_hal_uart_ex.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_hal_usart.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_hal_usart_ex.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_hal_wwdg.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_ll_adc.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_ll_comp.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_ll_crc.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_ll_crs.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_ll_dac.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_ll_dma2d.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_ll_dma.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_ll_exti.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_ll_fmc.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_ll_gpio.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_ll_i2c.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_ll_lptim.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_ll_lpuart.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_ll_opamp.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_ll_pwr.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_ll_rcc.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_ll_rng.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_ll_rtc.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_ll_sdmmc.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_ll_spi.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_ll_swpmi.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_ll_tim.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_ll_usart.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_ll_usb.c
        ${ROOT_SOURCES}/Drivers//STM32L4xx_HAL_Driver/Src/stm32l4xx_ll_utils.c
        )


SET(VENDOR_SRC
        ${HAL_SOURCES}
        ${ROOT_SOURCES}/startup_stm32l432xx.s
        ${ROOT_SOURCES}/Src/vendor/Segger/RTT_Syscalls_GCC.c
        ${ROOT_SOURCES}/Src/vendor/Segger/SEGGER_RTT.c
        ${ROOT_SOURCES}/Src/vendor/Segger/SEGGER_RTT_printf.c
        )

set_property(SOURCE
        ${ROOT_SOURCES}/startup_stm32l432xx.s
        PROPERTY LANGUAGE C)


include_directories(${ROOT_SOURCES}
        ${ROOT_SOURCES}/Inc
        ${ROOT_SOURCES}/Drivers/STM32L4xx_HAL_Driver/Inc
        ${ROOT_SOURCES}/Drivers/CMSIS/Device/ST/STM32L4xx/Include
        ${ROOT_SOURCES}/Drivers/CMSIS/Include)


add_executable(${PROJECT_NAME}.elf ${SOURCES} ${VENDOR_SRC} ${LINKER_SCRIPT})

add_custom_command(TARGET ${PROJECT_NAME}.elf POST_BUILD
        COMMAND ${CMAKE_OBJCOPY} -Oihex $<TARGET_FILE:${PROJECT_NAME}.elf> ${HEX_FILE}
        COMMAND ${CMAKE_OBJCOPY} -Obinary $<TARGET_FILE:${PROJECT_NAME}.elf> ${BIN_FILE}
        COMMAND ${CMAKE_SIZE} ${PROJECT_NAME}.elf
        COMMENT "Building ${HEX_FILE} \nBuilding ${BIN_FILE}")
