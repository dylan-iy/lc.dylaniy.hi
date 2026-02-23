# dylaniy lc percussion patch 👽🥁🎶

**IMPORTANT**: SET ESTUARY TEMPO TO 100 (`!setbpm 100`) AND VIEW TO 2x2 (`!presetview twobytwo`) BEFORE PASTING/RUNNING CODE 

## 2/22 6:37a
Took time to familiarize myself with the basic concepts of minitidal and implementation in  in estuary terminal. Picked a percussion loop to recreate ([1:49 of We're Dumb by salami rose joe louis](https://salamirosejoelouismusic.bandcamp.com/track/we-re-dumb)).

Figured out how to exit `!localview audiomap` via `!presetview twobytwo`. Shoutout Fero Király for his concise [terminal documentation](https://www.ferokiraly.com/estuary/en/docs/estuary/terminal/).

## 2/23 1:09a
Stem separated We're Dumb to solo out drums. Figured out BPM of loop and began replicating starting with kick. I'm sure there's a way to simplify the statment for the kick pattern but I am in a hurry and am just gonna go with what I know works until it doesn't:  
`s "bd ~ bd ~ bd ~!4 bd ~ bd"`

I'll be using `stack` to contain each individual drum part as the sequencing is too complex for me to layer in one `s` statement cleanly.

Clap and backbeat hat were straightforward: `s "~!2 [~ cp] ~"` and `s "~ hh"` respectively.

The shaker part (using just another `hh` for now) is complicated. After slowing it down to hear what it's doing I'm realizing I misheard the subdivision in the kick part... ARGH! Gonna have to rewrite the kick drum statement to get it right. There is 1000000% a more elegant way to write this out, but here's what I came up with:  
`s "[bd [~ bd ~!2]] [[~ bd] ~] ~ [bd [~ bd ~!2]]"`


### Small aside to explain my thought process
Essentially the system for part writing I've come up with is as follows:
1. Divide the bar long pulse into 4 beats (`"~ ~ ~ ~"` or `"~!4"`)
2. Nest additional subdivisions using `[]` square brackets as needed  
    ex. 4/4 triplet subdivision: `"[~ ~ ~] [~ ~ ~] [~ ~ ~] [~ ~ ~]"` or `"[~ ~ ~]!4"`

## It is now 2:30a 🥱
Using this method I tackled the side snare part (rim click sound in the OG drum beat) with the following statement:  
`s "[~ sd ~ [~ sd]] [[~ sd] ~!3] [~ sd] ~" # gain 0.8`

Preemptively adjusted the gain of every other pattern before realizing I have 1-2 more parts to transcribe. Your time of reckoning is here, shaker part!!  
`s "[hh!2 [~ hh]!2] [hh!3 [hh!2]] ~ [hh!3 ~]" # gain 0.7`  

### We're getting there!
As it stands this is the code for the patch **without any sample indexing**:

    stack [
      --kick
       s "[bd [~ bd ~!2]] [[~ bd] ~] ~ [bd [~ bd ~!2]]" # gain 0.87,

      --clap
       s "~!2 [~ cp] ~" # gain 0.8,

      --backbeat hat
       s "~ hh" # gain 0.9,

      --side snare
       s "[~ sd ~ [~ sd]] [[~ sd] ~!3] [~ sd] ~" # gain 0.62,

      --shaker/hat
       s "[hh!2 [~ hh]!2] [hh!3 [hh!2]] ~ [hh!3 ~]" # gain 0.83
    ]

That being said, I think I can make the patch just a little cooler + more authentic to the original if I choose different indices for the side snare & backbeat hat patterns...

After messing with index options for both, I opted to only change the backbeat hat to a more open hat/bell jingle (`hh:8`). I also added a little horn stab every 2 cycles using `hh:4` to imitate a sfx from the groove in the song.  
**Here is the final patch code**:

    	



I'm including audio clips of the original drums and the estuary patch, both by themselves and alongside the rest of the instrumental. Pretty happy with the results!

### SOMETHING BOTHERED ME AS I WORKED ON THIS PATCH

The occasional to frequent output jitter/lag gets in the way of an accurate performance of the patch. Not sure if it was because of my internet or if this is typical of estuary but it killed the vibe ever so slightly. Had a lot of fun with this otherwise!! I've been meaning to transcribe that groove for a long time and the syncopation/polyrhythm friendly minitidal was a fantastic vehicle to do so.  
Until next time!
## 🕉️♈💜🌚