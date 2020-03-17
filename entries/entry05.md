# Entry 5
##### 3/16/2020

# Summary

I wrote the code to start the game loop where it can perfectly render notes at the exact time provided in the beatmap.

# Engineering Design Process

To start, we created three main classes: Game, Note, and Staff

1. [Game](https://github.com/alexy4744/rhythm-game/blob/master/src/structures/Game.ts): This class is used to hold the game's audio manager, input handler, internal game time, and video output.
   - Audio Manager: Responsible for holding all of the audio related business, such as adding a track or sample to the game
   - Input Handler: Will handle all key presses, such as a key being pressed down to hit a note, etc..
   - Video Output: Responsible for all the scene, camera, renderer, lighting, and also video related logic such as when to draw the next note depending on the current position of the track.

   When a game is created, the `.start()` method will kickstart the game loop. This is similar to Unity's update method, or p5js draw function.

2. [Note](https://github.com/alexy4744/rhythm-game/blob/master/src/structures/Note.ts): A basic class that will represent a note of a beatmap. It is mainly to share common functionality of a note throughout the game, such as specifying when the note should appear, how long the note should be held for and etc...

3. [Staff](https://github.com/alexy4744/rhythm-game/blob/master/src/structures/Staff.ts): A class that keeps track of all of the notes that is read from a beatmap. It also tells the game how fast the BPM of the track is.

We also created a basic beatmap that currently look like [this](https://github.com/alexy4744/rhythm-game/tree/master/public/beatmaps/test)

Currently we have gotten it to render the notes on time. The next step will be to render the noteås in advance and have it move closer to the strum bar of the game on the z-axis, so that the notes can be properly hit with the correct timing. We will also have to add timing gates so that depending how early/late a note is hit, the player will be penalizing for hitting the note off-beat

The game currently looks like this with the beats spawning correctly in time (which is shown when the note spawns in sync with the console):

![](https://raw.githubusercontent.com/alexy4744/apcsa-freedom-project/master/ezgif.com-video-to-gif.gif)

# Skills

I learned how to add things to a 3D scene by creating meshes with geometry and material and then manipulating it's position vector. I also learned some basic music theory in order to create basic beatmaps to check if the game is in sync with the song.

# Next Steps

I need to make each note fall down like in guitar hero at the correct timing. After this is done, we can add timing gates and handle key inputs so that when the player hits a note, it will dissappear in order to meet halfway of our MVP. Then we can proceed to make a interface for players to create their own beatmaps with the songs they wish to complete our MVP. 

[Previous](entry04.md) | [Next](entry06.md)

[Home](../README.md)
