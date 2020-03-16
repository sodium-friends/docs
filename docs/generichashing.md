---
id: generichashing
title: Generic Hashing
sidebar_label: Generic Hashing
---

Bindings for the crypto_generichash API. [See the libsodium crypto_generichash docs for more information](https://download.libsodium.org/doc/hashing/generic_hashing).

## Constants
**Buffer lengths (integer)**
* `crypto_generichash_BYTES`
* `crypto_generichash_BYTES_MIN`
* `crypto_generichash_BYTES_MAX`
* `crypto_generichash_KEYBYTES`
* `crypto_generichash_KEYBYTES_MIN`
* `crypto_generichash_KEYBYTES_MAX`
* `crypto_generichash_STATEBYTES`

**String constants (string)**
* `crypto_generichash_PRIMITIVE`

***
## `crypto_generichash`
![sodium-native][node] ![sodium-javascript][js]
``` js
sodium.crypto_generichash(out, in, [key])
```
Hashes a value with an optional key using the `generichash` method.
* `out` should be a `buffer` of length within `crypto_generichash_BYTES_MIN` - `crypto_generichash_BYTES_MAX`
* `in` should be a `buffer` of any length
* `key` is an optional `buffer` of length within `crypto_generichash_KEYBYTES_MIN` - `crypto_generichash_KEYBYTES_MAX`

The generated hash is stored in `out`.

Also exposes `crypto_generichash_BYTES` and `crypto_generichash_KEYBYTES` that can be used as "default" `buffer` sizes.
***
## `crypto_generichash_batch`
![sodium-native][node] ![sodium-javascript][js]
``` js
sodium.crypto_generichash_batch(out, inArray, [key])
```
Same as `crypto_generichash`, except that this hashes an array of `buffer`'s instead of a single one.
***
## Instance API
### `crypto_generichash_instance`
![sodium-native][node] ![sodium-javascript][js]
``` js
var instance = sodium.crypto_generichash_instance([key], [outlen])
```
Creates a `generichash` instance that can hash a stream of input `buffer`'s.
* `key` is an optional `buffer` as above
* `outlen` is the `buffer` size of your output

### `instance.update`
``` js
instance.update(in)
```
Updates the instance with a new piece of data.
* `in` should be a `buffer` of any size

### `instance.final`
``` js
instance.final(out)
```
Finalizes the instance.
* `out` should be a `buffer` as above with the same length you gave when creating the instance

The generated hash is stored in `out`.

## Stateful API
Replaces the above instance implementation in the N-API release
### `crypto_generichash_init`
![sodium-native][node]
```js
var state = Buffer.alloc(crypto_generichash_STATEBYTES)

sodium.crypto_generichash_init(state, [key], outlen)
```
Initialise a new hash state with an optional key and the desired output length.
* `state` must be a buffer of length `crypto_generichash_STATEBYTES` bytes
* `key` is an optional buffer as above

### `crypto_generichash_update`
![sodium-native][node]
```js
sodium.crypto_generichash_update(state, in)
```
Update a hash state with a given input.
* `state` must be a buffer of length `crypto_generichash_STATEBYTES` bytes
* `in` should be a a `buffer` of any length

### `crypto_generichash_final(state, out)`
![sodium-native][node]
```js
sodium.crypto_generichash_final(state, out)
```
Finalize a given hash state and write the digest to `output` buffer
* `state` must be a buffer of length `crypto_generichash_STATEBYTES` bytes
* `out` must be a `buffer` with `crypto_generichash_BYTES_MIN <= out.byteLength <= crypto_generichash_BYTES_MAX`
* `out.byteLength` should equal `outlen` specified when `crypto_generichash_init` was called

[js]: /docs/img/icon_js.svg
[node]: /docs/img/nodejs-icon.svg
