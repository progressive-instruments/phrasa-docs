This quickstart guide is intended to give you an overview over the capabilities of Phrasa. If you feel confused at some part, don't worry, In the next 'Concepts' section we will start right at the beginning.

#### Hello Sound

So let's stop talking and play some sounds.

Open 'Phrasa Control', type the following text and press on 'space'
``` Phrasa
tempo 102bpm
saw-synth.event
  frequency 440
    end 50%
```
We defined and played the most basic Phrasa structure - a single musical phrase containing a single event playing repeatedly.

Let's open up the text.

the first expression is `tempo 120bpm`. which assigns the value '120bpm' to the subject 'tempo'. The subject and the value are seperated with space.

Then we have this expression: 
```
saw-synth.event
  frequency 440
  end 50%
```
which sets an 'event' to the instrument named 'saw-synth' with the following properties:
* Frequency of 440 hertz
* End time of 50% the phrase length.

Here, the subject 'saw-synth.event' is separated from the values (`frequency 440` and `end 50%`) by multiple lines written in a higher indentation level from the subject. 

To short things up we can replace the expression subject 'saw-synth.event' with 'saw-synth~'

#### Phrasing

Let's make things slightly more interesting, let's add some more notes to be played:

``` Phrasa
tempo 135bpm
phrases.1.saw-synth~.note C3
phrases.2.saw-synth~.note D3
phrases.3.saw-synth~.note G3
phrases.4.saw-synth~.note F3

phrases.1 beat
```

In lines 2-5, we define 4 musical phrases, each contains a 'saw-synth' event with a note property. 
A 'phrase' in Phrasa is a fixed time frame where events can occur. Each phrase can have inner 'phrases' which will be ordered one after the other.

Instead of repeating the subject path 'phrases.x.saw-synth~.note' we can use the incredible selector symbol '#':

This way we are selecting the phrase number we wish to define in the rows that follow the subject path.

the final expression `phrases.1 beat` defines the first phrase as the beat of the piece. This basically means that the tempo (120bpm) will be referred to it.

Lastly we can make it shorter by replacing every `pharses.X` with `>X`, for example:
`>1 beat`



#### Harmony

The power of Phrasa is within it's relative nature. Instead of working hard and repeating yourself, you define things more generally, and then set events in relation to its phrase context.
Let's explore this with pitch and rewrite our previous code:

``` Phrasa
tempo 135bpm
>1 beat

pitch
  grid (chord b-min)
  zone b3

>total 8
>#.saw-synth~.event.pitch
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

#### Reusing Patterns

```Phrasa
tempo 120bpm
>1-4
  >#.$notes 
    1 D4,E4,F4
    2 F4,E4,G4
  >1-2.>1-4
    beat
    saw-synth~.note ($notes >)
>4.>2.>total 3
```

#### Sequencing

```Phrasa
tempo 120bpm
>1 beat
sequences.notes D4,E4,F4
>1-8
  saw-synth~.note (sequences.notes >)
```

#### Multiple Instruments

`cool.piece`

```Phrasa
tempo 120bpm
pitch.zone F3
>1.pitch.grid (chord e-min)
>2.pitch.grid (chord F-MAJ)
>1-2
  branches.synth
    $pitches 0,-1,+2
    >1-4
      beat
      saw-synth~ 
        pitch ($pitches >)
        end 90%
  use <drums>
```
`drums.motif`

```Phrasa
>1-2
  &k.>#.kick~
    1 100%
    2 90%
    3 90%
  &sn.>2.snare~ 100%
  &cym.>2.>#.drums~ 
      1 cym1
      2 cym2
      4 cym1
2.&kick.>each
  kick~.start -25% 
```

