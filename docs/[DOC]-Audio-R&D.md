Thoughts about different ways audio support can be added to Red, along with design ideas, essential features, and what different target users will need.

# Positional Audio (via @BeardPower)

Depending on the level (low, high, frameworks, engines), every platform has various APIs.
Counting the ones for Windows, Android, and macOS, you get >10 APIs, each with different features and designs (low latency, output quality, memory usage, hardware acceleration).
Some API provide 3D Audio and positional audio out of the box.
It would be much easier to use a cross-platform sound engine, but with the cost of having a dependency.

## What is 3D Audio/Positional audio?
It can be seen as the equivalent to what 3D Graphics is. Sound waves are moving, scattered, reflected, obstructed,.. etc. in 3D space like photons are for graphics/visuals. 3D/Positional audio gives an impression of the real world related to sound design and sound immersion.

## How can it be implemented?
There are three different possibilities.
- (1) Through supported hardware (think of hardware acceleration for sound)
- (2) Through software (think of a software renderer for sound)
- (3) Through 3rd party libraries and frameworks, which support software and hardware solutions

## How does it work?
A 2D/3D scene is ray-traced/ray-casted like with graphics, to calculated values for obstruction, reflections, dampening, hall and, reverb and other effects. These estimated values are then fed into DSP algorithms (implemented in software or hardware) and the final sound stream is sent to the audio device, which drives the speakers.
Performing DSP calculations can be very performance heavy, as it requires (i)FFT ((inverse)Fast Fourier Transformation) to convert the time-space into the frequency-space and vice versa, frequency decomposition and more. Complex numbers are involved. For the interested reader: https://www.youtube.com/watch?v=spUNpyF58BY
 
## Thoughts about the solutions
(1) Individual hardware support offers the best performance, as it takes the computational load off of the CPU and the GPU, but requires sound cards and drivers, which do not come with the OS. Dependencies required.
Alternatively, computations can be run on the GPU without the need for a particular sound card.
(2) Computations are run on the CPU. No creativity limits, but decreased performance. No dependencies.
(3) The frameworks, often cross-platform, offer a lot of features and extensions including hardware support but requires a dependency, which is not part of the OS.

## APIs and libraries
For each platform, there are multiple APIs supported and implemented. They are separated into low-level APIs and high-level APIs. The former act as a building block for implementing a sound engine. They are optimized for low latency, jitter-free, and high-quality sound. Some come with extensions for DSP work and helper classes. The latter are black-boxes with fixed functionality, which act like a complete sound engine.
Think the former as a graphics equivalent to SDL/SFLM/Kha and the latter to UE/CryEngine/Unity.

A nice overview of the implemented Android APIs: https://www.linkedin.com/pulse/android-audio-apis-jorge-frisancho

Open SL ES is the sound equivalent to OpenGL ES. It is optimized for mobile and embedded devices: https://www.khronos.org/opensles/

XAudio2 is the successor to DirectSound and XAudio. Previous versions do not come with the OS and were part of the DirectX SDK, so older versions of Windows cannot be supported without installing the SDK, which adds a dependency. XAudio2 is the base to build a sound engine: https://msdn.microsoft.com/en-us/library/windows/desktop/ee415813(v=vs.85).aspx

X3DAudio is built on top of XAudio2 and offers 3D sound: https://msdn.microsoft.com/en-us/library/windows/desktop/ee415714(v=vs.85).aspx

The Core Audio API is the foundation of DirectSound and the building block for a sound engine: https://msdn.microsoft.com/en-us/library/windows/desktop/dd370802(v=vs.85).aspx

All Apple devices use CoreAudio. It can be used a building block for a sound engine: https://developer.apple.com/library/content/documentation/MusicAudio/Conceptual/CoreAudioOverview/Introduction/Introduction.html

Superpowered is one of the newer sound engines. It supports Android, OSX, and iOS and is free. It's lower latency and offers more performance than other APIs for Android and Apple devices: http://www.superpowered.com

The following are Industry leading and tested sound engines/frameworks, which are not free (depends on the project and budget). They all support every major Desktop, Mobile, and Console device.

FMOD: https://www.fmod.com/

Wise: https://www.audiokinetic.com/products/wwise/

Miles: http://www.radgametools.com/miles.htm

Howler.js: https://github.com/goldfire/howler.js

# Other Interesting Links

* [AudioMasher](https://audiomasher.org) - live audio and music programming environment based on [Sporth](http://paulbatchelor.github.io/proj/sporth.html), a Forth dialect for audio DSP.
* [API History](http://shanekirk.com/2015/10/a-brief-history-of-windows-audio-apis/) - A brief history of Windows audio APIs.
* R/S can be leveraged for writing Pure Data [externals](https://github.com/pure-data/externals-howto). No idea how Max/MSP handles that.
* [Open Music](https://en.wikipedia.org/wiki/OpenMusic) - OO visual programming environment for musical composition, based on Common Lisp. Unlike conventional "patch-based" environments (e.g. PD, Max/MSP) the result of computation is displayed in a conventional musical notation. Has a moderately large and diverse library ecosystem (see the wiki page for more details).