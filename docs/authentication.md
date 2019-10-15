---
id: authentication
title: Authentication
sidebar_label: Authentication
---

Bindings for the crypto_auth API. [See the libsodium crypto_auth docs for more information](https://download.libsodium.org/doc/secret-key_cryptography/secret-key_authentication).

``` js
crypto_auth(output, input, key)
```
Create an authentication token.
* `output` should be a `buffer` of length `crypto_auth_BYTES`
* `input` should be a `buffer` of any size
* `key` should be a `buffer` of length `crypto_auth_KEYBYTES`

The generated token is stored in `output`.

``` js
var bool = crypto_auth_verify(output, input, key)
```
Verify a token.
* `output` should be a `buffer` of length `crypto_auth_BYTES`
* `input` should be a `buffer` of any size
* `key` should be a `buffer` of length `crypto_auth_KEYBYTES`

Returns `true` if the token could be verified. Otherwise `false`.
