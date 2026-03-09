# dylaniy one sample innit 💂🏻🇬🇧💂🏻

Ohhh sweet lord I have done it again. Sunday night and it's time to grind out something presentable for class. Let's get into it!

## STARTING OUT

Starting things off I reviewed the 02synths&effects README.md... really stoked on the polyrhythmic potential; was already stoked while making my previous percussion patch.

I wanted to try creating my own sample based on the [strudel documentation on custom samples](https://strudel.cc/learn/samples/#loading-custom-samples)... this process was quite simpler than I was anticipating! THOUGH I WILL CONCEDE that I hastily made a file URL before realizing I could just load samples from my harddrive... oh well!

The sample I'll be working with for now is a very short little click called **Snappy.wav**. I included it in the submission folder if you want to check it out.

Cute!! Ok moving on...

## THE PLAN

*My main goal is to manipulate the sample to imitate different drum sounds.* If I have time, figuring out how to stretch the sample out and create something melodic would also be phenomenal.

I'm going to build drums around this pattern:

```
//load snappy sample from file URL (https://www.image2url.com/file-to-url)
samples(
  {snappy: '1773018568172-b8ab3cad-d7e8-4fed-839f-8051e4046219.wav'},
  'https://image2url.com/r2/default/files/'
);

stack(
  //main motif
  "<[snappy ~ ~ snappy] [snappy ~ ~ ~]>".delay(0.15),
).s()
```

#### I found that I needed to put `"snappy"` within the parentheses of the `.s()` at the end of the `stack()` in order for `note` and `n` to play my sample instead of the default sample (a triangle wave I believe).

## UH OH, TIME TO TRIM AND EDIT THE SAMPLE

```
samples(
  {snappy: '1773018568172-b8ab3cad-d7e8-4fed-839f-8051e4046219.wav'},
  'https://image2url.com/r2/default/files/'
);

stack(
  //main motif
  "<[snappy ~!2 snappy] [snappy ~!3]>".delay(0.15),
  note("c-3")
).s("snappy")
```

When pitching little snappy down, the bit of silence right before the transient becomes ever clearer the lower it gets. This won't do because I want to fashion a kick drum out of the sample and will need to have the playback be more accurate. As it is right now, it's an eighth note behind!! Also it's a stereo snap?? That won't do either!

Simple and quick fix though! Just cut the beginning silence in Live, made it mono, bounced, created a new file URL, and updated the URL in my code:

```
//load snappy sample from file URL (https://www.image2url.com/file-to-url)
samples( // DIFFERENT FILE URL
  {snappy: '1773036634038-22267e60-2ea2-4964-ae2b-cf0be293d4f3.wav'},
  'https://image2url.com/r2/default/files/'
);

stack(
  //main motif
  "<[snappy ~!2 snappy] [snappy ~!3]>".delay(0.15),
  note("c-3")
).s("snappy")
```

Wonderful!! I'm messing around with a "kick" pattern as we speak. I'll need to figure out how to shorten the sample in this context but that should be doable!

## (SPONGEBOB NARRATOR): A FEW HOURS LATER

Ok so... I cooked off camera. **Here's a list to cover roughly everything I did:**
- Added a "kick" sort of low melody:
    ```
  note("<c-4 [~ c-3 [~ b-4] ~]>")
  .gain(0.5)
  .crush(32) // add some texture w lower nums
  .decay(.2)
  .sustain(0.003),
    ```
- Added click "hats" on both L & R channels... both sides will randomly play triplets, 16th notes, or fivelets. They can play the same rhythm in unison or be polyrhythmic:
    ```
  // L hat
    note("[[c5!12]|[c5!16]|[c5!20]|[c5!12]|[c5!12]|[c5!16]|[c5!16]|[c5!20]|[c5!20]|[c5!20]|[c5!16]|[c5!20]|[c5!12]|[c5!12]|[c5!16]]")
     .gain(0.0911)
     .pan(0),
  // R hat
    note("[[c5!12]|[c5!16]|[c5!20]|[c5!20]|[c5!16]|[c5!20]|[c5!12]|[c5!12]|[c5!16]|[c5!12]|[c5!12]|[c5!16]|[c5!16]|[c5!20]|[c5!20]]")
     .gain(0.0911)
     .pan(1),
    ```
- Added a "snare":
    ```
  note("~!2 b-1 ~")
  .gain(0.08)
  .room(0.45),
    ```
- *This addition feels pretty deranged*... I added a little melody by using what is essentially wavetable synthesis via many many many samples within one cycle. The melody changes randomly every cycle:
    ```
  "[snappy!800|snappy!1600|[snappy!400][snappy!800]|[snappy!800][snappy!400]|[snappy!400][snappy!420]|[snappy!800][snappy!840]]"
   .gain(0.00111)
   .room(2),
   ```
   - I definitely would have added more of these and tried to make some harmonies/countermelodies... but adding more than one of these bad babies breaks strudel
   - I'm sure there is a better way to achieve this same effect with less computing power, but for tonight I'm too lazy to find out how.
- **NEW ADDITION** I decided to just go for it and add one more "wavetable synth" part. Just a simple bassline since I figured *less* samples/cycle is better than *more* in terms of performance:
  ```
  "<[[snappy!25] [[snappy!25][snappy!13]] [~] [snappy!53]][[~!12 snappy!13] [[snappy!25][snappy!13]] [[snappy!27][snappy!13]] [~]]>"
   .gain(0.005)
  ```

Here is the final code:
```
//load snappy sample from file URL (https://www.image2url.com/file-to-url)
samples(
  {snappy: '1773036634038-22267e60-2ea2-4964-ae2b-cf0be293d4f3.wav'},
  'https://image2url.com/r2/default/files/'
);
//bpm
setcpm(104/4)

//yeaaaa this is where the magic happens baby 😎 🐸 comment parts in n out as you please (beware..the "synths" kind of break things)
stack(
  //main motif
  "<[snappy ~!2 snappy] [snappy ~!3]>"
   .gain(0.04)
   .delay(0.111)
   .room(0.2),

  //"kick"/very low melody
  note("<c-4 [~ c-3 [~ b-4] ~]>")
   .gain(0.5)
   .crush(32) // add some texture w lower nums (default 32... maybe stay above like 6)
   .attack(0) // woahh (default 0)
   .decay(.2)
   .sustain(0.01),
  
  //hat 1 (panned L) random btw trips, 16ths, and fives
  note("[[c5!12]|[c5!16]|[c5!20]|[c5!12]|[c5!12]|[c5!16]|[c5!16]|[c5!20]|[c5!20]|[c5!20]|[c5!16]|[c5!20]|[c5!12]|[c5!12]|[c5!16]]")
   .gain(0.082)
   .pan(0)
   .room(0.075),
  //hat 2 (panned R) also random btw 3s 4s 5s
  note("[[c5!12]|[c5!16]|[c5!20]|[c5!20]|[c5!16]|[c5!20]|[c5!12]|[c5!12]|[c5!16]|[c5!12]|[c5!12]|[c5!16]|[c5!16]|[c5!20]|[c5!20]]")
   .gain(0.082)
   .pan(1)
   .room(0.075),
  //there is so much within the arrays bc I wanted L&R to be any polyrhythmic combo of the three possible rhythms... yea u get it

  //snare (makes it feel good ash)
  note("~!2 b-1 ~")
   .gain(0.09)
   .room(0.45),

  //"synth" melody (click update ~1/4 note before for downbeat entrance/exit)
  "[snappy!800|snappy!1600|[snappy!400][snappy!800]|[snappy!800][snappy!400]|[snappy!400][snappy!420]|[snappy!800][snappy!840]]"
   .gain(0.00111)
   .room(2),

  //synth bass... mhm
  "<[[snappy!25] [[snappy!25][snappy!13]] [~] [snappy!53]][[~!12 snappy!13] [[snappy!25][snappy!13]] [[snappy!26][snappy!13]] [~]]>"
   .gain(0.0065),
  
).s("snappy")
```
The "performance" of this piece is essentially just commenting, uncommenting, and updating to add or remove one or more parts. Excited to run it in class!

## ONE MORE THING

In my percussion patch submission I was complaining about the jitteriness/lag of estuary. I'm pretty sure using Google Chrome helps mitigate that issue. Ok that's it see ya next time!

# 🧬💜☯️