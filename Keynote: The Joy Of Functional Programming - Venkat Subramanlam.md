## KEYNOTE: THE JOY OF FUNCTIONAL PROGRAMMING
_Venkat Subramaniam Monday, June 22, 10:00AM - 11:15AM (Ballroom A, 3rd Floor)_

### Intro
* We think a lot about programming
* We don't leave things at work
* We are part of a community -- we are all interconnected

### Mainstream
* What does it mean?  It's what people around you use, what's common.
    * Conventional wisdom has been wrong many many times
* Example: discovering gravity
    * Many people were wrong along the way
    * It took a long time to learn -- that's perfectly okay
* Example: center of the solar system
    * Sun being the center wasn't *mainstream*
    * It was dangerous to diverse from that mainstream thinking (Galileo was imprisoned)
* Example: health/childbirth
    * Hospital death rate for childbirth was 3x than using midwife
    * Doctors didn't know about transferring germs from patent to patient
    * Washing hands wasn't *mainstream*
    * Still isn't at most airports ;)
    * Took several decades for handwashing to go mainstream
* Example: declaration of independence says all men equal
    * Slavery was *mainstream*
    * Took a century to remove slavery from mainstream, didn't fix equality
    * Rosa Parks came along
    * Suffrage of women came along
    * Took a lot of time for change to mainstream to happen
* What's wrong with people?
* Mainstream really sucks

### Computer Science & Mainstream
* Largely procedural at the beginning
    * Dijkstra suggested avoiding using GOTO's
* Structural programming became mainstream
    * Very difficult to scale structural programming
* Object oriented programming
    * When did it become mainstream? 1990
    * When was it created? 1967
    * **23 years for OOP to become mainstream**
* Functional programming
    * When was functional programing first considered? 1929?
    * When did functional programming become mainstream? Not yet
* Alan Kay didn't have C++ in mind when OOP came along
* *Some ideas look good at first, but aren't over time*
* What do we do at work?
    * Write code, fix bugs, write code, fix bugs... forever
    * That's *mainstream*
    * Grace Hopper: Most dangerous phrase is "We've always done it this way"

### Functional Programming
* Alonzo Church - lamba calculus, Alan Turing's professor
* John McCarthy - first language was a functional programming language
* Pure functions
    * If the input stays the same, output is always the same
    * Output is always predictable, much easier to test
    * Much easier to understand code that is **pure**
    * Fewer errors and bugs
* Benefit: Referential transparency
    * Compiler can optimize code by replacing expression with the value of the expression
    * Utilizing cache
    * 123 + 14 = 137 (maybe a little slow the first time, but if you ask me a second time, I respond quickly)
    * Example of a pure function ^^
    * Compilers can easily reorder pure functions to get the best caching, performance, etc
* Imperative style
    * It's *mainstream*
    * Nobody likes flags -- we call them horrible things like "temp"
    * Why is it so bad?
    * We tell the what and how -- primitive obsession
    * *Refer to the finding nemo code example from the session video*
* Declarative style
    * Tell **what** we want but don't tell **how** to do it
    * Let the underlying library figure out how to do it best
    * You write less code, usually means less bugs
    * *Refer to doubling list code example from video*
    * If you're creating empty arrays, first sign of danger
    * Mutating variables over and over is painful
    * Use things like .map and .collect (from Java example) rather than looping over the array and adjusting it
    * **Don't torture your variables**
* Imperative style is like working with a toddler
    * Fun the first day
    * Second day is the same as the first
    * Continues for the next 18 years
    * You yearn for a conversation with an adult
* Functional style is like working with a responsible adult
    * Communicating with something more intelligent
    * More joyful, doesn't drag you down
    * Be declarative
    * Small change in syntax, big change in semantics
* Imperative style is opaque, can hide errors
    * *Refer to list of numbers code example from session video*
    * Use .filter, .map, and .findFirst (java) instead of looping
    * Should be able to read code top-down, no oscillating up and down
    * Read top-down in a single pass and explain it, code reads like the problem statement
    * Very transparent, expressive
    * Can read the code like a sentence
* Efficiency?
    * Declarative programming usually makes fewer system calls
    * Computer will try its best to do the least amount of work
    * Don't make the computer do the work if the result is never used (lazy evaluation)
    * Efficiency of computation

### Entering Mainstream
* *Imperative code is familiar, declarative code is simpler*
* Eliminate the moving parts
    * *Refer to session video for code example*
    * Reading code should be a story, not a puzzle
* Writing functional code feels like we're getting something done
    * Code becomes expressive
    * Keeps you focused on problem you're solving
    * Easier for colleagues to understand
    * Transparent
    * Easier to test
    * Easy to optimize
    * Easier to make concurrent
* That's where the joy comes from ^^
* **Biggest change in the programming world is in the minds of programmers**
