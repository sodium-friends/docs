---
id: shorthashes
title: Short Hashes
sidebar_label: Short Hashes
---

Bindings for the crypto_shorthash API. [See the libsodium crypto_shorthash docs for more information](https://download.libsodium.org/doc/hashing/short-input_hashing).

## Constants
**Buffer lengths (integer)**
* `crypto_shorthash_BYTES`
* `crypto_shorthash_KEYBYTES`

**String constants (string)**
* `crypto_shorthash_PRIMITIVE`

***
## `crypto_shorthash`
![sodium-native][node] ![sodium-javascript][js]
``` js
sodium.crypto_shorthash(output, input, key)
```
Hashes a value to a short hash based on a `key`.
* `output` should be a `buffer` of length `crypto_shorthash_BYTES`
* `input` should be a `buffer` of any size
* `key` should be a `buffer` of length `crypto_shorthash_KEYBYTES`

The generated short hash is stored in `output`.


[js]: /docs/img/icon_js.svg
[node]: /docs/img/nodejs-icon.svg
