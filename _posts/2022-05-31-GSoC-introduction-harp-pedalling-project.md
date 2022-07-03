---
layout: post
title: "GSoC 2022 - Harp pedalling diagrams"
---
Hello everyone, my name is James and I’ve been fortunate enough to have been chosen to take on one of this year’s projects for Google Summer of Code. I’ll be working on adding a well integrated method of adding and editing harp pedalling diagrams to scores. Harps have seven foot pedals that are used to sharpen or flatten certain strings. The pedalling diagram tells the harpist which pedals they need to press to get the correct pitches for the current passage of music. These diagrams deserve to be a first-class supported feature of MuseScore, instead of being hidden away as a plugin as they so often are in scoring programs. Below is a picture of a diagram, and beneath that an example of its use in a score.

![A harp pedal diagram set to the key of A major](/assets/img/diagram.png)
*Ex 1: A pedalling diagram for the key of A major*

![Excerpt from Debussy's Arabesques with pedalling in diagram and text form](/assets/img/score_example.png)
*Ex 2: An excerpt from Debussy’s Deux Arabesques for Harp with pedal settings shown in diagram and text form.*

The user will drag a pedalling diagram from the palette to the score and then be able to edit it from the inspector. The panel shown below will allow a user to edit pedal settings, with changes appearing on the symbol placed in the score. This will function in the same way as the already excellently integrated fret diagram editor.

![Proposed designs for the panel to edit diagrams](/assets/img/Harp_Pedal_2_0.jpg)
*Ex 3: Proposed designs for the panel users will edit diagrams from*

The diagram will be placed in the score above or below a corresponding note at the beginning of the passage the pedalling is applicable to. An option in the inspector will allow the user to switch between a pedal diagram and pedal text. This will allow the support of single string changes instead of writing the entire pedal configuration out again.

![A single string change](/assets/img/single_string.png)
*Ex 4: Text showing a change to a single pedal*

Hopefully once the diagrams can be added to the score I will have time to add some extra useful features. I would like to include a proofreading feature which will be immediate and automatic, giving the user feedback by highlighting notes that are impossible to play in red, and warning users about notes which may be playable as enharmonics.

![Score showing unplayable notes and enharmonic errors](/assets/img/enharmonic_0.png)
*Ex 5: A#s currently unplayable are highlighted in red and a Db which need respelling in order to be playable is highlighted in gold*

Some other features which may be implemented if I have time include reflecting pedal settings in playback and ensuring transposition affects the diagram as well as the notes.

This project will make an essential part of writing for harps much easier for composers. As a common task for those writing for orchestra, this feature deserves to be supported in the main program. Building this following the existing guitar fretboard code will integrate this feature closely into the program and make it intuitive for existing users. It also offers an opportunity for an automatic harp pedalling feature to be added after the summer is over and the project merged.

Here’s where you’ll be able to find updates as the project progresses:
- [My fork of MuseScore](https://github.com/miiizen/musescore/tree/harp-pedalling)
- [This blog on MuseScore.org](https://musescore.org/en/user/3773138/blog)

I’m thrilled to have been picked for this opportunity and really looking forward to working with the team and wider community this summer!