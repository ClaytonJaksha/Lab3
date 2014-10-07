Lab 3
====
#### Clayton Jaksha | ECE 382 | Dr. York | M2A

## Objective and Purpose
### Objective

The objective of this lab is to manipulate the Nokia 1202 display with the MSP430 microcontroller by use of its front-panel buttons. We must create a box and then create new boxes based off of the user's input into direction control boxes.

### Purpose

The purpose of this lab is to familiarize us with SPI, I/O, and hardware/software interface.

## Preliminary Design

In order to make a box, I must make a straight line and then repeat the line in 7 adjacent columns. This can be made into a subroutine.

In order to place a box wherever a user wants, we must have a cursor controlled by the Nokia 1202 buttons. Then, at the desired cursor locations, call the subroutine that creates a box.

## Required Tables/Logic Analyzer

##### Data Packets Coded to be Sent
| Line | `R12` | `R13` | Purpose |
|------|-------|-------|---------|
| 66 | `0x01` | `0xE7` | Draws 8-bit high beam with 2-bit gap |
| 277 | `0x00` | `0xB6` | Mask out any weird nibble bits <br> mask in "BO" as the prefix for a page address |
| 289 | `0x00` | `0x11` | Mask out upper nibble <br> 10 is the prefix for an upper column address |
| 295 | `0x00` | `0x06` | Set up call for next instruction |

##### The four signals sent to the Nokia 1202 as seen by the logic analyzer
The first non-bused signal is `SMCLK`, the second is `MOSI`
![alt text](http://i.imgur.com/beL5Rfg.jpg "1st Signal")
![alt text](http://i.imgur.com/YfaUCrU.jpg "2nd Signal")
![alt text](http://i.imgur.com/Al6VBRp.jpg "3rd Signal")
![alt text](http://i.imgur.com/xX3yULg.jpg "4th Signal")

##### Signals Sent to Nokia 1202
| Line | Command/Data | 8-Bit Packet |
|------|-------|-------|---------|
| 66 | Data | `0xE7` |
| 277 | Command | `0xB6` |
| 289 | Command | `0x11` |
| 295 | Command | `0x06` |


##### Reset Signal
In order to capture the reset signal, I had to reload the program on the MSP430 while the logic analyzer was running. The below logical analyzer printout shows that the reset signal (annotated) is applied for roughly 20ms after the program is loaded.

In order to create this delay, the program counts backwards from `0xFFFF` to `0x0000` using `dec` command. Simple math reveals that each count takes 0.020627sec/0xFFFF=3.147x10^-7 sec= **315 ns**
![alt text](http://i.imgur.com/AnmCYr4.jpg "Reset Timing")

## Code Walkthrough

Most of the code belongs to Dr. Coulston, so I will only discuss my *novel contributions* to his code that enabled me to make lots of cool boxes wherever I want.

```
main:
	mov.w   #__STACK_END,SP				; Initialize stackpointer
	mov.w   #WDTPW|WDTHOLD, &WDTCTL  	; Stop watchdog timer
	dint								; disable interrupts

	call	#init						; initialize the MSP430
	call	#initNokia					; initialize the Nokia 1206
	call	#clearDisplay				; clear the display and get ready....

	clr		R10							; used to move the cursor around
	clr		R11
	clr 	r14

while1:
	bit.b	#8, &P2IN
	jz		while0					; bit 3 of P1IN set?
whileleft:
	bit.b	#BIT2, &P2IN
	jz		moveleft
whileright:
	bit.b	#BIT1, &P2IN
	jz		moveright
whileup:
	bit.b	#BIT5, &P2IN
	jz		moveup
whiledown:
	bit.b	#BIT4, &P2IN
	jz		movedown
	jmp		while1						; Yes, branch back and wait


while0:
	bit.b	#8, &P2IN					; bit 3 of P1IN clear?
	jz		while0						; Yes, branch back and wait
	push	r10
	push	r11
	call	#makethebox
	pop		r11
	pop		r10
	clr		r14
	jmp		while1

moveright:
	bit.b	#BIT1, &P2IN					; bit 3 of P1IN clear?
	jz		moveright
	add		#8, r11
	jmp		while1
moveleft:
	bit.b	#BIT2, &P2IN					; bit 3 of P1IN clear?
	jz		moveleft
	sub		#8, r11
	jmp		while1
moveup:
	bit.b	#BIT5, &P2IN					; bit 3 of P1IN clear?
	jz		moveup
	dec		r10
	jmp		while1
movedown:
	bit.b	#BIT4, &P2IN					; bit 3 of P1IN clear?
	jz		movedown
	inc		r10
	jmp		while1

makethebox:
	inc		R11
	inc		r14							; just let the columm overflow after 92 buttons
	mov		R10, R12					; increment the row
	mov		R11, R13					; and column of the next beam
	call	#setAddress					; we draw

	mov		#NOKIA_DATA, R12			; For testing just draw an 8 pixel high
	mov		#0xFF, R13					; beam with a 2 pixel hole in the center
	call	#writeNokiaByte

	cmp		#8, r14
	jeq		donebreak
	jmp		makethebox
donebreak:
	ret
```

#### Initialization



#### Main Loop



## Debugging



## Testing Methodology/Results



#### Required Functionality

![alt text](http://i.imgur.com/yFz0D83.jpg "A vast wasteland of LCD pixels...")

![alt text](http://i.imgur.com/HcHysIj.jpg "A WILD BOX APPEARS!")

#### A Functionality

![alt text](http://i.imgur.com/qPnxqb5.jpg "TWO wild boxes appear!")

![alt text](http://i.imgur.com/UiEswiK.jpg "Stop the madness!")

![alt text](http://i.imgur.com/7zLSPcI.jpg "This could quickly turn into the most self-defeating game of snake")

## Observations and Conclusion
#### Observations

* If a single step in SPI is messed up, the whole thing will not work.
* Outputting data is not as simple as writing to a port.


#### Conclusion

The objective of this lab was to design code create boxes located on the user's locational input. I successfully met this objective.

## Documentation

None


# GO ARMY, BEAT AIR FORCE
![alt text](http://i.imgur.com/hxyaK0K.jpg "GO ARMY, BEAT AIR FORCE")
