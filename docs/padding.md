---
id: padding
title: Padding
sidebar_label: Padding
---

Bindings for the padding API. [See the libsodium padding docs for more information](https://download.libsodium.org/doc/padding).
***
## `sodium_pad`
![sodium-native][node]
``` js
var paddedLength = sodium.sodium_pad(buf, unpaddedLength, blocksize)
```
Pads `buf` with random data from index `unpaddedLength` up to closest multiple of `blocksize`.
* `buf` must be a `buffer`
* `unpaddedLength` must be an integer at most `buf.length`
* `blocksize` must be an integer greater than 1, but at most `buf.length`

Returns the length of the padded data (so you may `.slice` the `buffer` to here).
***
## `sodium_unpad`
![sodium-native][node]
``` js
var unpaddedLength = sodium.sodium_unpad(buf, paddedLength, blocksize)
```
Calculates `unpaddedLength` from a padded `buf` with `blocksize`.
* `buf` must be a `buffer`
* `paddedLength` must be an integer at most `buf.length`
* `blocksize` must be an integer greater than 1, but at most `buf.length`

Returns the length of the unpadded data (so you may `.slice` the `buffer` to here).


[js]: /docusaurus/img/icon_js.svg
[node]: /docusaurus/img/nodejs-icon.svg
