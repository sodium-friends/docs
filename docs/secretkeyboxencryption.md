---
id: secretkeyboxencryption
title: Secret Key Box Encryption
sidebar_label: Secret Key Box Encryption
---

Bindings for the crypto_secretbox API. [See the libsodium crypto_secretbox docs for more information](https://download.libsodium.org/doc/public-key_cryptography/authenticated_encryption).

## Constants
**Buffer lengths (integer)**
* `crypto_secretbox_MACBYTES`
* `crypto_secretbox_NONCEBYTES`
* `crypto_secretbox_KEYBYTES`

**String constants (string)**
* `crypto_secretbox_PRIMITIVE`

***
## `crypto_secretbox_detached`
![sodium-native][node] ![sodium-javascript][js]
``` js
sodium.crypto_secretbox_detached(c, mac, m, n, sk)
```
Encrypts a message.
* `c` should be a `buffer` of length `m.length`
* `mac` should be a `buffer` of length `crypto_secretbox_MACBYTES`
* `m` should be a `buffer` of any length
* `n` should be a `buffer` of length `crypto_secretbox_NONCEBYTES`
* `sk` should be a secret key of length `crypto_secretbox_KEYBYTES`

The encrypted message will be stored in `c`, and the authentification code will be stored in `mac`.
***
## `crypto_secretbox_easy`
![sodium-native][node] ![sodium-javascript][js]
``` js
sodium.crypto_secretbox_easy(c, m, n, sk)
```
Same as `crypto_secretbox_detached`, except that it encodes the `mac` in the `message`.
* `c` should be a `buffer` of length `m.length + crypto_secretbox_MACBYTES`
* `m` should be a `buffer` of any length
* `n` should be a `buffer` of length `crypto_secretbox_NONCEBYTES`
* `sk` should be a secret key of length `crypto_secretbox_KEYBYTES`
***
## `crypto_secretbox_open_detached`
![sodium-native][node] ![sodium-javascript][js]
``` js
var bool = sodium.crypto_secretbox_open_detached(m, c, mac, n, sk)
```
Decrypts a message.
* `m` should be a `buffer` of length `c.length`
* `mac` should be a `buffer` of length `crypto_secretbox_MACBYTES`
* `c` should be a `buffer` of any length
* `n` should be a `buffer` of length `crypto_secretbox_NONCEBYTES`
* `sk` should be a secret key

Returns `true` if the ciphertext could be decrypted. Otherwise `false`.

The decrypted message will be stored in `m`.
***
## `crypto_secretbox_open_easy`
![sodium-native][node] ![sodium-javascript][js]
``` js
var bool = sodium.crypto_secretbox_open_easy(m, c, n, sk)
```
Decrypts a message encoded with the easy method.
* `m` should be a `buffer` of length `c.length - crypto_secretbox_MACBYTES`
* `c` should be a `buffer` of length at least `crypto_secretbox_MACBYTES`
* `n` should be a `buffer` of length `crypto_secretbox_NONCEBYTES`
* `sk` should be a secret key

Returns `true` if the ciphertext could be decrypted. Otherwise `false`.

The decrypted message will be stored in `m`.


[js]: /docs/img/icon_js.svg
[node]: /docs/img/nodejs-icon.svg
