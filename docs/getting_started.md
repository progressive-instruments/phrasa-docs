# Getting Started

This getting started session is intended to demonstrate what you can do with Phrasa, without getting too deep into syntax and terms. If you feel confused at some part, don't worry, In the next 'Concepts' section we will start right at the beginning.

This guide presumes you have a minimal understanding about music and sound. If you are unfamiliar with terms like frequency, note or tempo you might want to begin with <u>Phrasa Music & Sound Cheatsheet</u>, or jump to it at any time you feel.



## Installation

Before we begin, you can download and install Phrasa Control from [here](https://github.com/progressive-instruments/phrasa-control/releases/download/v0.1.0/phrasa-control-v0.1.0-win64.zip).

<u>Note, this is a very limited, experimental and unmatured version of Phrasa Control - expect great things to come in the very near future.</u>



## Hello Sound

Ok, let's play the first sound.

Open 'Phrasa Control' and type the following text:
``` phrasa linenums="1"
tempo 102bpm
beat
lead.event
  frequency 440
  end 50%
```
We defined and played the most basic Phrasa structure - a single musical phrase containing a single event playing repeatedly.

The first expression `tempo 102bpm` sets `tempo` to `102bpm`.

The second line `beat` will make more sense in the next section.

Then we have an expression that assign a musical `event` to the instrument by the name `lead` with the following properties:
* Frequency of 440 hertz
* End time of 50% the phrase length.

To short things up we can replace the expression subject `lead.event` with `lead~`



## Phrasing

Let's make things slightly more interesting, adding some notes to be played:

``` phrasa linenums="1"
tempo 135bpm
phrases.1.lead~.note C3
phrases.2.lead~.note D3
phrases.3.lead~.note G3
phrases.4.lead~.note F3

phrases.1 beat
```

In lines 2-5, we divide the piece into 4 musical phrases, each contains an event to be sent to the instrument`lead` with the property `note`. This is how our piece is divided:

<img src="img/phrasing.png" style="zoom:85%;" />

A 'phrase' is a fixed time frame where events can occur. Every phrase can be divided into inner phrases.

To make the code above a bit more elegant we can use the mighty selector symbol - #:

``` phrasa linenums="1"
phrases.#.lead~.note
  1 C3
  2 D3
  3 G3
  4 F4
```

The final expression `phrases.1 beat` defines the first phrase as the beat length of the piece (see picture above). This basically means that tempo (in this case 135 beats per minute) will be relative to the duration of this phrase.

Finally again, let's make things  shorter by replacing the expressions `pharses.X` with `>X`, for example:
`>1 beat`. 



## Harmony

The 1st Phrasa super power is <span style="color:blueviolet;font-size:120%">**<u>Relativity</u>**</span>. 

In this example, instead of writing down the actual note as we did before, we can write the offset in relation to its harmonic context:

``` phrasa linenums="1"
tempo 135bpm
>1 beat

pitch.grid (chord b-min)
pitch.zone b3

>total 8
>#.lead~.pitch
  1 3
  4 4
  6 -3
  7 0
```

Lines 4-5 define the harmonic context.

`pitch.grid` defines the set of notes, in this case 'B minor' chord in all octaves.

`pitch.zone` defines the initial position within the grid. In this case, the note 'B' in the 3rd octave.

The expression `>total 8` sets the total number of phrases to 8 (if this property is not set, total phrases will be equal to the last phrase assigned)

In lines 9-13 we set events for phrases 1,4,6 and 7.

The value of `pitch` defines the offset within the previously defined harmonic context.



## Reusing Patterns

The 2nd Phrasa super power is <span style="color:blueviolet;font-size:120%">**<u>Reusability</u>**</span>.

Repetition is probably the most notable element of music. It's right there within the physical nature of every periodic sound. Music without repetition is just random sound, or more technically - noise. 

In Phrasa you can define multiple phrases in a single expression: `phrases.x-y` or `phrases.x,y`, and then make variations over them:

``` phrasa linenums="1"
tempo 127bpm

pitch
  grid (chord c-maj)
  zone g4
  
>1-4
  >1 beat
  >1-4.>#.lead~.pitch 
    2 0
    4 1
  >#.>3.lead~.pitch
    1 -1
    2 2

>3-4.pitch.grid (chord g-maj)
```

By setting multiple phrases collectively, we are keeping all their shared properties in one place. This will make our piece much more flexible.

Here is a diagram that illustrates the resulted phrases and and events:

![](img/reusability_example.png)

Imagine how hard you had to work writing these events one by one, and even harder if you wanted to make a change. This is the power of reusability.




## Sequencing

Until now we have played with musical elements based on the concepts of hierarchy and repetition. Now let's play with another fundamental concept of perceiving time - **continuity**.

For this we have our old pal - the sequencer:

``` phrasa linenums="1"
tempo 125bpm

pitch
  grid (scale g-maj)
  zone g3
  
sequences.ascending 1,3,4,5,7
>1-4
  beat
  >1-4.lead~.pitch (sequences.ascending >)

>4.pitch.grid (scale d-maj)
```

In line 7 we defines a sequence of values by the name `ascending`. 

In line 10 we assign the expression `(sequences.ascending >)` to the the property `pitch`. Each of the events assigned to this expression, will increment the sequence position by one (as indicated by the symbol `>`) and use the current value of the sequence.

Here is an illustration of the outcome:![](img/sequencing_example.png)

The subject `sequences.name` can be shorten to `$name`.



## Multiple Instruments

<span style="color:red">**<u>Not supported</u>**</span>.

So we had all these cool stuff with a single instrument, so let's branch out and have multiple instruments playing together. 

Also, to make our code more readable and manageable we're gonna write down the piece in multiple files:

``` phrasa linenums="1"
tempo 130bpm
pitch
  grid (scale g-maj)
  zone g3

use drums
use bass
```
`cool.piece`



``` phrasa linenums="1"
branches.cymbals
  >1-8
    >#.drums~.sample
      1 cym1
      2 cym2
branches.kicks
  >total 16
  >1 beat
  >1,5,8,14.drums~
    sample kick
  >5,8.drums~
    sample snare
```

`drums.motif`



``` phrasa linenums="1"
branches.bass
  $ascending 1,3,4,5,7
  >1-4
    beat
    >1-4.lead~.pitch ($ascending +)

  >4.pitch.grid (scale d-maj)
```

`bass.motif`



The main file of this piece is `cool.piece` which defines the base pitch and tempo.

The `use` expressions import the entire content of an external .motif file. 

In the motif files we created 3 branches: `cymbals`, `kicks` and `bass`. A branch is a new phrase derived from the context of it's parent phrase, taking it's own path with it's own inner phrases, sequences and other things. It gives you the freedom to create multiple parallel structures and define events within each of them.

