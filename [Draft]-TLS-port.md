We use `port/extra` to set the TLS port for now. Need to find a better way to do it. The supported settings are as follows:

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

