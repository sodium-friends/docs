---
id: keyboxencryption
title: Public/secret key box encryption
sidebar_label: Public/secret key box encryption
---

Bindings for the crypto_box API. [See the libsodium crypto_box docs for more information](https://download.libsodium.org/doc/public-key_cryptography/authenticated_encryption).

``` js
crypto_box_seed_keypair(publicKey, secretKey, seed)
```
Create a new keypair based on a seed.
* `publicKey` should be a buffer with length `crypto_box_PUBLICKEYBYTES`
* `secretKey` should be a buffer with length `crypto_box_SECRETKEYBYTES`
* `seed` should be a buffer with length `crypto_box_SEEDBYTES`

The generated public and secret key will be stored in passed in buffers.

``` js
crypto_box_keypair(publicKey, secretKey)
```
Create a new keypair.
* `publicKey` should be a buffer with length `crypto_box_PUBLICKEYBYTES`
* `secretKey` should be a buffer with length `crypto_box_SECRETKEYBYTES`

The generated public and secret key will be stored in passed in buffers.

``` js
crypto_box_detached(ciphertext, mac, message, nonce, publicKey, secretKey)
```
Encrypt a message.
* `ciphertext` should be a buffer with length `message.length`
* `mac` should be a buffer with length `crypto_box_MACBYTES`
* `message` should be a buffer of any length
* `nonce` should be a buffer with length `crypto_box_NONCEBYTES`
* `publicKey` should be a public key
* `secretKey` should be a secret key

The encrypted message will be stored in `ciphertext` and the authentification code will be stored in `mac`.

``` js
crypto_box_easy(ciphertext, message, nonce, publicKey, secretKey)
```
Same as `crypto_box_detached` except it encodes the `mac` in the message.
* `ciphertext` should be a buffer with length `message.length + crypto_box_MACBYTES`
* `message` should be a buffer of any length
* `nonce` should be a buffer with length `crypto_box_NONCEBYTES`
* `publicKey` should be a public key
* `secretKey` should be a secret key

The encrypted message and authentification code will be stored in `ciphertext`.

``` js
var bool = crypto_box_open_detached(message, ciphertext, mac, nonce, publicKey, secretKey)
```
Decrypt a message.
* `message` should be a buffer with length `ciphertext.length`
* `mac` should be a buffer with length `crypto_box_MACBYTES`
* `ciphertext` should be a buffer of any length
* `nonce` should be a buffer with length `crypto_box_NONCEBYTES`
* `publicKey` should be a public key
* `secretKey` should be a secret key

Returns `true` if the message could be decrypted. Otherwise `false`.

The decrypted message will be stored in `message`.

``` js
var bool = crypto_box_open_easy(message, ciphertext, nonce, publicKey, secretKey)
```
Decrypt a message encoded with the easy method.
* `message` should be a buffer with length `ciphertext.length - crypto_box_MACBYTES`
* `ciphertext` should be a buffer with length at least `crypto_box_MACBYTES`
* `nonce` should be a buffer with length `crypto_box_NONCEBYTES`
* `publicKey` should be a public key
* `secretKey` should be a secret key

Returns `true` if the message could be decrypted. Otherwise `false`.

The decrypted message will be stored in `message`.
