---
title: GSoC 2022 with MuseScore - Harp Pedalling Diagrams
link: https://github.com/musescore/MuseScore/pull/12269
date: 2022-09-12
layout: project
---

This post will sum up all of the additions and changes I have made to MuseScore for 2022’s Google Summer of Code [harp pedalling project](https://musescore.org/en/user/3773138/blog/2022/06/01/gsoc-2022-harp-pedalling-diagrams).
The previous blog posts with weekly updates can be found [here](https://musescore.org/en/user/3773138/blog).

As the team are working towards the release of MuseScore 4.0, this code will only be merged after its release and be included in a 4.x update.  

![Screenshot showing harp pedalling UI and diagrams in a score with proofreading showing unplayable notes in red](/assets/img/gsoc-final.PNG)
*Ex 1: Screenshot showing harp pedalling UI and diagrams in a score with proofreading showing unplayable notes in red*

## Code changes

* [Pull requests submitted as part of this project](https://github.com/musescore/MuseScore/pulls/miiizen?q=created:2022-05-20..2022-09-12)

PR [#12269](https://github.com/musescore/MuseScore/pull/12269) contains feedback for changes made to the engraving and palette modules but was abandoned in favour of only maintaining one PR.  PR [#12597](https://github.com/musescore/MuseScore/pull/12597) will be merged eventually.  This contains the changes in #12269 as well as changes in the notation module to implement the UI. A summary of the changes are below:

**Pull Request #[12597](https://github.com/musescore/MuseScore/pull/12597) Harp pedalling diagrams and UI**

* 2022-09-09 [dd05785](https://github.com/musescore/MuseScore/pull/12597/commits/dd057854908059f2b8c09bb06d4d0d5f4b844018) Created base class for harp pedal diagram
* 2022-09-09 [97eee81](https://github.com/musescore/MuseScore/pull/12597/commits/97eee81d8b48a86e1f54f4ea9810ff181bf1d34f) Add palette for harp notation
* 2022-09-09 [3a591ba](https://github.com/musescore/MuseScore/pull/12597/commits/3a591ba47c56f047547a5744dac8478f47d19a18) Created pedal settings element with diagram and text variations. Changes made for undo, text styles, score interaction.
* 2022-09-09 [42d3960](https://github.com/musescore/MuseScore/pull/12597/commits/42d3960dc9322eae2ea65340f745e254c2cd52a0) Element popups viewable in the score
* 2022-09-09 [746482d](https://github.com/musescore/MuseScore/pull/12597/commits/746482dccbab805b8aa23ebd188ae7610d71c1cf)<span style="text-decoration:underline;"> </span>Created pedalling UI. This includes necessary changes to HarpPedalDiagram class and integration with the undo stack
* 2022-09-09 [b9c9d9a](https://github.com/musescore/MuseScore/pull/12597/commits/b9c9d9af8a45dd725b0a5447fe8be5c75f0f8cff) Added proofreading via notehead colours
* 2022-09-09 [ea6326d](https://github.com/musescore/MuseScore/pull/12597/commits/ea6326d5e4164ab6ba80d2188d0bdb50d020d3e6) Create utests and vtest


## Evaluation
The project has met the goals I outlined in my project proposal - diagrams can be added to the score and edited with a new UI, and immediate automatic proofreading is provided.  This is a much better integrated and user friendly experience compared to previous versions, where this had to be done through a plugin.  

## Future work
The code will not be included until after the release of 4.0, so I will have to maintain the PR until then to ensure it remains mergeable.  When it is merged and more people see and use the feature, more bugs may appear so I will stay on hand to help fix those.
I’d like to add glissando playback which will need to be done after some structural work is done on the playback system.  This is connected to another product, Muse Sounds which is being developed in house and is unreleased.  After release, changes can be made to the glissando playback system with a lower likelihood of having to repeat this work in the future.  