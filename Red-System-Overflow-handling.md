Current state:

### Intel

* Addition: optional post-check
* Subtraction: optional post-check
* Multiplication: optional post-check
* Division
    * Division by zero: hard-exception (maskable)
    * Division overflow: soft-exception

### ARMv5-6

* Addition: optional post-check
* Subtraction: optional post-check
* Multiplication: optional post-check
* Division
    * Division by zero: soft-exception
    * Division overflow: soft-exception

### ARMv7+

* Addition: optional post-check
* Subtraction: optional post-check
* Multiplication: optional post-check
* Division
    * Division by zero: returns 0
    * Division overflow: returns 80000000h
