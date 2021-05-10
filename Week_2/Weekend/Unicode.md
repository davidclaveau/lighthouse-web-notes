# Unicode

* Unicode is a way to encode letters and symbols into bytes.

* Encoding is similar to cyphers, you use a "key" to "crack" the code of a message using an encoder. This allows a message to be sent and received with the same translation from bits.

* The most popular system at the moment is **UTF-8** which uses up to four bytes to encode more than a million characters.

* UTF-8 also allows backwards compatability with systems that still use the 128 character ASCII system.

* The first byte of the UTF-8 encoded string will indicate the concatentation of the other bytes 1s and 0s. This means that, instead of "a" being four bytes with three of the bytes being 0, the first byte indicates the which parts of the bytes to read.