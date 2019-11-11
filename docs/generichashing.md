---
id: generichashing
title: Generic Hashing
sidebar_label: Generic Hashing
---

Bindings for the crypto_generichash API. [See the libsodium crypto_generichash docs for more information](https://download.libsodium.org/doc/hashing/generic_hashing).
***
## `crypto_generichash`
![sodium-node][node] ![sodium-javascript][js]
``` js
sodium.crypto_generichash(output, input, [key])
```
Hash a value with an optional key using the `generichash` method.
* `output` should be a `buffer` of length within `crypto_generichash_BYTES_MIN` - `crypto_generichash_BYTES_MAX`
* `input` should be a `buffer` of any length
* `key` is an optional `buffer` of length within `crypto_generichash_KEYBYTES_MIN` - `crypto_generichash_KEYBYTES_MAX`

The generated hash is stored in `output`.

Also exposes `crypto_generichash_BYTES` and `crypto_generichash_KEYBYTES` that can be used as "default" `buffer` sizes.
***
## `crypto_generichash_batch`
![sodium-node][node] ![sodium-javascript][js]
``` js
sodium.crypto_generichash_batch(output, inputArray, [key])
```
Same as `crypto_generichash` except this hashes an array of `buffer`'s instead of a single one.
***
## `crypto_generichash_instance`
![sodium-node][node] ![sodium-javascript][js]
``` js
var instance = sodium.crypto_generichash_instance([key], [outputLength])
```
Create a `generichash` instance that can hash a stream of input `buffer`'s.
* `key` is an optional `buffer` as above
* `outputLength` the `buffer` size of your output

## `instance.update`
``` js
instance.update(input)
```
Update the instance with a new piece of data.
* `input` should be a `buffer` of any size

## `instance.final`
``` js
instance.final(output)
```
Finalize the instance.
* `output` should be a `buffer` as above with the same length you gave when creating the instance

The generated hash is stored in `output`.


[js]: /docusaurus/img/icon_js.svg
[node]: /docusaurus/img/nodejs-icon.svg
