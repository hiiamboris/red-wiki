## Introduction
This is the document for the TCP port, UDP port, TLS port, DNS port and file port in Red. Those ports are asynchronous only.

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
will create a new port and also perform initializations associated with the open action. 

## Port Actions
The actions defined for ports are:

**make**

    make a new port object

**to**

    special (convert an object to a port)

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

