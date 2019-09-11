# STM32F103VET6 STUno

An ST development board based on the STM32F103VET6/STM32F103VE (Clock: 72MHz, Flash: 512Kb, Ram:64Kb) MCU (the LQFP-100 version of the famous Blue Pill) compatible with STM32Duino (Arduino).

<img align="top" src="rSTM32F103VET6.main.jpg" />

Pinout:

<img align="top" width=80% src="rSTM32F103VET6.pinout.jpg" />


## Files and Folders description

- **RBOARD_F103VET6** it's the variant board for the STM32 Arduino Core.
- **rSTM32F103VET6** it's the KiCAD 5.1.2 project.
- **rSTM32F103VET6.schematic.pdf** the schematic.


## Install

_pending..._


## Installation Bug

There is a bug on the core 1.6.1 (and probably olders), which didn't let you compile anything, even the blink sketch, this is bug that ST has already acknowledge and fixed on their 1.7 version (which at the time of writing this documentation isn't yet released). So if you're using the 1.7 core or newer you can skip this part, if you are using 1.6.1 you do need to manually fix the problem (thanks to **pkourany** for publish this [bug and its solution](https://github.com/stm32duino/Arduino_Core_STM32/issues/585) ).

So, you need to change the function _void TIM8_IRQHandler(void)_ on the file **timer.c** (on win7 located at: C:\Users\<USER_NAME>\AppData\Local\Arduino15\packages\STM32\hardware\stm32\1.6.1\cores\arduino\stm32\ ,line around 1321) and leave it as follow:

```
void TIM8_IRQHandler(void)
{
  if (timer_handles[TIMER8_INDEX] != NULL) {
    HAL_TIM_IRQHandler(timer_handles[TIMER8_INDEX]);
  }

#if defined(STM32F1xx) || defined(STM32F2xx) ||defined(STM32F4xx) || defined(STM32F7xx)
#if defined(TIMER13_BASE)
  if (timer_handles[TIMER13_INDEX] != NULL) {
    HAL_TIM_IRQHandler(timer_handles[TIMER13_INDEX]);
  }
#endif
#endif
}
```

That's it, the arduino ide should compile your sketches fine.



## The real board

**Front**

<img align="top" src="rSTM32F103VET6.front.jpg" />

**Back**

<img align="top" src="rSTM32F103VET6.back.jpg" />

_*The footprint for the second crystal (Y2) was wrong on this board but it's already corrected on the KiCAD files._


## Running some tests

<img align="top" src="rSTM32F103VET6.sample0.jpg" />


The **STUno** running the Adafruit ILI9341 library over hardware SPI.


_Video in progress..._
