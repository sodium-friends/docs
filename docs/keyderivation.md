---
id: keyderivation
title: Key Derivation
sidebar_label: Key Derivation
---

Bindings for the crypto_kdf API. [See the libsodium crypto_kdf docs for more information](https://download.libsodium.org/doc/key_derivation/).

## Constants
**Buffer lengths (integer)**
* `crypto_kdf_KEYBYTES`
* `crypto_kdf_BYTES_MIN`
* `crypto_kdf_BYTES_MAX`
* `crypto_kdf_CONTEXTBYTES`

**String constants (string)**
* `crypto_kdf_PRIMITIVE`

***
## `crypto_kdf_keygen`
![sodium-native][node] ![sodium-javascript][js]
``` js
sodium.crypto_kdf_keygen(key)
```
Generates a new master key.
* `key` should be a `buffer` of length `crypto_kdf_KEYBYTES`

***
## `crypto_kdf_derive_from_key`
![sodium-native][node] ![sodium-javascript][js]
``` js
sodium.crypto_kdf_derive_from_key(subkey, subkeyId, ctx, key)
```
Derives a new key from a master key.
* `subkey` should be a `buffer` between `crypto_kdf_BYTES_MIN` and `crypto_kdf_BYTES_MAX`
* `subkeyId` should be an integer
* `ctx` should be a `buffer` of length `crypto_kdf_CONTEXTBYTES`
* `key` should be a `buffer` of length `crypto_kdf_KEYBYTES`


[js]: /docs/img/icon_js.svg
[node]: /docs/img/nodejs-icon.svg
