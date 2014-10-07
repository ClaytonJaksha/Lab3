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

Also incorrect. I moved through the vowels, changing `guess1` to `"e"`. I recieved this:

![alt text](http://i.imgur.com/KsmQd8K.png "Guess 3")

This time: success! The third time's the charm. I knew I had guessed the right values for frequent characters and no more debugging was necessary.

## Testing Methodology/Results

[Three test cases](http://ece382.com/labs/lab2/index.html) are provided on the [ECE 382 Website](http://ece382.com). The tests and results are found below.

#### C Functionality

Given an encrypted message of `0xef,0xc3,0xc2,0xcb,0xde,0xcd,0xd8,0xd9,0xc0,0xcd,0xd8,0xc5,0xc3,0xc2,0xdf,0x8d,0x8c,0x8c,0xf5,0xc3,0xd9,0x8c,0xc8,0xc9,0xcf,0xde,0xd5,0xdc,0xd8,0xc9,0xc8,0x8c,0xd8,0xc4,0xc9,0x8c,0xe9,0xef,0xe9,0x9f,0x94,0x9e,0x8c,0xc4,0xc5,0xc8,0xc8,0xc9,0xc2,0x8c,0xc1,0xc9,0xdf,0xdf,0xcd,0xcb,0xc9,0x8c,0xcd,0xc2,0xc8,0x8c,0xcd,0xcf,0xc4,0xc5,0xc9,0xda,0xc9,0xc8,0x8c,0xde,0xc9,0xdd,0xd9,0xc5,0xde,0xc9,0xc8,0x8c,0xca,0xd9,0xc2,0xcf,0xd8,0xc5,0xc3,0xc2,0xcd,0xc0,0xc5,0xd8,0xd5,0x8f` and a key of `0xac`, I received:

![alt text](http://i.imgur.com/yKEjyJe.png "C Functionality")

The decryped message appears to be correct based off of its contents.

#### B Functionality

Given an encrypted message of `0xf8,0xb7,0x46,0x8c,0xb2,0x46,0xdf,0xac,0x42,0xcb,0xba,0x03,0xc7,0xba,0x5a,0x8c,0xb3,0x46,0xc2,0xb8,0x57,0xc4,0xff,0x4a,0xdf,0xff,0x12,0x9a,0xff,0x41,0xc5,0xab,0x50,0x82,0xff,0x03,0xe5,0xab,0x03,0xc3,0xb1,0x4f,0xd5,0xff,0x40,0xc3,0xb1,0x57,0xcd,0xb6,0x4d,0xdf,0xff,0x4f,0xc9,0xab,0x57,0xc9,0xad,0x50,0x80,0xff,0x53,0xc9,0xad,0x4a,0xc3,0xbb,0x50,0x80,0xff,0x42,0xc2,0xbb,0x03,0xdf,0xaf,0x42,0xcf,0xba,0x50,0x8f` and a key of `0xac,0xdf,0x23`, I received:

![alt text](http://i.imgur.com/guQh3yV.png "B Functionality")

The decryped message appears to be correct based off of its contents.

#### A Functionality

![alt text](http://i.imgur.com/qPnxqb5.jpg "TWO wild boxes appear")

![alt text](http://i.imgur.com/UiEswiK.jpg "Stop the madness!")

![alt text](http://i.imgur.com/7zLSPcI.jpg "This could quickly turn into the most self-defeating game of snake")

I feel this answer is correct as it is sensible and composed of only letters, spaces, and periods.

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
