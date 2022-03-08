## Introduction
This is the document for the TCP port, UDP port, TLS port, DNS port and file port in Red.

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

| Scheme\Event      | read | wrote | connect | accept | close | EOF |
|:-----------------:|------|-------|---------|--------|-------|-----|
| TCP               | yes  | yes   | yes     | yes    | yes   | no  |
| TLS               | yes  | yes   | yes     | yes    | yes   | no  |
| UDP               | yes  | yes   | no      | no     | yes   | no  |
| FILE              | yes  | yes   | yes     | no     | yes   | yes |

## Using Ports
An AWAKE handler function must be provided to process the callback events. Each time you perform an asynchronous action, you'll get a callback event once it's finished. 

Here is an example of a server and a client that send a small message back and forth. Both the sides use asynchronous I/O.

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
A program synchronize with the port by using the WAIT function. When a port is opened, it will be tracked internally. So you can open many ports and wait on them, no need to pass them to WAIT function.

```
port1: open tls://www.red-lang.org
port1/awake: :my-tls-awake-handler
port2: open file:///E/movies/movie1.avi
port2/awake: :my-file-awake-handler
wait 10   ;-- timeout in 10 seconds if no event received
```
For a server which need to wait infinite time, wait on the port directly or pass `-1` as timeout.
```
server: open tcp://:8123
server/awake: :my-server-awake-handler
wait server   ;-- or wait -1
```