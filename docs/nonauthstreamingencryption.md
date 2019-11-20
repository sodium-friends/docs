---
id: nonauthstreamingencryption
title: Non-Authenticated Streaming Encryption
sidebar_label: Non-Authenticated Streaming Encryption
---

Bindings for the crypto_stream API. [See the libsodium crypto_stream docs for more information](https://download.libsodium.org/doc/advanced/stream_ciphers/xsalsa20).

## Constants
**Buffer lengths (integer)**
* `crypto_stream_NONCEBYTES`
* `crypto_stream_KEYBYTES`

**String constants (string)**
* `crypto_stream_PRIMITIVE`

***
## `crypto_stream`
![sodium-native][node] ![sodium-javascript][js]
``` js
sodium.crypto_stream(ciphertext, nonce, key)
```
Generates random data based on a `nonce` and `key` into the `ciphertext`.
* `ciphertext` should be a `buffer` of any size
* `nonce` should be a `buffer` of length `crypto_stream_NONCEBYTES`
* `key` should be a secret key of length `crypto_stream_KEYBYTES`

The generated data is stored in `ciphertext`.
***
## `crypto_stream_xor`
![sodium-native][node] ![sodium-javascript][js]
``` js
sodium.crypto_stream_xor(ciphertext, message, nonce, key)
```
Encrypts, but *not* authenticates, a `message` based on a `nonce` and `key`
* `ciphertext` should be a `buffer` of length `message.length`
* `message` should be a `buffer` of any size
* `nonce` should be a `buffer` of length `crypto_stream_NONCEBYTES`
* `key` should be a secret key of length `crypto_stream_KEYBYTES`

The encrypted data is stored in `ciphertext`. To decrypt, swap `ciphertext` and `message`. Also supports in-place encryption where you use the same `buffer` as `ciphertext` and `message`.

Encryption defaults to `XSalsa20`, use `crypto_stream_chacha20_xor` if you want to encrypt/decrypt with `ChaCha20` instead.
***
## `crypto_stream_chacha20_xor`
![sodium-native][node]
``` js
sodium.crypto_stream_chacha20_xor(ciphertext, message, nonce, key)
```
Encrypts, but *not* authenticates, a `message` based on a `nonce` and `key`
* `ciphertext` should be a `buffer` of length `message.length`
* `message` should be a `buffer` of any size
* `nonce` should be a `buffer` of length `crypto_stream_NONCEBYTES`
* `key` should be a secret key of length `crypto_stream_KEYBYTES`

The encrypted data is stored in `ciphertext`. To decrypt, swap `ciphertext` and `message`. Also supports in-place encryption where you use the same `buffer` as `ciphertext` and `message`.

Encryption defaults to `XSalsa20`, use `crypto_stream_chacha20_xor` if you want to encrypt/decrypt with `ChaCha20` instead.
***
## `crypto_stream_xor_instance`
![sodium-native][node] ![sodium-javascript][js]
``` js
var instance = sodium.crypto_stream_xor_instance(nonce, key)
```
A streaming instance to the `crypto_stream_xor` API. Pass a `nonce` and `key` in the constructor.

Encryption defaults to `XSalsa20`, use `crypto_stream_chacha20_xor_instance` if you want to encrypt/decrypt with `ChaCha20` instead.

## `crypto_stream_chacha20_xor_instance`
![sodium-native][node]
``` js
var instance = sodium.crypto_stream_chacha20_xor_instance(nonce, key)
```
A streaming instance to the `crypto_stream_xor` API. Pass a `nonce` and `key` in the constructor.

Encryption defaults to `XSalsa20`, use `crypto_stream_chacha20_xor_instance` if you want to encrypt/decrypt with `ChaCha20` instead.

## `instance.update`
``` js
instance.update(ciphertext, message)
```
Encrypts the next message.

## `instance.final`
``` js
instance.final()
```
Finalizes the stream. Zeros out internal state.


[js]: /docusaurus/img/icon_js.svg
[node]: /docusaurus/img/nodejs-icon.svg
