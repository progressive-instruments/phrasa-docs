# Concepts



## Phrases

In traditional music theory, a phrase is a semi-abstract term that defines a period of time within a piece that can be perceived by the listener as a whole unit.
Phrasa build upon the traditional definition and extend into a much more abstract form.
In Phrasa, a musical piece is by itself a single phrase, that is made out of multiple phrases that are made from more phrases and so on.

## Ratios

Phrases durations are perceived in relation to each other. The relative durations of the phrases helps the listener to keep up with the piece, and build upon it's expectations. In phrasa we describe the length of a phrase as a ratio with relation to it's father phrase. By defining  with length property of a phrase is defined relatively to its parent, for example '/4' or 'X2'.
If the phrase contains inner phrases, the actual length will be the sum of them all multiiplied by it's own length.
For example, if a phrase with a length of 1, contains 4 phrases of length 1/4, the final length will be 1/4 + 1/4 + 1/4 + 1/4 = 1.
If the same phrase contains 5 phrases of length 1/4 it's final length will be 5/4.
A phrase with the length of 2, containing 3 phrases of 1/4, will end up with the length of 3/4 X 2 = 6/4



## Phrasa Expression

Everything in Phrasa is either a value, a list of values or an expression.
A value is defined in a single word without spaces. for example `beat`, `140bpm` or `1/4`.
A list of values are multiple values seperated with , or a newline within the same indentation.
For example `1, 2.4235, 3/6` or:

```
snare
kick
drum
```

arrays within arrays can be defined:

```
23,(10,20),33
```

or

```
23
10,20
33
```

An expression can be though of as a simple sentance consisting of 2 parts, a subject and an object. They can either be separated with spaces, for example:

```Phrasa
tempo 120bpm
```

or indented in a new line:

```Phrasa
tempo
  120bpm
```

This expression sets the tempo (the subject) to 120 bpm (the object)
The expression input can either be a value, such as above, or it can be a list of values:
cool_sequence 10,20,10
or:

```Phrasa
cool_sequence
  10
  20
  30
```

expression input can also be an expression by itself, defined in a single row by encosing with paranthesis, for example

```Phrasa
cool_event.offset (random 10%-40%)or in a new line:cool_event.offset    random 10%-40%
```

or:

```Phrasa
cool_event.offset
  random
    10%-40%
```

And of course it can also be a list of expression/values, for example:

```Phrasa
phrases.1
  pitch.grid (chord Maj-C) // an expression which defines the pitch in the 1st phrase to be around the C Major chord.
  beat // a value that defines the 1st phrase as the beat phrase
```

The most common type is the assignment expression which are at the core of Phrasa, functioning as instructions for updating the piece Tree.
For example 

```Phrasa
pitch.zone F3
```

Which is an assignment expression for setting the property zone within the property pitch with the note value of F3.
An expression type 

