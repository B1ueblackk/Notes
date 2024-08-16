# Lec1

course assessment 

* 2 quizzes, 40% each(short questions & T/F)
* mini project 20% (23 Oct 10min per team of 2, 8 for pre,2 for QA)
* 11 Sep -- quiz1 / 16 Oct -- quiz2

## Computer Security

7 keys:

* Authentication
* Authorization
* Confidentiality
* Data/Message integrity
* Accountability
* Availability
* Non-repudiation

### Caesar Cipher Decryption

shift by a fixed distance, like A -> C, B -> D...

the scrambled key is fixed, so it is easy to encrypt the message

**key space must be large**

### General Case

not necessarily a shift of the alphabet

but this kind of encryption has its leak, we can calculate the frequency of the appearance of every letter and compare it with the real alphabet's frequency list

main weakness --  letters used in English are very unevenly distributed

**so do not use substitute cipher**

### Vigenere Cipher

this method using a word to be the key. For example, 'DUH', all plaintext will add 3, 20, 7 repeated pattern.

in this method, same letter can be mapped to different letters, so we call this poly-alphabetic substitution

Although Vigenere Cipher > Caesar cipher, it's still easy to break

1. figure out the key's length,  THEYDRINKTHETEA -> DUH -> **WBL**BXYLHR**WBL**WYH 
2. notice that WBL appears twice at nine-letter intervals
3. deduce the key's length from 9 to the number that divides nine -- 3
4. they may guess the WBL may represent 'THE'

so longer keywords implies stronger Vigenere Cipher, short message implies stronger Vigenere Cipher

### One-Time Pad

**perfect encryption**

that means the keys' length is the same as the length of plaintext, and the key is random and can only use one time

here is the problem -- 

* how to generate truly random LONG one-time pad?
* how to store OTP securely
* how to encrypt and decrypt securely
* both parties have to keep in synchronization portions of pad that has already been used, so both parties can keep on talking
* if old OTP is used up or compromised, how to agree on new OTP?

### Randomness

there is no real random digits, the real random thing is that the algorithm or the process, once the digits generated, it would no longer be random.



