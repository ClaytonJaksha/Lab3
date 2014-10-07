Lab 3
====
#### Clayton Jaksha | ECE 382 | Dr. York | M2A

## Objective and Purpose
### Objective

The objective of this lab is to manipulate the Nokia 1202 display with the MSP430 microcontroller by use of its front-panel buttons. We must create a box and then create new boxes based off of the user's input into direction control boxes.

### Purpose

The purpose of this lab is to familiarize us with SPI, I/O, and hardware/software interface.

## Preliminary Design



## Code Walkthrough

| Line | `R12` | `R13` | Purpose |
|------|-------|-------|---------|
| 66 | `0x01` | `0xE7` | Draws 8-bit high beam with 2-bit gap |
| 277 | `0x00` | `0xB9` | Mask out any weird nibble bits <br> mask in "BO" as the prefix for a page address |
| 289 | `0x00` | `0x11` | Mask out upper nibble <br> 10 is the prefix for an upper column address |
| 295 | `0x00` | `0x06` | Set up call for next instruction |

| Line | Command/Data | 8-Bit Packet |
|------|-------|-------|---------|
| 66 | Data | `0xE7` |
| 277 | Command | `0xB9` |
| 289 | Command | `0x11` |
| 295 | Command | `0x06` |

#### Initialization



#### Main Loop



## Debugging

Debugging was done through iterative and line-by-line testing. Looking at each register and memory location after executing an instruction, I hunted down mistakes and corrected them. There were no major code overhauls. No issues requiring debugging exist in the current version of the code.

Only the A functionality test required some serious debugging since it depends on educated guesswork. I noticed the byte `0x90` appeared quite a bit and it was also the last byte of the message, so I guessed it was a period. I inputted a `"."` into `guess2` and started out with a `" "` in `guess1` since I figured the other most likely byte should be a space (even though they could both be periods). I received the following result:

![alt text](http://i.imgur.com/JnVPzRh.png "Guess 1")

Looking at the result, we notice about half the message makes sense. Everything keyed off `guess2` is a reasonable character. Therefore, I assumed `guess2` was correct and progressed onto determining the correct value of `guess1`. I moved onto vowels, starting with `"a"`. I received the following result:

![alt text](http://i.imgur.com/lxELMPH.png "Guess 2")



![alt text](http://i.imgur.com/KsmQd8K.png "Guess 3")


## Testing Methodology/Results

[Three test cases](http://ece382.com/labs/lab2/index.html) are provided on the [ECE 382 Website](http://ece382.com). The tests and results are found below.

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
