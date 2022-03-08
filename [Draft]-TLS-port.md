We use `port/extra` to configure the TLS port for now. Need to find a better way to do it. The supported settings are as follows:

|         key         | datatype | description                                        |
|:-------------------:|:--------:|----------------------------------------------------|
| domain              | string!  | TBD                                                |
| key                 | string!  | private key                                        |
| password            | string!  | password for the private key                       |
| certs               | block!   | certificates                                       |
| roots               | block!   | root certificates                                  |
| accept-invalid-cert | logic!   | whether the port accept invalid certificate or not |
| disable-builtin-roots | logic!   | TBD |
| min-protocol        | integer! | the minimum supported protocol version |
| max-protocol        | integer! | the maximum supported protocol version |

Here is an example of a server and a client that send a small message back and forth.

### Pong Server
```
Red []

cert: {
-----BEGIN CERTIFICATE-----
MIICqTCCAk6gAwIBAgIIAlPHPJeDDOcwCgYIKoZIzj0EAwIwgYQxCzAJBgNVBAYT
AkNOMRUwEwYDVQQKEwxiaXRiZWdpbi5jb20xHTAbBgNVBAsTFGJpdGJlZ2luIHRl
c3QgY2EgZWNjMRwwGgYDVQQDExNlY2MuY2EuYml0YmVnaW4uY29tMSEwHwYJKoZI
hvcNAQkBDBJiaXRiZWdpbkBnbWFpbC5jb20wHhcNMjAwNzI5MTAxODI1WhcNMjEw
NzI5MTAxODI1WjCBgzELMAkGA1UEBhMCQ04xFTATBgNVBAoTDGJpdGJlZ2luLmNv
bTEaMBgGA1UECxMRYml0YmVnaW4gdGVzdCBlY2MxHjAcBgNVBAMTFWVjYy50ZXN0
LmJpdGJlZ2luLmNvbTEhMB8GCSqGSIb3DQEJAQwSYml0YmVnaW5AZ21haWwuY29t
MFkwEwYHKoZIzj0CAQYIKoZIzj0DAQcDQgAEdpVlebb9Ytkyh+S7jt9PU/3Wgoce
n8ow6uGdkTXndztsdVRsa3xWoXZZNPoHyZTEX+OsRo8EKY59XsIadNpNmaOBqDCB
pTAOBgNVHQ8BAf8EBAMCBaAwHQYDVR0lBBYwFAYIKwYBBQUHAwEGCCsGAQUFBwMC
MB0GA1UdDgQWBBRwmvJWNz8OWyQFgksfTdZE7jXEYjAfBgNVHSMEGDAWgBRDtn96
EtUZvjCPdDe7aKuFM4YPJDA0BgNVHREELTArghVlY2MudGVzdC5iaXRiZWdpbi5j
b22BEmJpdGJlZ2luQGdtYWlsLmNvbTAKBggqhkjOPQQDAgNJADBGAiEApk0PGmEn
M52Jj9AM4ADUdYIoITAgTb5ufXdMnTKK56wCIQD1fvB03WdEj5jtNTJ1dtNP36s/
fyxJfdmh3D4m87j0lA==
-----END CERTIFICATE-----
}

chain: {
-----BEGIN CERTIFICATE-----
MIICvDCCAmKgAwIBAgIIXTWd8+wMzG8wCgYIKoZIzj0EAwIwgYgxCzAJBgNVBAYT
AkNOMRUwEwYDVQQKEwxiaXRiZWdpbi5jb20xHzAdBgNVBAsTFmJpdGJlZ2luIHRl
c3QgZWNjIHJvb3QxHjAcBgNVBAMTFWVjYy5yb290LmJpdGJlZ2luLmNvbTEhMB8G
CSqGSIb3DQEJAQwSYml0YmVnaW5AZ21haWwuY29tMB4XDTIwMDcyOTEwMTYwMVoX
DTIxMDcyOTEwMTYwMVowgYQxCzAJBgNVBAYTAkNOMRUwEwYDVQQKEwxiaXRiZWdp
bi5jb20xHTAbBgNVBAsTFGJpdGJlZ2luIHRlc3QgY2EgZWNjMRwwGgYDVQQDExNl
Y2MuY2EuYml0YmVnaW4uY29tMSEwHwYJKoZIhvcNAQkBDBJiaXRiZWdpbkBnbWFp
bC5jb20wWTATBgcqhkjOPQIBBggqhkjOPQMBBwNCAASW1UJm0UuBTkr3ZPd3W0CC
LrDS7WU7UAvDxVvK9RC2buvU7sXnM57HBJVOZeC+QTbbufgGmUnI7RwNb7lrktSs
o4G3MIG0MA4GA1UdDwEB/wQEAwIFoDAdBgNVHSUEFjAUBggrBgEFBQcDAQYIKwYB
BQUHAwIwDwYDVR0TAQH/BAUwAwEB/zAdBgNVHQ4EFgQUQ7Z/ehLVGb4wj3Q3u2ir
hTOGDyQwHwYDVR0jBBgwFoAUUtg1u5Mb9YcgyF8UwVWkXK17lLwwMgYDVR0RBCsw
KYITZWNjLmNhLmJpdGJlZ2luLmNvbYESYml0YmVnaW5AZ21haWwuY29tMAoGCCqG
SM49BAMCA0gAMEUCIGvs3zEAWlmGjDvzc5wCCsX/M4RoMmud+nqKVsKVnR65AiEA
7gYFJIVIPt7a04oCb9LCvzYU6Fsp7YoJ5OgWYptO5Eo=
-----END CERTIFICATE-----
}

key: {
-----BEGIN EC PRIVATE KEY-----
MHcCAQEEIO+S0fDqSIa59vOh+AJGtAYdmg87JujJb9cM5MjdzVQCoAoGCCqGSM49
AwEHoUQDQgAEdpVlebb9Ytkyh+S7jt9PU/3Wgocen8ow6uGdkTXndztsdVRsa3xW
oXZZNPoHyZTEX+OsRo8EKY59XsIadNpNmQ==
-----END EC PRIVATE KEY-----
}

protos: [
    sslv3   0300h
    tls1.0  0301h
    tls1.1  0302h
    tls1.2  0303h
    tls1.3  0304h
]

print "Ping pong server TLS"

server: open tls://:8085

server/extra: compose/deep [
    certs: [(cert) (chain)]
    key: (key)
    min-protocol: (protos/tls1.1)
    max-protocol: (protos/tls1.3)
]

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

wait 30
```

### Ping Client
```
Red []

protos: [
    sslv3   0300h
    tls1.0  0301h
    tls1.1  0302h
    tls1.2  0303h
    tls1.3  0304h
]

print "Ping pong client"

ping-count: 0

client: open tls://127.0.0.1:8085

client/extra: compose/deep [
    accept-invalid-cert: (true)
    disable-builtin-roots: (false)
    min-protocol: (protos/tls1.2)
    max-protocol: (protos/tls1.2)
]

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

wait 3
```