What is a Turing machine?![[FundamentalsCS/TuringMachine/TuringMachine-images/GetImage.png]]

Elements in Turing machine:
![[FundamentalsCS/TuringMachine/TuringMachine-images/GetImage (1).png]]

![[FundamentalsCS/TuringMachine/TuringMachine-images/GetImage (2).png]]

-basically, a Turing machine is like a finite automation that reads input and traverses to different states. However, the input it reads is on a tape. Each slot of the tape contains a single character. When it reads a input character, it traverses to different state or remain in the current state. Besides that, it also output a character to the read tape slot and a direction to tell which slot to read next. Therefore, there is two major components in Turing machine: the finite automation and the tape.

Example with a picture(b with an arrow pointing is the tape head, q1 is the start state.)
![[FundamentalsCS/TuringMachine/TuringMachine-images/GetImage (3).png]]
![[FundamentalsCS/TuringMachine/TuringMachine-images/GetImage (4).png]]

Differences between Turing machine and Deterministic finite automation 

-Turing machine may not terminate when the input is completely processed(it can still run when reading a blank character input) 

-Turing machine may process the input several times(it may go left and right on the tape for multiple times) 

-Turing machine terminates once it touches the accepting or rejecting states(it cares not if there is input left on the tape, which is quite the opposite of DFA) 

-Turing machine may enter an infinite loop. 

-Turing machine can manipulate input(it can write to the tape) 

-Turing machine is deterministic(one unique start state && exactly one transition for each character on one state, like a deterministic finite automation)

Language of a Turing machine
![[FundamentalsCS/TuringMachine/TuringMachine-images/GetImage (5).png]]

-If a language is accepted by a Turing machine, this language is called recognizable, and the machine is called the recognizer of the language. 

-Recursively Enumerable(RE) is a class of all recognizable languages.
![[FundamentalsCS/TuringMachine/TuringMachine-images/GetImage (6).png]]
-R is a class of all decidable languages. 

-Because every decider is a Turing machine, R is a subset of RE. 

The relationship between different languages
![[FundamentalsCS/TuringMachine/TuringMachine-images/GetImage (7).png]]
-RLs means regular languages which can be accepted/expressed by DFA or regular expression. 

-CFLs means context-free languages which can be expressed by context-free grammar. 

-R is a class of all decidable languages which can be accepted by a decider(a Turing machine that can only either accept or reject). 

-Recognizable languages/recursively enumerable(RE) is a class of all recognizable languages which can be accepted by a Turing machine(can accept, reject, or enter infinite loop).