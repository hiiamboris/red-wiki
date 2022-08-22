## Introduction
This is the document for the TCP port, UDP port, [TLS port](https://github.com/red/red/wiki/%5BDraft%5D-TLS-port) and file port in Red.

## Making Ports
All ports are made from a spec -- a specification of the port's attributes. The spec is a block that includes many fields to indicate various options for the port. The general form is: 
```
port: make port! spec
```
Here are some examples: 
```
TBD
```
One of the most common methods to create a port is with the open function. For example: 
```
port1: open tls://www.red-lang.org
port2: open file:///E/movies/movie1.avi
```
will create a new port and also perform initialization associated with the open action. 

## Port Actions
The actions defined for ports are:

**make**

    make a new port object

**open**

    initialize external operations

**close**

    conclude external operations

**insert**

    transfer data to the port

**copy**

    transfer data from the port

**query**

    get information about the port

Actions on a port can be synchronous or asynchronous. 

In general:

* Synchronous actions will not return until the requested function has completed.
* Asynchronous actions will return as soon as possible, even if the function is still being performed.

For network ports and file port, `make`, `open`, `close` and `query` are synchronous actions. `insert` and `copy` are asynchronous actions.

## Port Events
The events supported by the port depends on the type of the port.

| Scheme\Event      | read | wrote | connect | accept | close | error |
|:-----------------:|------|-------|---------|--------|-------|-----|
| TCP               | yes  | yes   | yes     | yes    | yes   | yes  |
| TLS               | yes  | yes   | yes     | yes    | yes   | yes  |
| UDP               | yes  | yes   | no      | no     | yes   | yes  |
| FILE              | yes  | yes   | yes     | no     | yes   | yes |

## Using Ports
An AWAKE handler function must be provided to process the callback events. Each time you perform an asynchronous action, you'll get a callback event once it's finished. 

Here is an example of a server and a client that send a small message back and forth. Both the sides use asynchronous I/O. A TLS version is [here](https://github.com/red/red/wiki/%5BDraft%5D-TLS-port). 

### Pong Server
Here is the TCP server. It accepts connections and waits for a ping packet. Then, it responds with a pong packet.
```
print "Ping pong server"

server: open tcp://:8083

server/awake: func [event /local client] [
    if event/type = 'accept [
        client: event/port
        client/awake: func [event] [
            switch event/type [
                read [
                    print ["Client said:" to-string event/port/data]
                    insert event/port "pong!"
                ]
                wrote [
                    print "Server sent pong to client"
                    copy event/port
                ]
            ]
        ]
        copy client
    ]
]

wait server
```

### Ping Client

Here is the client. It sends a ping to the server, then waits to get a pong reply back. When it does, it sends another ping. The ping-pong repeats several times, then stops.

Here, it pings 50 times, but feel free to try larger numbers.
```
print "Ping pong client"

ping-count: 0

client: open tcp://127.0.0.1:8083

client/awake: func [event /local port] [
    port: event/port
    switch event/type [
        connect [insert port "ping!"]
        wrote [
            print "Client sent ping to server"
            copy port
        ]
        read [
            print ["Server said:" to-string port/data]
            ping-count: ping-count + 1
            either ping-count > 50 [
                close port  ;-- close itself
            ][
                insert port "ping!"
            ]
        ]
    ]
]

wait client
```

**Note** that we only support single `insert` and `copy` for now. That means you should not perform the same action before the previous one finished. The following code may not work properly.
```
client/awake: func [event /local port] [
    port: event/port
    switch event/type [
        connect [
            insert port "ping!"
            insert port "ping again!"  ;@@ May fail if the previous insert hasn't been finished
        ]
        more code...
    ]
    more code ...
]
```

## Waiting on a Port
A program synchronize with the port by using the WAIT function. 

* You can wait on a single port or a block of ports. `WAIT` will return either after **all the ports were closed** or timeout.

```
port1: open tls://www.red-lang.org
port1/awake: :my-tls-awake-handler
port2: open file:///E/movies/movie1.avi
port2/awake: :my-file-awake-handler

;; timeout in 10 seconds if no event received
wait reduce [10 port1 port2]

;; Or you can wait infinite time
;; wait reduce [port1 port2]
```
Wait on a server port.
```
server: open tcp://:8123
server/awake: :my-server-awake-handler
wait server
```

* Passing an integer to `WAIT`, `WAIT` will return either after **received an event** or timeout.
```
port: open tcp://online-data.com
copy port

forever [
   wait 10
   if port/data [break]
]
```

When using ports in view, we don't have to use `wait` explicitly. The event loop in view will also handle I/O events.

Here is an example by using file port to copy file.

![image](https://user-images.githubusercontent.com/1673525/157180930-434adb99-2d1a-4514-b38f-94acecb7baf3.png)

```
Red [
    title: "Copy File"
    Needs: View
]

file-src: none
file-dst: none
port-src: none
port-dst: none

total-size: 0.0
read-cnt: 0.0
amount: 0.0

select-src: func [face event][
    if file-src: request-file [
        f1/text: to-local-file file-src
        total-size: to float! size? file-src
        file-src: rejoin [file:// file-src]
    ]
]

select-dst: func [face event][
    if file-dst: request-file/save [
        f2/text: to-local-file file-dst
        file-dst: rejoin [file:// file-dst]
    ]
]

view [
    text "Source File: " f1: field 300 button "Select" :select-src return
    text "Destination: " f2: field 300 button "Select" :select-dst return
    base 160.160.160 460x1 return
    prog: progress hidden 390x23 button "Copy" [
        if any [not file-src not file-dst][exit]
        prog/data: 0.0
        read-cnt: 0.0
        amount: 0.0
        port-dst: open/write/new file-dst
        port-src: open file-src
        port-src/awake: func [event /local port][
            port: event/port
            switch event/type [
                connect [copy port]
                read [
                    read-cnt: read-cnt + length? port/data
                    insert port-dst port/data
                ]
                error [         ;-- end of file
                    close port
                    close port-dst
                    prog/visible?: no
                ]
            ]
        ]
        port-dst/awake: func [event /local r][
            if event/type = 'wrote [
                r: read-cnt / total-size
                if r - amount >= 0.005 [
                    prog/data: r
                    amount: r
                ]
                copy port-src
            ]
        ]
        prog/visible?: yes
    ]
]
```