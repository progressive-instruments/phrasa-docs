# Getting Started

This getting started session is intended to give you an overview over the capabilities of Phrasa. If you feel confused at some part, don't worry, In the next 'Concepts' section we will start right at the beginning.

### Hello Sound

Ok, let's play our first sound.

Open 'Phrasa Control', type the following text:
``` phrasa
tempo 102bpm
beat
lead.event
  frequency 440
  end 50%
```
We defined and played the most basic Phrasa structure - a single musical phrase containing a single event playing repeatedly.

Let's open up the text.

the first expression is `tempo 120bpm`. which assigns the value `120bpm` to the subject `tempo`. The subject and the value are separated with space.

Then we have this expression: 
``` python
lead.event
  frequency 440
  end 50%
```
which sets an 'event' to the instrument named 'lead' with the following properties:
* Frequency of 440 hertz
* End time of 50% the phrase length.

Here, the subject 'lead.event' is separated from the values (`frequency 440` and `end 50%`) by multiple lines written in a higher indentation level from the subject. 

To short things up we can replace the expression subject 'lead.event' with 'lead~'

### Phrasing

Let's make things slightly more interesting, let's add some more notes to be played:

``` phrasa
tempo 135bpm
phrases.1.lead~.note C3
phrases.2.lead~.note D3
phrases.3.lead~.note G3
phrases.4.lead~.note F3

phrases.1 beat
```

In lines 2-5, we define 4 musical phrases, each contains a 'lead' event with a note property. 
A 'phrase' in Phrasa is a fixed time frame where events can occur. Each phrase can have inner 'phrases' which will be ordered one after the other.

Instead of repeating the subject path 'phrases.x.lead~.note' we can use the incredible selector symbol '#':

This way we are selecting the phrase number we wish to define in the rows that follow the subject path.

the final expression `phrases.1 beat` defines the first phrase as the beat of the piece. This basically means that the tempo (120bpm) will be referred to it.

Lastly we can make it shorter by replacing every `pharses.X` with `>X`, for example:
`>1 beat`



### Harmony

The 1st power of Phrasa is within it's relative nature. Instead of working hard and repeating yourself, you define things more generally, and then set events in relation to its phrase context.
Let's explore this with pitch and rewrite our previous code:

``` phrasa
tempo 135bpm
>1 beat

pitch
  grid (chord b-min)
  zone b3

>total 8
>#.lead~.event.pitch
  1 3
  4 4
  6 -3
  7 0
```

In line 4-6 we define the general pitch for our root phrase.
The pitch `grid` defines the set of notes to be played, in this case the notes of the 'B minor' chord in every octave.
The pitch `zone` defines the pitch relative position over the frequency range. In this case, pitch will be relative to b3 - the note 'B' in the 3rd octave.

The expression `>total 8` is setting the number of inner phrases to 8. If this property is not set, the last phrase that was assigned is the last phrase.  

In lines 9-13 we are setting events for phrases 1,4,6 and 7.
We are setting now the property `pitch` which will be relative to the pitch grid and zone that were defined.
The value of the pitch is the relative offset from the pitch zone within the pitch grid that are defined in the phrase context.
For this example '0' will be the closest note to b3 within the defined grid, which is b3. '1' is the note after it and '-2' is 2 notes before it

### Reusing Patterns

Repetition is probably the most notable element in the experience of music. Its right there within the physical nature of every periodic sound. Without repetition, music is just random sound, aka noise. 
Phrasa allows you to reuse musical elements within your piece and variate them. It's all about the phrases:
**(work on this)**

``` phrasa
tempo 131bpm

pitch
  grid (scale g-maj)
  zone g3
  
>1.>1 beat
>1-4
  >1-4.>#.lead~.pitch 
    1 -1
    2 0
    4 1
  >#.>3.bass~.pitch
    1 0
    2 1
>4.pitch
  grid (scale g-maj)
  zone a2
```

The `>1-4` expression is used to define the phrases 1,2,3 and 4 with the same properties. Each phrase consists of 16 inner phrases, and some are assigned with events.


### Sequencing

Until now we played with musical elements that are based on the concepts of hierarchy and repetition. Another essential musical element is events sequencing - events that follow one another in a sequence:

``` phrasa
tempo 125bpm

pitch
  grid (scale g-maj)
  zone g3
  
sequences.ascending 1,3,4,5,7
>1-4
  beat
  >1-4.lead~.pitch (sequences.ascending +)

>4.pitch.grid (scale d-maj)
```

In the expression `sequences.ascending 1,3,4,5,7`  we define a sequence by the name 'ascending' with some values. 

In line 10 we assign the value `(sequences.ascending >)` to the pitch of all events. Each of the events defined with this expression, will get the next value (specified by the symbol '+') of the sequence.

### Multiple Instruments

The last piece of the puzzle is having multiple instruments, playing together. For this we are going to use 'branches'. Also to make it more readable and manageable we're gonna write down the piece in multiple files:

`cool.piece`

``` phrasa
tempo 130bpm
pitch
  grid (scale g-maj)
  zone g3

use drums
use bass
```
`drums.motif`

``` phrasa
branches.cymbles
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

`bass.motif`

``` phrasa
branches.bass
  sequences.ascending 1,3,4,5,7
  >each
    beat
    >each.lead~.pitch (sequences.ascending +)

  >4.pitch.grid (scale d-maj)
```



Our final .piece file is now defining only the base pitch and the tempo.

The `use` expressions are used to import the entire content of an external .motif file. 

In the motif files we created 3 branches named 'cymbles', 'kicks' and 'bass'. A branch is simply a new phrase derived from the context of it's parent phrase, taking it's own path with it's own inner phrases, sequences and other things. It allows you to set events to be run in parallel to it's parent phrase.

