<img src="https://raw.githubusercontent.com/mytechnotalent/ATmega328P_IO_Driver/refs/heads/main/ATmega328P%20IO%20Driver.png">

## FREE Reverse Engineering Self-Study Course [HERE](https://github.com/mytechnotalent/Reverse-Engineering-Tutorial)

<br>

# ATmega328P IO Driver
An ATmega328P IO driver written entirely in Assembler.

<br>

# Code
```assembler
; ===================================================================
; Project: ATmega328P IO Driver
; ===================================================================
; Author: Kevin Thomas
; E-Mail: ket189@pitt.edu
; Version: 1.0
; Date: 12/27/24
; Target Device: ATmega328P (Arduino Nano)
; Clock Frequency: 16 MHz
; Toolchain: AVR-AS, AVRDUDE
; License: Apache License 2.0
; Description: This program is a simple I/O to which when a button
;              connected to PD2 is pressed, an external LED connected
;              to PD4 will illuminate, otherwise it will be off.
; ===================================================================

; ===================================================================
; SYMBOLIC DEFINITIONS
; ===================================================================
.equ     DDRD, 0x0A               ; Data Direction Register for PORTD
.equ     PORTD, 0x0B              ; PORTD Data Register
.equ     PIND, 0x09               ; Input Pins Address for PORTD
.equ     PD2, 2                   ; Pin 2 of PORTD (D2 on Nano)
.equ     PD4, 4                   ; Pin 4 of PORTD (D4 on Nano)   

; ===================================================================
; PROGRAM ENTRY POINT
; ===================================================================
.global  program                  ; global label; make avail external
.section .text                    ; start of the .text (code) section

; ===================================================================
; PROGRAM LOOP
; ===================================================================
; Description: Main program loop which executes all subroutines and 
;              then repeads indefinately.
; -------------------------------------------------------------------
; Instructions: AVR Instruction Set Manual
;               6.87 RCALL – Relative Call to Subroutine
;               6.90 RJMP – Relative Jump
; ===================================================================
program:
  RCALL  Config_Pins              ; config pins
program_loop:
  RCALL  Check_Button             ; check button state; control LED
  RJMP   program_loop             ; infinite loop

; ===================================================================
; SUBROUTINE: Config_Pins
; ===================================================================
; Description: Main configuration of pins on the ATmega128P Arduino 
;              Nano.
; -------------------------------------------------------------------
; Instructions: AVR Instruction Set Manual
;               6.33 CBI – Clear Bit in I/O Register
;               6.95 SBI – Set Bit in I/O Register
;               6.88 RET – Return from Subroutine
; ===================================================================
Config_Pins:
  CBI    DDRD, PD2                ; set PD2 as input (button pin)
  SBI    PORTD, PD2               ; enable pull-up resistor on PD2
  SBI    DDRD, PD4                ; set PD4 as output (LED pin)
  RET                             ; return from subroutine

; ===================================================================
; SUBROUTINE: Check_Button
; ===================================================================
; Description: Checks if PD2 is pressed and if so, drive LED high,
;              otherwise drive LED low.
; -------------------------------------------------------------------
; Instructions: AVR Instruction Set Manual
;               6.97 SBIS – Skip if Bit in I/O Register is Set
;               6.96 SBIC – Skip if Bit in I/O Register is Cleared
;               6.88 RET – Return from Subroutine
; ===================================================================
Check_Button:
  SBIS   PIND, PD2                ; skip next inst if PD2 is high
  RCALL  Led_On                   ; if PD2 is low; BTN pressed
  SBIC   PIND, PD2                ; skip next inst if PD2 is low
  RCALL  Led_Off                  ; if PD2 is high; BTN not pressed
  RET                             ; return from subroutine

; ===================================================================
; SUBROUTINE: Led_On
; ===================================================================
; Description: Sets PB5 high to turn on the LED.
; -------------------------------------------------------------------
; Instructions: AVR Instruction Set Manual
;               6.95 SBI – Set Bit in I/O Register
;               6.88 RET – Return from Subroutine
; ===================================================================
Led_On:
  SBI    PORTD, PD4               ; set PD4 high
  RET                             ; return from subroutine

; ===================================================================
; SUBROUTINE: Led_Off
; ===================================================================
; Description: Clears PB5 to turn off the LED.
; -------------------------------------------------------------------
; Instructions: AVR Instruction Set Manual
;               6.33 CBI – Clear Bit in I/O Register
;               6.88 RET – Return from Subroutine
; ===================================================================
Led_Off:
  CBI    PORTD, PD4               ; set PD4 low
  RET                             ; return from subroutine
```

<br>

## License
[Apache License 2.0](https://github.com/mytechnotalent/ATmega328P_IO_Driver/blob/main/LICENSE)
