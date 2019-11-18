---
id: secretkeyboxencryption
title: Secret Key Box Encryption
sidebar_label: Secret Key Box Encryption
---

Bindings for the crypto_secretbox API. [See the libsodium crypto_secretbox docs for more information](https://download.libsodium.org/doc/public-key_cryptography/authenticated_encryption).
***
## `crypto_secretbox_detached`
![sodium-node][node] ![sodium-javascript][js]
``` js
sodium.crypto_secretbox_detached(ciphertext, mac, message, nonce, secretKey)
```
Encrypts a message.
* `ciphertext` should be a `buffer` of length `message.length`
* `mac` should be a `buffer` of length `crypto_secretbox_MACBYTES`
* `message` should be a `buffer` of any length
* `nonce` should be a `buffer` of length `crypto_secretbox_NONCEBYTES`
* `secretKey` should be a secret key of length `crypto_secretbox_KEYBYTES`

The encrypted message will be stored in `ciphertext`, and the authentification code will be stored in `mac`.
***
## `crypto_secretbox_easy`
![sodium-node][node] ![sodium-javascript][js]
``` js
sodium.crypto_secretbox_easy(ciphertext, message, nonce, secretKey)
```
Same as `crypto_secretbox_detached`, except that it encodes the `mac` in the `message`.
* `ciphertext` should be a `buffer` of length `message.length + crypto_secretbox_MACBYTES`
* `message` should be a `buffer` of any length
* `nonce` should be a `buffer` of length `crypto_secretbox_NONCEBYTES`
* `secretKey` should be a secret key of length `crypto_secretbox_KEYBYTES`
***
## `crypto_secretbox_open_detached`
![sodium-node][node] ![sodium-javascript][js]
``` js
var bool = sodium.crypto_secretbox_open_detached(message, ciphertext, mac, nonce, secretKey)
```
Decrypts a message.
* `message` should be a `buffer` of length `ciphertext.length`
* `mac` should be a `buffer` of length `crypto_secretbox_MACBYTES`
* `ciphertext` should be a `buffer` of any length
* `nonce` should be a `buffer` of length `crypto_secretbox_NONCEBYTES`
* `secretKey` should be a secret key

Returns `true` if the message could be decrypted. Otherwise `false`.

The decrypted message will be stored in `message`.
***
## `crypto_secretbox_open_easy`
![sodium-node][node] ![sodium-javascript][js]
``` js
var bool = sodium.crypto_secretbox_open_easy(message, ciphertext, nonce, secretKey)
```
Decrypts a message encoded with the easy method.
* `message` should be a `buffer` of length `ciphertext.length - crypto_secretbox_MACBYTES`
* `ciphertext` should be a `buffer` of length at least `crypto_secretbox_MACBYTES`
* `nonce` should be a `buffer` of length `crypto_secretbox_NONCEBYTES`
* `secretKey` should be a secret key

Returns `true` if the message could be decrypted. Otherwise `false`.

The decrypted message will be stored in `message`.


[js]: /docusaurus/img/icon_js.svg
[node]: /docusaurus/img/nodejs-icon.svg
