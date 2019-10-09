---
id: padding
title: Padding
sidebar_label: Padding
---

Bindings to the padding API. [See the libsodium padding docs for more information](https://download.libsodium.org/doc/padding).

``` js
var paddedLength = sodium.sodium_pad(buf, unpaddedLength, blocksize)
```
Pad `buf` with random data from index `unpaddedLength` up to closest multiple of `blocksize`.
* `buf` must be `buffer`
* `unpaddedLength` must be integer at most `buf.length`
* `blocksize` must be integer greater than 1 but at most `buf.length`

Returns the length of the padded data (so you may `.slice` the buffer to here).

``` js
var unpaddedLength = sodium.sodium_unpad(buf, paddedLength, blocksize)
```
Calculate `unpaddedLength` from a padded `buf` with `blocksize`.
* `buf` must be `buffer`
* `paddedLength` must be integer at most `buf.length`
* `blocksize` must be integer greater than 1 but at most `buf.length`

Returns the length of the unpadded data (so you may `.slice` the buffer to here).
