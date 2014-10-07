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

Most of the code belongs to Dr. Coulston, so I will only discuss my *novel contributions* to his code that enabled me to make lots of cool boxes wherever I want. My contributions were limited to the main loop and the creation of one subroutine; I will only discuss these parts.


#### Initialization
This section initializes the MSP430 and the Nokia Display, then clears the display for use. Then we clear `r10`, `r11`, and `r14` because they will be used later on in the main loop.
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
```
#### Polling
This section of code polls for any button presses. If `SW3` is pressed, it gets ready to build a box. If any of the others are pressed, it moves the cursor to where the user wants to build his or her next box.
```
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
```
#### Preparing to Build
After the user has decided that it's time to build a box, the code waits for the user to let go of the button. Then, we push the values of the cursor, call the box-making subroutine, and pop the cursor values back. After that's all done, we go back to polling.
```
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
```
#### Moving the Cursor
If the user decides that they want to move the cursor and put a box somewhere new and exciting, they come to these loops. Depending on what button was pressed, it will call one of the loops (the names are pretty self explanatory). First, each loops checks to see if the button has been let go of. Then it moves the cursor to one block's length in the desired direction and goes back to polling.
```
moveright:
	bit.b	#BIT1, &P2IN					
	jz		moveright
	add		#8, r11
	jmp		while1
moveleft:
	bit.b	#BIT2, &P2IN					
	jz		moveleft
	sub		#8, r11
	jmp		while1
moveup:
	bit.b	#BIT5, &P2IN					
	jz		moveup
	dec		r10
	jmp		while1
movedown:
	bit.b	#BIT4, &P2IN					
	jz		movedown
	inc		r10
	jmp		while1
```
#### Subroutine: `makethebox`
This here is the money-maker subroutine.  It draws a single, solid, 8-bit line, 8 times in a row from the initial cursor point. This creates a box.
```
makethebox:
	inc		R11
	inc		r14							
	mov		R10, R12					; increment the row
	mov		R11, R13					; and column of the next beam
	call	#setAddress					; we draw

	mov		#NOKIA_DATA, R12			
	mov		#0xFF, R13					
	call	#writeNokiaByte

	cmp		#8, r14
	jeq		donebreak
	jmp		makethebox
donebreak:
	ret
```

## Debugging

Debugging was primarily done by looking at the Nokia 1202 display and the stored register values within CCS-6.0's debugging feature.

## Testing Methodology/Results

I was able to meet the required and "A" functionality.

### Required Functionality

##### Here we see a vast wasteland of LCD pixels.
![alt text](http://i.imgur.com/yFz0D83.jpg "A vast wasteland of LCD pixels...")
##### Press `SW3` and a wild box appears!
![alt text](http://i.imgur.com/HcHysIj.jpg "A WILD BOX APPEARS!")

### A Functionality

##### Press the right button (`SW1`) and then hit `SW3` again and another box appears directly beneath the first.
![alt text](http://i.imgur.com/qPnxqb5.jpg "TWO wild boxes appear!")
##### This time follow the same procedure, but with the down button (`SW4`).
![alt text](http://i.imgur.com/UiEswiK.jpg "Stop the madness!")
##### With this code, you can skip spaces and basically put a box wherever you want. It's pretty cool!
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
