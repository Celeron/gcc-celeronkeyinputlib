========================
# gcc-celeronkeyinputlib
========================

General C library (crossplatform) for "Buttons" and "Encoders" input processing in microcontrollers




Features
--------

* Any Button connect schematics (simple buttons or keyboards etc) supported by custom user code. Default code implementation provided for a simple button connection (one Button on particular Port).

* Three Encoder connect methods have provided: 1) "periodical sampling in SysTick interrupt", 2) via port's "change of level" interrupt, 3) by the advanced instrumentality of hardware Timer (on some microcontrollers like STM32)...

* The "suppression of the ringing contacts" feature have implemented (adjustable and disconnectable) for any button or encoder channels. You could freely omit the RC-chain for Buttons... (Hint: for Encoders, rather to use hardware RC-chaines, it is not recomended to use software "ringing suppression"... But you still could!)

* A lot of methods provided for button State recognition: "short button press", "long button press", "several buttons pressed, but another several not pressed, with different time of holding"... Allow flexible user interface programming.

* Integrated implementation of ["Smart Buttons"] (<http://we.easyelectronics.ru/AVR/avrasm-biblioteka-procedur-dlya-intellektualnoy-obrabotki-vvoda-v-mk-sobytiy-ot-knopok-i-enkoderov-chast-2-poryadok-vnedreniya-i-ispolzovaniya.html#handlermagicbutton>) - most useful for easy "values adjustment" via user interface...

* Standardized API for applied code (for ["event handlers"] (<http://we.easyelectronics.ru/AVR/avrasm-biblioteka-procedur-dlya-intellektualnoy-obrabotki-vvoda-v-mk-sobytiy-ot-knopok-i-enkoderov-chast-2-poryadok-vnedreniya-i-ispolzovaniya.html#handler>)).

* Feature parameters are customizable via macro definitions (simple tuning). Rare used Features could be excluded via macro definitions (to reduce library code size).

* 8-bit microcontrollers are supported (since "byte-atomic" arithmetics have implemented in library code). Of course, 32-bit microcontrollers are supported also.




Description
-----------

Not yet written... 

See instead [description on former library port in assembler...] (<http://we.easyelectronics.ru/AVR/avrasm-biblioteka-procedur-dlya-intellektualnoy-obrabotki-vvoda-v-mk-sobytiy-ot-knopok-i-enkoderov-chast-1-avtorskaya-metodika-i-realizaciya.html >)




Brief usage example
-------------------

::

    #include "celeronkeyinputlib.h"

    int main(void) 
    {

      // initialization...


      // main cycle
      while(1) 
      {

        // Event Handlers 
        // Note: you should write mutually exclusive handler conditions!


        if(BUTTON_HAVE_STATUS( BUTTON1, BSC_ShortHold ))
        {
            // Event: BUTTON1 have pushed just now. (Note: other buttons are ignored in this clause.)
        }
        else if(BUTTON_HAVE_STATUS( BUTTON1, BSC_ShortPress ))
        {
            BUTTON_RESET(BUTTON1);
            // Event: BUTTON1 have completely pressed (pushed, holden short time, then released)...
        }



        if(BUTTON_HAVE_FLAG( BUTTON2, BUTTON_IS_HOLDDOWN ))
        {
          // Event: BUTTON2 have puched just now... (you could immediately issue some notification to display or etc)

          if(BUTTON_HAVE_TIME( BUTTON2, 750))
          {
            BUTTON_RESET(BUTTON2);
            // Event: BUTTON2 have holden 750ms already - run the "applied reaction" now!
          }
        }

        else if(BUTTON_HAVE_FLAG( BUTTON2, BUTTON_IS_PRESSED ))
        {
          BUTTON_RESET(BUTTON2);
          // Event: BUTTON2 have released early... 
          // Note: No "applied reaction" have run before! But you could do something other: clear display notification or etc.
        }



        if( !BUTTON_HOLDDOWN_OR_RESET( BUTTON1 ) &&
             BUTTON_HAVE_FLAG(         BUTTON2, BUTTON_IS_HOLDDOWN ) && 
             BUTTON_HAVE_STATUS(       BUTTON3, BSC_LongHold ))
        {
            BUTTON_RESET(BUTTON3);
            // Event: if "BUTTON1" have not touched AND while "BUTTON2" have pushed (like a "Shift" key) AND the "BUTTON3" have pressed (and holden for a long time).
            // Note: This is Combined event of several buttons. You can play chords like on piano!
        }




        // "Smart Buttons" handlers

        if(keyIfSmartButtonPressed( BUTTON_UP, 4 ))
        {
            somevalue++;
        }
        else if(keyIfSmartButtonPressed( BUTTON_DOWN, 4 ))
        {
            somevalue--;
        }

      }

      return 0;
    }




Requirements
------------

GCC compatible C compiler.


Tested with:

- Keil ARMCC compiler (firmware for 32bit "STM32 Cortex-M" microcontrollers: code-independent from low level drivers, like "CMSIS" and "STM32Cube")
- Microchip XC8 compiler (firmware for 8bit "PIC16F", "PIC18F" series microcontrollers)


