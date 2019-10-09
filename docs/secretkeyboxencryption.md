---
id: secretkeyboxencryption
title: Secret key box encryption
sidebar_label: Secret key box encryption
---

Bindings for the crypto_secretbox API. [See the libsodium crypto_secretbox docs for more information](https://download.libsodium.org/doc/public-key_cryptography/authenticated_encryption).

``` js
crypto_secretbox_detached(ciphertext, mac, message, nonce, secretKey)
```
Encrypt a message.
* `ciphertext` should be a buffer with length `message.length`
* `mac` should be a buffer with length `crypto_secretbox_MACBYTES`
* `message` should be a buffer of any length
* `nonce` should be a buffer with length `crypto_secretbox_NONCEBYTES`
* `secretKey` should be a secret key with length `crypto_secretbox_KEYBYTES`

The encrypted message will be stored in `ciphertext` and the authentification code will be stored in `mac`.

``` js
crypto_secretbox_easy(ciphertext, message, nonce, secretKey)
```
Same as `crypto_secretbox_detached` except it encodes the mac in the message.
* `ciphertext` should be a buffer with length `message.length + crypto_secretbox_MACBYTES`
* `message` should be a buffer of any length
* `nonce` should be a buffer with length `crypto_secretbox_NONCEBYTES`
* `secretKey` should be a secret key with length `crypto_secretbox_KEYBYTES`

``` js
var bool = crypto_secretbox_open_detached(message, ciphertext, mac, nonce, secretKey)
```
Decrypt a message.
* `message` should be a buffer with length `ciphertext.length`
* `mac` should be a buffer with length `crypto_secretbox_MACBYTES`
* `ciphertext` should be a buffer of any length
* `nonce` should be a buffer with length `crypto_secretbox_NONCEBYTES`
* `secretKey` should be a secret key

Returns `true` if the message could be decrypted. Otherwise `false`.

The decrypted message will be stored in `message`.

``` js
var bool = crypto_secretbox_open_easy(message, ciphertext, nonce, secretKey)
```
Decrypt a message encoded with the easy method.
* `message` should be a buffer with length `ciphertext.length - crypto_secretbox_MACBYTES`
* `ciphertext` should be a buffer with length at least `crypto_secretbox_MACBYTES`
* `nonce` should be a buffer with length `crypto_secretbox_NONCEBYTES`
* `secretKey` should be a secret key

Returns `true` if the message could be decrypted. Otherwise `false`.

The decrypted message will be stored in `message`.
