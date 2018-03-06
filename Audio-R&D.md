Thoughts about different ways audio support can be added to Red, along with design ideas, important features, and what different target users will need.

# Positional Audio (via @BeardPower)

https://www.linkedin.com/pulse/android-audio-apis-jorge-frisancho
https://www.khronos.org/opensles/
https://msdn.microsoft.com/en-us/library/windows/desktop/ee415813(v=vs.85).aspx
https://msdn.microsoft.com/en-us/library/windows/desktop/ee415714(v=vs.85).aspx
https://msdn.microsoft.com/en-us/library/windows/desktop/dd370802(v=vs.85).aspx

Depending on the level (low, high, frameworks, engines), every platform has various APIs.

Counting the ones for Windows, Android and macOS, you get >10 APIs, each with different features and designs (low latency, output quality, memory usage, hardware acceleration,...).

Some API provide 3D Audio and positional audio out of the box.

It would be much easier to use a cross-platform sound engine, but with the cost of having a dependency.

