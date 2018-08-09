`Take` is a wonderful function, making some operations very easy. Knowing a bit about how it works, just at a very high level, can help you use it effectively. As the following shows, taking items from the head of a series, especially a large one, is slower than taking from the tail. Why is this? It's because, when you take an item from a series, all the items following it move down in memory to fill the empty space. That makes each `take` an expensive operation. If you take from the tail (`take/last`), there are no elements following that need to move, so it's efficient. You can also use `take/part` to remove more than one item at a time.

Depending on your needs, it may be worth taking items from the series, tail to head, then reversing your result. Or maybe the order in which you take items doesn't matter, in which case, taking from the tail will be much faster.

```red
blk: collect [repeat i 100'000 [keep i]]

take-from-head:         func [blk][until [take blk               empty? blk]]
take-from-tail:         func [blk][until [take/last blk          empty? blk]]
take-part-from-head:    func [blk][until [take/part blk 1        empty? blk]]
take-part-from-tail:    func [blk][until [take/part/last blk 1   empty? blk]]
take-part-from-head-32: func [blk][until [take/part blk 32       empty? blk]]
take-part-from-tail-32: func [blk][until [take/part/last blk 32  empty? blk]]

profile/show [
    [take-from-head copy blk]
    [take-from-tail copy blk]
    [take-part-from-head copy blk]
    [take-part-from-tail copy blk]
    [take-part-from-head-32 copy blk]
    [take-part-from-tail-32 copy blk]
]
```
```red
Count: 1
Time         | Time (Per)   | Memory      | Code
0:00:00.004  | 0:00:00.004  | 4202496     | [take-part-from-tail-32 copy blk]
0:00:00.088  | 0:00:00.088  | 7122944     | [take-part-from-tail copy blk]
0:00:00.09   | 0:00:00.09   | 7122944     | [take-from-tail copy blk]
0:00:00.108  | 0:00:00.108  | 4284416     | [take-part-from-head-32 copy blk]
0:00:03.729  | 0:00:03.729  | 7122944     | [take-part-from-head copy blk]
0:00:03.935  | 0:00:03.935  | 7122944     | [take-from-head copy blk]
```