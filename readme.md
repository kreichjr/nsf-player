# Changes - 09/10/2022
Added `setPlaybackTempo()`,`getVoiceNames()`,`muteVoice()`, and `togglePause()` methods to the nsfPlayer to allow users a bit more control over the playback.

Also updated the code `channels[n][i] = Module.getValue(buffer + i * e.outputBuffer.numberOfChannels * 2 + n * 2, 'i32') / INT16_MAX;` as the original code `channels[n][i] = Module.getValue(buffer + i * e.outputBuffer.numberOfChannels * 2 + n * 4, 'i32') / INT16_MAX;` has `n * 4` which from what I can tell, ends up writing the left channel's audio into the right channel. If my understanding is wrong, please let me know!


## New methods

**nsfPlayer.setPlaybackTempo(float)**
This method takes a float value between *0.01* and *4* as determined by the gme module. Changes the tempo of playback without altering pitch. 

**nsfPlayer.getVoiceNames()**
Returns an array of each voice name, where the index is the voice's reference index (can be used in the muteVoice method), and the value is the
given name. For example, calling this with the Super Mario Land gameboy rom will return `["Square 1", "Square 2", "Wave", "Noise"]` meaning:

Voice 0 = Square 1
Voice 1 = Square 2
Voice 2 = Wave
Voice 3 = Noise

**nsfPlayer.muteVoice(voiceIndex, muteInt)**
This method takes a *voiceIndex* (which you can find via `getVoiceNames()`) and an integer value of either *0* or *1* for the *muteInt* and
will either unmute or mute that voice respectively. For example:
```
nsfPlayer.muteVoice(2, 1)  // This will mute the Wave channel based on the above getVoiceNames() example.
nsfPlayer.muteVoice(1, 0)  // This will unmute the Square 2 channel based on the above getVoiceNames() example.
``` 

**nsfPlayer.togglePause()**
This method uses the built-in methods of the AudioContext to pause and resume playback. If the audio is playing, simply call by doing:
```
nsfPlayer.togglePause()  // Calling once while playing will pause the audio
...
nsfPlayer.togglePause()  // Calling again will unpause
```

Note that once paused, calling `nsfPlayer.play()` or `nsfPlayer.stop()` will not force the context to be unpaused. Only using `nsfPlayer.load()`
will allow playback as it creates a new AudioContext.


# Original Readme
# NSF Player

This is a player for NSF-format Nintendo Entertainment System music files. It's written in JavaScript and should work in any major browser.

## How to use it
Include **libgme.js** and **index.js** in your project.
```
<script src="libgme/libgme.js"></script>
<script src="index.js></script>
```
Create a player by calling `createNsfPlayer`.
```
const nsfPlayer = createNsfPlayer(); // An audio context is created for you.
```
Optionally, you can pass in your own audio context.
```
const ctx = new AudioContext();
const nsfPlayer = createNsfPlayer(ctx);
```
NSF files may contain multiple tracks. Play a track by calling `play` and passing it the path to your NSF file and the index of the track you wish to hear. (Indexes start at 0 and go up.)
```
nsfPlayer.play('./songs/smb.nsf', 2);
```
If you just want to hear the first track, you don't have to pass an index.
```
nsfPlayer.play('./songs/smb.nsf');
```
Stop the music by calling `stop`.
```
nsfPlayer.stop();
```
## Notes
This uses (and includes) libgme, A.K.A. Game_Music_Emu, a library for emulating video game music.
I adapted it from code I found on the web at [onakasuita.org/jsgme](http://onakasuita.org/jsgme/).