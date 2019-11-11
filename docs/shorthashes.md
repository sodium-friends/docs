---
id: shorthashes
title: Short Hashes
sidebar_label: Short Hashes
---

Bindings for the crypto_shorthash API. [See the libsodium crypto_shorthash docs for more information](https://download.libsodium.org/doc/hashing/short-input_hashing).
***
## `crypto_shorthash`
![sodium-node][node] ![sodium-javascript][js]
``` js
sodium.crypto_shorthash(output, input, key)
```
Hash a value to a short hash based on a key.
* `output` should be a `buffer` of length `crypto_shorthash_BYTES`
* `input` should be a `buffer` of any size
* `key` should be a `buffer` of length `crypto_shorthash_KEYBYTES`

The generated short hash is stored in `output`.


[js]: /docusaurus/img/icon_js.svg
[node]: /docusaurus/img/nodejs-icon.svg