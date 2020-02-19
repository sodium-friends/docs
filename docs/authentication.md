---
id: authentication
title: Authentication
sidebar_label: Authentication
---

Bindings for the crypto_auth API. [See the libsodium crypto_auth docs for more information](https://download.libsodium.org/doc/secret-key_cryptography/secret-key_authentication).

## Constants
**Buffer lengths (integer)**
* `crypto_auth_BYTES`
* `crypto_auth_KEYBYTES`

**String constants (string)**
* `crypto_auth_PRIMITIVE`

***
## `crypto_auth`
![sodium-native][node]
``` js
sodium.crypto_auth(out, in, k)
```
Creates an authentication token.
* `out` should be a `buffer` of length `crypto_auth_BYTES`
* `in` should be a `buffer` of any size
* `k` should be a `buffer` of length `crypto_auth_KEYBYTES`

The generated token is stored in `out`.
***
## `crypto_auth_verify`
![sodium-native][node]
``` js
var bool = sodium.crypto_auth_verify(out, in, k)
```
Verifies a token.
* `out` should be a `buffer` of length `crypto_auth_BYTES`
* `in` should be a `buffer` of any size
* `k` should be a `buffer` of length `crypto_auth_KEYBYTES`

Returns `true` if the token could be verified. Otherwise `false`.


[js]: /docs/img/icon_js.svg
[node]: /docs/img/nodejs-icon.svg
