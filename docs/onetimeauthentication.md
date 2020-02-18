---
id: onetimeauthentication
title: One-Time Authentication
sidebar_label: One-Time Authentication
---

Bindings for the crypto_onetimeauth API. [See the libsodium crypto_onetimeauth docs for more information](https://download.libsodium.org/doc/advanced/poly1305).

## Constants
**Buffer lengths (integer)**
* `crypto_onetimeauth_BYTES`
* `crypto_onetimeauth_KEYBYTES`
* `crypto_onetimeauth_STATEBYTES`

**String constants (string)**
* `crypto_onetimeauth_PRIMITIVE`

***
## `crypto_onetimeauth`
![sodium-native][node]
``` js
sodium.crypto_onetimeauth(output, input, key)
```
Creates an authentication token based on a onetime key.
* `output` should be a `buffer` of length `crypto_onetimauth_BYTES`
* `input` should be a `buffer` of any size
* `key` should be a `buffer` of length `crypto_onetimeauth_KEYBYTES`

The generated token is stored in `output`.
***
## `crypto_onetimeauth_verify`
![sodium-native][node]
``` js
var bool = sodium.crypto_onetimeauth_verify(output, input, key)
```
Verifies a token.
* `output` should be a `buffer` of length `crypto_onetimeauth_BYTES`
* `input` should be a `buffer` of any size
* `key` should be a `buffer` of length `crypto_onetimeauth_KEYBYTES`

Returns `true` if the token could be verified. Otherwise `false`.
***
## Instance API
### `crypto_onetimeauth_instance`
![sodium-native][node]
``` js
var instance = sodium.crypto_onetimeauth_instance(key)
```
Creates an instance that creates a token from a onetime key and a stream of input data.
* `key` should be a `buffer` of length `crypto_onetimeauth_KEYBYTES`

### `instance.update`
``` js
instance.update(input)
```
Updates the instance with a new piece of data.
* `input` should be a `buffer` of any length

### `instance.final`
``` js
instance.final(output)
```
Finalizes the instance.
* `output` should be a `buffer` of length `crypto_onetimeauth_BYTES`

The generated hash is stored in `output`.

## Stateful API
Replaces the above instance implementation in the N-API release
### `crypto_onetimeauth_init`
![sodium-native][node]
```js
var state = Buffer.alloc(crypto_onetimeauth_STATEBYTES)

sodium.crypto_onetimeauth_init(state, key)
```
Initialise a new auth state with a key.
* `state` must be a buffer of length `crypto_onetimeauth_STATEBYTES` bytes
* `key` must be a buffer of length `crypto_onetimeauth_STATEBYTES` bytes

### `crypto_onetimeauth_update`
![sodium-native][node]
```js
sodium.crypto_onetimeauth_update(state, in)
```
Update a hash state with a given input.
* `state` must be a buffer of length `crypto_onetimeauth_STATEBYTES` bytes
* `in` should be a a `buffer` of any length

### `crypto_onetimeauth_final(state, out)`
![sodium-native][node]
```js
sodium.crypto_onetimeauth_final(state, out)
```
Finalize a given hash state and write the digest to `output` buffer
* `state` must be a buffer of length `crypto_onetimeauth_STATEBYTES` bytes
* `out` must be a `buffer` of length `crypto_onetimeauth_BYTES` bytes

[js]: /docs/img/icon_js.svg
[node]: /docs/img/nodejs-icon.svg
