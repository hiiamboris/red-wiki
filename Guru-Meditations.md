# Debugging

@SteeveGit hit a crashing bug under 0.6.0: `view [button "crash" [type?/word event]]`

It led to issue #1983. You should read the full guru answer there.

tl;dr is that you should compile with `-d` for debug mode, and that simple tools (`probe` `??` `dump4` `stack/dump` `print-symbol`) are generally enough. Remember that Red's toolchain is minimal, building from source is fast and easy, and the source base is small and well organized. And if you've only ever worked at the Red level, a small amount of time learning some Red/System can go a long way. Even if you aren't confident you could write a fix, you may help save others time, and have fun exploring in the process. Don't be afraid.





