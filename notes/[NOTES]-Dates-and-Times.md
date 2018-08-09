# Date

https://doc.red-lang.org/en/date.html

Currently, time! values are stored internally as a 64-bit float. Date! values *contain* time values, but the two are separate. The date component is a varying number of bits per segment. i.e., the minimum number of bits needed for each part. The year part has 15 bits (signed) allocated. So not suitable for astronomy. As https://doc.red-lang.org/en/date.html says, they are Gregorian calendar dates.

## Arithmetic

## References


# Time

`time!` behaves like any other 64-bit binary float, with it's precision best near zero:
```red
>> 0:0:1.23456789e-100 * 1e96
== 0:00:00.0001234568
```
Unlike date! (which only makes sense for where the calendar defines it), time! is suitable enough for astronomy:
```red
>> 24:0:0 * 365.25 * 1e100 / 1e100 / 365.25
== 24:00:00
```
Although formatting is limited by 32-bit integers right now:
```red
>> 24:0:0 * 365.25 * 1e100 
== -2147483648:-2147483648:06.5862164e97
```