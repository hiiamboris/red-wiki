If you have snippets or tips for keyboard handling, please add them to this wiki.

Docs: https://github.com/red/docs/blob/master/en/view.adoc#11-events


# Hints and Tips

`key-down` and `key-up` events do not translate keys into characters. For example `Shift+1` is correctly detected via `all [event/key = #"1" event/shift?`, rather than `event/key = #"!"`. `event/key = #"!"` works if using the `key` (`on-key`) event.

# Chats

[Filtering keys in fields](https://rebol.tech/gitter.im/red/red/welcome/2021/#msg609fa7199f2c352db10eed6c)