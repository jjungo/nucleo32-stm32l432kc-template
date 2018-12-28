A simple template for the STM32L432KC dev kit.

### build

    mkdir build && cd build
    cmake ..
    make

## flash

     st-flash write build/nucleo32-stm32l432kc-base.bin 0x8000000
