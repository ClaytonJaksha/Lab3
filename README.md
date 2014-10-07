Lab 3
====
#### Clayton Jaksha | ECE 382 | Dr. York | M2A

## Objective and Purpose
### Objective



### Purpose



## Preliminary Design



## Code Walkthrough

| Line | `R12` | `R13` | Purpose |
|------|-------|-------|---------|
| 66 | `#NOKIA_DATA` | `0xE7` | Draws 8-bit high beam with 2-bit gap |
| 277 | `#NOKIA_CMD` | `0xB9` | Mask out any weird nibble bits <br> mask in "BO" as the prefix for a page address |
| 298 | `#NOKIA_CMD` | `0x11` | Mask out upper nibble <br> 10 is the prefix for an upper column address |
| 295 | `#NOKIA_CMD` | `0x06` | Set up call for next instruction |

| Line | Command/Data | 8-Bit Packet |
|------|-------|-------|---------|
| 66 |  | `0xE7` |
| 277 |  | `

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

Given an encrypted message of `0x35,0xdf,0x00,0xca,0x5d,0x9e,0x3d,0xdb,0x12,0xca,0x5d,0x9e,0x32,0xc8,0x16,0xcc,0x12,0xd9,0x16,0x90,0x53,0xf8,0x01,0xd7,0x16,0xd0,0x17,0xd2,0x0a,0x90,0x53,0xf9,0x1c,0xd1,0x17,0x90,0x53,0xf9,0x1c,0xd1,0x17,0x90` and no key. I decrypted it to be the following:

![alt text](http://i.imgur.com/KsmQd8K.png "A Functionality")

I feel this answer is correct as it is sensible and composed of only letters, spaces, and periods.

## Observations and Conclusion
#### Observations

* Subroutine calls require the immediate addressing of the label they are trying to call, otherwise everything will get messed up.
* The xor encryption is a very simple encryption; decrypting more challenging message without knowledge of its encryption process or its key would be far more challenging.
* The testing for this code could have taken <i>far</i> longer than it actually did. There were (1+1+26+26)^2 possible different guesses. Having some sense of where bytes frequently appear is helpful in reducing the amount of tries it takes to correctly guess it.

#### Conclusion

The objective of this lab was to design code that would take a message, a key (or none), and return a decrypted message in memory. I was able to meet these requirements to their fullest required extent.

## Documentation

I referenced the follwoing [ASCII table](http://benborowiec.com/wp-content/uploads/2011/07/better_ascii_table.jpg) in the planning and development of this code.

![alt text](http://benborowiec.com/wp-content/uploads/2011/07/better_ascii_table.jpg "ASCI
