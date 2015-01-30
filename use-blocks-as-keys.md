Maps already have strings as keys and these are mutable.  Since this is the case, blocks should also have the same capability; keys of a map.

'''
m: map [[1 2] [3 4]]

m/[1 2] => [3 4]
'''