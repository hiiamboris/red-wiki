The standard JSON codec is meant to be easy to use, and leverages Red for a high level implementation. It is not optimized for high performance. That doesn't mean we can't have other options, but it should work for the vast majority of use cases. What we want to avoid is having a large number of incompatible libraries that differ and specialize in small ways, but without each adding much value outside that.

There are so many JSON libs out there, that we can learn from and perhaps define 3 primary goal-oriented libs:
1) Standard - Trades off performance for readability and ease of implementation. Should just work out of the box, but without many extra options which would complicate it.
2) High performance - Probably done in R/S, perhaps with tricks to reduce memory consumption. Could even leverage things learned from Red's fast lexer, applied to JSON.
3) Flexible - Config options, more parsing control, who knows?

There are many ways to approach these goals, and experiments to show how Red allows new ways to solve this problem will be great. But, ultimately, there should be no more than (arbitrarily) 3 "blessed" libs we guide people to.

Something else to consider is that if we have a standard data model on the Red side, the libs can be intermixed in apps, depending on the needs. Maybe decoding should be fast, but encoding needs to be flexible. 

> January-2020 note: The JSON codec is now a mix of Red, using `parse`, and Red/System (R/S) to optimize string decoding. 

# References, comparisons, benchmarks, etc.

It would be interesting to see how a JSON parser modeled on Red's fast lexer performs. But the more important comparison is how fast Red can `load` Red data compared to how fast other langs can "load" JSON such that it's usable in an application. And this is important not only because we want to show Red's power and performance, but because JSON wasn't designed to be processed quickly. It was designed to be easy to parse and long-lasting. Red is the same, but much richer. So we need to look at the lifecycle of the data from production thru consumption to result. That includes time spent developing the application. How long does it take to write a Red script to do what you need, compared to a C++ app using simdjson? Then you have to amortize the dev cost against runtime, including how much sooner the Red app can be used. At some scale the highly optimized version will make sense, and this is true in general, so another great tool we can use to help people is those calcs and costs. 

We can also compare redbin to binary equivalents of other formats.

- https://www.json.org/json-en.html
- https://medium.com/tourradar/working-efficiently-with-json-in-go-cb80dcca0466
- https://github.com/simdjson/simdjson King of the hill C++ lib using SIMD instructions. Quite a few test files as well.
- https://chadaustin.me/2013/01/json-parser-benchmarking/
- https://lemire.me/blog/2018/05/03/how-fast-can-you-parse-json/

# Cycles

- https://github.com/red/red/issues/4691
