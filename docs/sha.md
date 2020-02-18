---
id: sha
title: SHA
sidebar_label: SHA
---

## Constants
**Buffer lengths (integer)**
* `crypto_hash_BYTES`
* `crypto_hash_sha256_BYTES`
* `crypto_hash_sha512_BYTES`
* `crypto_hash_sha512_STATEBYTES`

**String constants (string)**
* `crypto_hash_PRIMITIVE`

***
## `crypto_hash`
![sodium-native][node]
``` js
sodium.crypto_hash_(output, input)
```
Hashes a value to a short hash based on a key.
* `output` should be a `buffer` of length `crypto_hash_BYTES`
* `input` should be a `buffer` of any size

The generated short hash is stored in `output`.

***
## `crypto_hash_sha256`
![sodium-native][node]
``` js
sodium.crypto_hash_sha256(output, input)
```
Hashes a value to a short hash based on a key.
* `output` should be a `buffer` of length `crypto_hash_sha256_BYTES`
* `input` should be a `buffer` of any size

The generated short hash is stored in `output`.
***
## Stateful API
### `crypto_hash_sha256_instance`
![sodium-native][node]
``` js
var instance = sodium.crypto_hash_sha256_instance()
```
Creates an instance that has stream of input data to `sha256`.

### `instance.update`
``` js
instance.update(input)
```
Updates the instance with a new piece of data.
* `input` should be a `buffer` of any size

### `instance.final`
``` js
instance.final(output)
```
Finalizes the instance.
* `output` should be a `buffer` of length `crypto_hash_sha256_BYTES`

The generated hash is stored in `output`.
***
### `crypto_hash_sha512`
![sodium-native][node]
``` js
sodium.crypto_hash_sha512(output, input)
```
Hashes a value to a short hash based on a key.
* `output` should be a `buffer` of length `crypto_hash_sha512_BYTES`
* `input` should be a `buffer` of any size

The generated short hash is stored in `output`.
***
### `crypto_hash_sha512_instance`
![sodium-native][node]
``` js
var instance = sodium.crypto_hash_sha512_instance()
```
Creates an instance that has stream of input data to `sha512`.

### `instance.update`
``` js
instance.update(input)
```
Updates the instance with a new piece of data.
* `input` should be a `buffer` of any size

### `instance.final`
``` js
instance.final(output)
```
Finalizes the instance.
* `output` should be a `buffer` of `length crypto_hash_sha512_BYTES`

The generated hash is stored in `output`.

## Stateful API
Replaces the above instance implementation in the N-API release
### `crypto_hash_sha256_init`
![sodium-native][node]
```js
var state = Buffer.alloc(crypto_hash_sha256_STATEBYTES)

sodium.crypto_hash_sha256_init(state, [key], outlen)
```
Initialise a new hash state.
* `state` must be a buffer of length `crypto_hash_sha256_STATEBYTES` bytes

### `crypto_hash_sha256_update`
![sodium-native][node]
```js
sodium.crypto_hash_sha256_update(state, in)
```
Update a hash state with a given input.
* `state` must be a buffer of length `crypto_hash_sha256_STATEBYTES` bytes
* `in` should be a a `buffer` of any length

### `crypto_hash_sha256_final(state, out)`
![sodium-native][node]
```js
sodium.crypto_hash_sha256_final(state, out)
```
Finalize a given hash state and write the digest to `output` buffer.
* `state` must be a buffer of length `crypto_hash_sha256_STATEBYTES` bytes
* `out` must be a `buffer` of byte length `crypto_hash_sha256_BYTES` bytes

### `crypto_hash_sha512_init`
![sodium-native][node]
```js
var state = Buffer.alloc(crypto_hash_sha512_STATEBYTES)

sodium.crypto_hash_sha512_init(state, [key], outlen)
```
Initialise a new hash state.
* `state` must be a buffer of length `crypto_hash_sha512_STATEBYTES` bytes

### `crypto_hash_sha512_update`
![sodium-native][node]
```js
sodium.crypto_hash_sha512_update(state, in)
```
Update a hash state with a given input.
* `state` must be a buffer of length `crypto_hash_sha512_STATEBYTES` bytes
* `in` should be a a `buffer` of any length

### `crypto_hash_sha512_final(state, out)`
![sodium-native][node]
```js
sodium.crypto_hash_sha512_final(state, out)
```
Finalize a given hash state and write the digest to `output` buffer.
* `state` must be a buffer of length `crypto_hash_sha512_STATEBYTES` bytes
* `out` must be a `buffer` of byte length `crypto_hash_sha512_BYTES` bytes

[js]: /docs/img/icon_js.svg
[node]: /docs/img/nodejs-icon.svg
