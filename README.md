<img src="https://raw.githubusercontent.com/mytechnotalent/ATmega128P_IO_Driver/refs/heads/main/ATmega328P%20IO%20Driver.png">

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
  rcall  config_pins              ; config pins
program_loop:
  rcall  check_button             ; check button state; control LED
  rjmp   program_loop             ; infinite loop

; ===================================================================
; SUBROUTINE: config_pins
; ===================================================================
; Description: Main configuration of pins on the ATmega128P Arduino 
;              Nano.
; -------------------------------------------------------------------
; Instructions: AVR Instruction Set Manual
;               6.33 CBI – Clear Bit in I/O Register
;               6.95 SBI – Set Bit in I/O Register
;               6.88 RET – Return from Subroutine
; ===================================================================
config_pins:
  cbi    DDRD, PD2                ; set PD2 as input (button pin)
  sbi    PORTD, PD2               ; enable pull-up resistor on PD2
  sbi    DDRD, PD4                ; set PD4 as output (LED pin)
  ret                             ; return from subroutine

; ===================================================================
; SUBROUTINE: check_button
; ===================================================================
; Description: Checks if PD2 is pressed and if so, drive LED high,
;              otherwise drive LED low.
; -------------------------------------------------------------------
; Instructions: AVR Instruction Set Manual
;               6.97 SBIS – Skip if Bit in I/O Register is Set
;               6.96 SBIC – Skip if Bit in I/O Register is Cleared
;               6.88 RET – Return from Subroutine
; ===================================================================
check_button:
  sbis   PIND, PD2                ; skip next inst if PD2 is high
  rcall  led_on                   ; if PD2 is low; BTN pressed
  sbic   PIND, PD2                ; skip next inst if PD2 is low
  rcall  led_off                  ; if PD2 is high; BTN not pressed
  ret                             ; return from subroutine

; ===================================================================
; SUBROUTINE: led_on
; ===================================================================
; Description: Sets PB5 high to turn on the LED.
; -------------------------------------------------------------------
; Instructions: AVR Instruction Set Manual
;               6.95 SBI – Set Bit in I/O Register
;               6.88 RET – Return from Subroutine
; ===================================================================
led_on:
  sbi    PORTD, PD4               ; set PD4 high
  ret                             ; return from subroutine

; ===================================================================
; SUBROUTINE: led_off
; ===================================================================
; Description: Clears PB5 to turn off the LED.
; -------------------------------------------------------------------
; Instructions: AVR Instruction Set Manual
;               6.33 CBI – Clear Bit in I/O Register
;               6.88 RET – Return from Subroutine
; ===================================================================
led_off:
  cbi    PORTD, PD4               ; set PD4 low
  ret                             ; return from subroutine
```

<br>

## License
[Apache License 2.0](https://github.com/mytechnotalent/ATmega128P_IO_Driver/blob/main/LICENSE)

