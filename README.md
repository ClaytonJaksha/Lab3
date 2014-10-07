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

![alt text](http://i.imgur.com/AnmCYr4.jpg "Reset Timing")

## Code Walkthrough



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
