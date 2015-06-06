## MetroHash: Faster, Stronger Hash Functions

MetroHash is a set of state-of-the-art hash functions for *non-cryptographic* use cases. They are notable for being algorithmically generated in addition to their exceptional performance. The set of published hash functions may be expanded in the future, having been selected from a very large set of hash functions that have been constructed this way.

* Fastest general-purpose functions for bulk hashing. 
* Fastest general-purpose functions for small, variable length keys. 
* Robust statistical bias profile, similar to the MD5 cryptographic hash.
* 64-bit, 128-bit, and 128-bit CRC variants currently available.
* Optimized for modern x86-64 microarchitectures.
* Elegant, compact, readable functions.
 
You can read more about the design and history [here](http://www.jandrewrogers.com/2015/05/27/metrohash/).

These hash function generation software made no effort toward portability. While these hash functions should be easily portable to big-endian microarchitectures, they have not been tested on them and the performance optimization algorithms were not targeted at them. ARM64 microarchitectures might be a worthwhile hash function generation targets if I had the hardware.

Hash functions in the same family are effectively statistically unique. In other words, if you need two hash functions for a bloom filter, you can use "metro64_1" and "metro64_2" in the same implementation without issue. An unbounded set of statistically unique functions can be generated in each family. The functions in this repo were generated specifically for public release.
