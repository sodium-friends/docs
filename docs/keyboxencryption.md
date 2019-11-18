---
id: keyboxencryption
title: Public/Secret Key Box Encryption
sidebar_label: Public/Secret Key Box Encryption
---

Bindings for the crypto_box API. [See the libsodium crypto_box docs for more information](https://download.libsodium.org/doc/public-key_cryptography/authenticated_encryption).
***
## `crypto_box_seed_keypair`
![sodium-node][node]
``` js
sodium.crypto_box_seed_keypair(publicKey, secretKey, seed)
```
Creates a new keypair based on a `seed`.
* `publicKey` should be a `buffer` of length `crypto_box_PUBLICKEYBYTES`
* `secretKey` should be a `buffer` of length `crypto_box_SECRETKEYBYTES`
* `seed` should be a `buffer` of length `crypto_box_SEEDBYTES`

The generated public and secret key will be stored in `buffer`'s.
***
## `crypto_box_keypair`
![sodium-node][node]
``` js
sodium.crypto_box_keypair(publicKey, secretKey)
```
Creates a new keypair.
* `publicKey` should be a `buffer` of length `crypto_box_PUBLICKEYBYTES`
* `secretKey` should be a `buffer` of length `crypto_box_SECRETKEYBYTES`

The generated public and secret key will be stored in `buffer`'s.
***
## `crypto_box_detached`
![sodium-node][node]
``` js
sodium.crypto_box_detached(ciphertext, mac, message, nonce, publicKey, secretKey)
```
Encrypts a message.
* `ciphertext` should be a `buffer` of length `message.length`
* `mac` should be a `buffer` of length `crypto_box_MACBYTES`
* `message` should be a `buffer` of any length
* `nonce` should be a `buffer` of length `crypto_box_NONCEBYTES`
* `publicKey` should be a public key
* `secretKey` should be a secret key

The encrypted message will be stored in `ciphertext` and the authentification code will be stored in `mac`.
***
## `crypto_box_easy`
![sodium-node][node]
``` js
sodium.crypto_box_easy(ciphertext, message, nonce, publicKey, secretKey)
```
Same as `crypto_box_detached`, except that it encodes the `mac` in the message.
* `ciphertext` should be a `buffer` of length `message.length + crypto_box_MACBYTES`
* `message` should be a `buffer` of any length
* `nonce` should be a `buffer` of length `crypto_box_NONCEBYTES`
* `publicKey` should be a public key
* `secretKey` should be a secret key

The encrypted message and authentification code will be stored in `ciphertext`.
***
## `crypto_box_open_detached`
![sodium-node][node]
``` js
var bool = sodium.crypto_box_open_detached(message, ciphertext, mac, nonce, publicKey, secretKey)
```
Decrypts a message.
* `message` should be a `buffer` of length `ciphertext.length`
* `mac` should be a `buffer` of length `crypto_box_MACBYTES`
* `ciphertext` should be a `buffer` of any length
* `nonce` should be a `buffer` of length `crypto_box_NONCEBYTES`
* `publicKey` should be a public key
* `secretKey` should be a secret key

Returns `true` if the message could be decrypted. Otherwise `false`.

The decrypted message will be stored in `message`.
***
## `crypto_box_open_easy`
![sodium-node][node]
``` js
var bool = sodium.crypto_box_open_easy(message, ciphertext, nonce, publicKey, secretKey)
```
Decrypts a message encoded with the easy method.
* `message` should be a `buffer` of length `ciphertext.length - crypto_box_MACBYTES`
* `ciphertext` should be a `buffer` of length at least `crypto_box_MACBYTES`
* `nonce` should be a `buffer` of length `crypto_box_NONCEBYTES`
* `publicKey` should be a public key
* `secretKey` should be a secret key

Returns `true` if the message could be decrypted. Otherwise `false`.

The decrypted message will be stored in `message`.


[js]: /docusaurus/img/icon_js.svg
[node]: /docusaurus/img/nodejs-icon.svg
