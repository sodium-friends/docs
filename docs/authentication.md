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
sodium.crypto_auth(output, input, key)
```
Creates an authentication token.
* `output` should be a `buffer` of length `crypto_auth_BYTES`
* `input` should be a `buffer` of any size
* `key` should be a `buffer` of length `crypto_auth_KEYBYTES`

The generated token is stored in `output`.
***
## `crypto_auth_verify`
![sodium-native][node]
``` js
var bool = sodium.crypto_auth_verify(output, input, key)
```
Verifies a token.
* `output` should be a `buffer` of length `crypto_auth_BYTES`
* `input` should be a `buffer` of any size
* `key` should be a `buffer` of length `crypto_auth_KEYBYTES`

Returns `true` if the token could be verified. Otherwise `false`.


[js]: /docusaurus/img/icon_js.svg
[node]: /docusaurus/img/nodejs-icon.svg
