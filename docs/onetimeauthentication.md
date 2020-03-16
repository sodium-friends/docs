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
sodium.crypto_onetimeauth(out, in, k)
```
Creates an authentication token based on a onetime k.
* `out` should be a `buffer` of length `crypto_onetimauth_BYTES`
* `in` should be a `buffer` of any size
* `k` should be a `buffer` of length `crypto_onetimeauth_KEYBYTES`

The generated token is stored in `out`.
***
## `crypto_onetimeauth_verify`
![sodium-native][node]
``` js
var bool = sodium.crypto_onetimeauth_verify(out, in, k)
```
Verifies a token.
* `out` should be a `buffer` of length `crypto_onetimeauth_BYTES`
* `in` should be a `buffer` of any size
* `k` should be a `buffer` of length `crypto_onetimeauth_KEYBYTES`

Returns `true` if the token could be verified. Otherwise `false`.
***
## Instance API
## `crypto_onetimeauth_instance`
![sodium-native][node]
``` js
var instance = sodium.crypto_onetimeauth_instance(k)
```
Creates an instance that creates a token from a onetime k and a stream of in data.
* `k` should be a `buffer` of length `crypto_onetimeauth_KEYBYTES`

## `instance.update`
``` js
instance.update(in)
```
Updates the instance with a new piece of data.
* `in` should be a `buffer` of any length

## `instance.final`
``` js
instance.final(out)
```
Finalizes the instance.
* `out` should be a `buffer` of length `crypto_onetimeauth_BYTES`

The generated hash is stored in `out`.

## Stateful API
Replaces the above instance implementation in the N-API release
## `crypto_onetimeauth_init`
![sodium-native][node]
```js
var state = Buffer.alloc(crypto_onetimeauth_STATEBYTES)

sodium.crypto_onetimeauth_init(state, k)
```
Initialise a new auth state with a k.
* `state` must be a buffer of length `crypto_onetimeauth_STATEBYTES` bytes
* `k` must be a buffer of length `crypto_onetimeauth_STATEBYTES` bytes

## `crypto_onetimeauth_update`
![sodium-native][node]
```js
sodium.crypto_onetimeauth_update(state, in)
```
Update a hash state with a given in.
* `state` must be a buffer of length `crypto_onetimeauth_STATEBYTES` bytes
* `in` should be a a `buffer` of any length

## `crypto_onetimeauth_final(state, out)`
![sodium-native][node]
```js
sodium.crypto_onetimeauth_final(state, out)
```
Finalize a given hash state and write the digest to `out` buffer
* `state` must be a buffer of length `crypto_onetimeauth_STATEBYTES` bytes
* `out` must be a `buffer` of length `crypto_onetimeauth_BYTES` bytes

[js]: /docs/img/icon_js.svg
[node]: /docs/img/nodejs-icon.svg
