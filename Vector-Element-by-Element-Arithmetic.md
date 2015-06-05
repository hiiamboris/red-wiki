Performing element by element arithmetic operations with two values of the vector! type is straightforward where the two vectors contain the same number of elements. This proposal is related to those cases where the vectors have different lengths. It seems that quite a few high-level languages such as MatLab and Wolfram will only perform these operations on strings of equal length.

The options are:

### 1. Return a vector with the same number of elements as the longer one.
If this approach is taken, a clear set of "rules" would need to be implemented covering all possible cases. For example, a different rule would be needed for dividing #(10, 20, 30) by #(1, 2, 3, 4) from dividing #(10, 20, 30) by #(1, 2).

### 2. Return a vector with the same number of elements as the shorter one.
This is very clear and does not need special rules which should make it easier to implement, learn and use. (This seems to be the approach taken by Haskell.)

### 3. Recycling
This is a technique to keep applying the elements of the shorter vector in turn until all elements of the  longer vector have been operated on. For example, #(1 2 3 4 5 6) multiplied by #(10 20) would yield #(10 40 30 80 50 120). (This seems to be the approach taken by R.)

### Proposal

Option 1 appears the least desirable.

Option 2 would appear to be the best in most use cases.

Options 3 could be very useful in some situations with the proviso that the length of the longer vector is a multiple of the shorter one.

It is proposed that Option 2 is adopted by Red and that a separate function (or functions be provided) in a library to perform recycling vector element by element arithmetic. 
