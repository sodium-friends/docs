---
id: keyboxencryption
title: Public/Secret Key Box Encryption
sidebar_label: Public/Secret Key Box Encryption
---

Bindings for the crypto_box API. [See the libsodium crypto_box docs for more information](https://download.libsodium.org/doc/public-key_cryptography/authenticated_encryption).

## Constants
**Buffer lengths (integer)**
* `crypto_box_PUBLICKEYBYTES`
* `crypto_box_SECRETKEYBYTES`
* `crypto_box_SEEDBYTES`
* `crypto_box_MACBYTES`
* `crypto_box_NONCEBYTES`

**String constants (string)**
* `crypto_box_PRIMITIVE`

***
## `crypto_box_seed_keypair`
![sodium-native][node]
``` js
sodium.crypto_box_seed_keypair(pk, sk, seed)
```
Creates a new keypair based on a `seed`.
* `pk` should be a `buffer` of length `crypto_box_PUBLICKEYBYTES`
* `sk` should be a `buffer` of length `crypto_box_SECRETKEYBYTES`
* `seed` should be a `buffer` of length `crypto_box_SEEDBYTES`

The generated public and secret key will be stored in `buffer`'s.
***
## `crypto_box_keypair`
![sodium-native][node]
``` js
sodium.crypto_box_keypair(pk, sk)
```
Creates a new keypair.
* `pk` should be a `buffer` of length `crypto_box_PUBLICKEYBYTES`
* `sk` should be a `buffer` of length `crypto_box_SECRETKEYBYTES`

The generated public and secret key will be stored in `buffer`'s.
***
## `crypto_box_detached`
![sodium-native][node]
``` js
sodium.crypto_box_detached(c, mac, m, n, pk, sk)
```
Encrypts a message.
* `c` should be a `buffer` of length `m.length`
* `mac` should be a `buffer` of length `crypto_box_MACBYTES`
* `m` should be a `buffer` of any length
* `n` should be a `buffer` of length `crypto_box_NONCEBYTES`
* `pk` should be a public key
* `sk` should be a secret key

The encrypted message will be stored in `c` and the authentification code will be stored in `mac`.
***
## `crypto_box_easy`
![sodium-native][node]
``` js
sodium.crypto_box_easy(c, m, n, pk, sk)
```
Same as `crypto_box_detached`, except that it encodes the `mac` in the message.
* `c` should be a `buffer` of length `m.length + crypto_box_MACBYTES`
* `m` should be a `buffer` of any length
* `n` should be a `buffer` of length `crypto_box_NONCEBYTES`
* `pk` should be a public key
* `sk` should be a secret key

The encrypted message and authentification code will be stored in `c`.
***
## `crypto_box_open_detached`
![sodium-native][node]
``` js
var bool = sodium.crypto_box_open_detached(m, c, mac, n, pk, sk)
```
Decrypts ciphertext `c` into message `m`.
* `m` should be a `buffer` of length `c.length`
* `mac` should be a `buffer` of length `crypto_box_MACBYTES`
* `c` should be a `buffer` of any length
* `n` should be a `buffer` of length `crypto_box_NONCEBYTES`
* `pk` should be a public key
* `sk` should be a secret key

Returns `true` if the message could be decrypted. Otherwise `false`.

The decrypted message will be stored in `m`.
***
## `crypto_box_open_easy`
![sodium-native][node]
``` js
var bool = sodium.crypto_box_open_easy(m, c, n, pk, sk)
```
Decrypts a ciphertext encoded with the easy method.
* `m` should be a `buffer` of length `c.length - crypto_box_MACBYTES`
* `c` should be a `buffer` of length at least `crypto_box_MACBYTES`
* `n` should be a `buffer` of length `crypto_box_NONCEBYTES`
* `pk` should be a public key
* `sk` should be a secret key

Returns `true` if the message could be decrypted. Otherwise `false`.

The decrypted message will be stored in `m`.


[js]: /docs/img/icon_js.svg
[node]: /docs/img/nodejs-icon.svg
