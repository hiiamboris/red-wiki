In most languages, code is evaluated when compiled, or interpreted. And that code is fixed. If you want added flexibility, you have to build it yourself. For example, let's say you have a switch statement in your code:

```
while [not empty? str: ask "What say you? "][
    print switch str [
        "Hello" ["Howdy"]
        "Bye" ["Later"]
    ]
]
```
Normal code. If you want to change the list of inputs and outputs, you edit the code. But you have to edit the code. You have to edit it where it sits. You have to be in the middle of the file with a bunch of other code. You can't take it out, put it in a config file, or create a tool that lets users make their own.

```
chat-map: [
    "Hello"  ["Howdy"]
    "Bye"    ["Later"]
    "What?!" ["Yeah, that's right"]
]
while [not empty? str: ask "What say you? "][
    print switch str chat-map
]
```

Nothing scary there, right? But I smell your next fear. "WHAT?! Put executable...stuff in the hands of users? Some crazy person will use this to write self-modifying code and the world will end! How do you debug anything when code, I mean data that's code, could come from anywhere?" All valid concerns. It's not a silver bullet, it's a tool. Like abstraction in OOP it can be used well or abused terribly. Red gives you this gift, and this burden. We don't tell you how to think, but offer you tools for new ways of thinking.