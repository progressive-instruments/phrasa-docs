##### Hello Synth

```Phrasa
tempo 120bpm
bass~.frequency 440
bass~.end 90%
```

##### Phrasing

```Phrasa
tempo 120bpm
phrases.1 beat
phrases.#.saw-synth~.note 
  1 C3
  2 D3
  3 E3
  4 F3
```

##### Sequencing

```Phrasa
tempo 120bpm
>1 beat
sequences.notes D4,E4,F4
>1-8
  saw-synth~.note (sequences.notes >)
```

##### Reusing Patterns

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
##### Harmony

```Phrasa
tempo 120bpm
pitch.zone F3
>#.pitch.grid
  1 (chord e-min)
  2 (chord F-MAJ)
>1-2
  $pitchs 0,-1,2
  >1-4
    beat
    saw-synth~.pitch ($pitches >)
```
##### Multiple Instruments

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

