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
## `crypto_onetimeauth_instance`
![sodium-native][node]
``` js
var instance = sodium.crypto_onetimeauth_instance(key)
```
Creates an instance that creates a token from a onetime key and a stream of input data.
* `key` should be a `buffer` of length `crypto_onetimeauth_KEYBYTES`

## `instance.update`
``` js
instance.update(input)
```
Updates the instance with a new piece of data.
* `input` should be a `buffer` of any length

## `instance.final`
``` js
instance.final(output)
```
Finalizes the instance.
* `output` should be a `buffer` of length `crypto_onetimeauth_BYTES`

The generated hash is stored in `output`.


[js]: /docs/img/icon_js.svg
[node]: /docs/img/nodejs-icon.svg
