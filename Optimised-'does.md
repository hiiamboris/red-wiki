In Red/System version1 'DOES is simply shorthand for func []. It could be optimised by:

1. Storing the address of the following statement.
2. Compiling the code in the 'DOES block with a branch to the stored return address.
3. Branching to the start of the code of the 'DOES block.



