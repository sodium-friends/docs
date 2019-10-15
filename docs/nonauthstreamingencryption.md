---
id: nonauthstreamingencryption
title: Non-authenticated streaming encryption
sidebar_label: Non-authenticated streaming encryption
---

Bindings for the crypto_stream API. [See the libsodium crypto_stream docs for more information](https://download.libsodium.org/doc/advanced/stream_ciphers/xsalsa20).

``` js
crypto_stream(ciphertext, nonce, key)
```
Generate random data based on a `nonce` and `key` into the `ciphertext`.
* `ciphertext` should be a `buffer` of any size
* `nonce` should be a `buffer` of length `crypto_stream_NONCEBYTES`
* `key` should be a secret key of length `crypto_stream_KEYBYTES`

The generated data is stored in `ciphertext`.

``` js
crypto_stream_xor(ciphertext, message, nonce, key)
```
or
``` js
crypto_stream_chacha20_xor(ciphertext, message, nonce, key)
```
Encrypt, but not authenticate, a `message` based on a `nonce` and `key`
* `ciphertext` should be a `buffer` of length `message.length`
* `message` should be a `buffer` of any size
* `nonce` should be a `buffer` of length `crypto_stream_NONCEBYTES`
* `key` should be a secret key of length `crypto_stream_KEYBYTES`

The encrypted data is stored in `ciphertext`. To decrypt, swap `ciphertext` and `message`. Also supports in-place encryption where you use the same `buffer` as `ciphertext` and `message`.

Encryption defaults to `XSalsa20`, use `crypto_stream_chacha20_xor` if you want to encrypt/decrypt with `ChaCha20` instead.

``` js
var instance = crypto_stream_xor_instance(nonce, key)
```
or
``` js
var istance = crypto_stream_chacha20_xor_instance(nonce, key)
```
A streaming instance to the `crypto_stream_xor` API. Pass a `nonce` and `key` in the constructor.

Encryption defaults to `XSalsa20`, use `crypto_stream_chacha20_xor_instance` if you want to encrypt/decrypt with `ChaCha20` instead.

``` js
instance.update(ciphertext, message)
```
Encrypt the next message.

``` js
instance.final()
```
Finalize the stream. Zeros out internal state.
