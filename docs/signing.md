---
id: signing
title: signing
sidebar_label: signing
---

Bindings for the crypto_sign API. [See the libsodium crypto_sign docs for more information](https://download.libsodium.org/doc/public-key_cryptography/public-key_signatures).

``` js
crypto_sign_seed_keypair(publicKey, secretKey, seed)
```
Create a new keypair based on a seed.
* `publicKey` should be a buffer with length `crypto_sign_PUBLICKEYBYTES`
* `secretKey` should be a buffer with length `crypto_sign_SECRETKEYBYTES`
* `seed` should be a buffer with length `crypto_sign_SEEDBYTES`

The generated public and secret key will be stored in passed in `buffers`.

``` js
crypto_sign_keypair(publicKey, secretKey)
```
Create a new keypair.
* `publicKey` should be a buffer with length `crypto_sign_PUBLICKEYBYTES`
* `secretKey` should be a buffer with length `crypto_sign_SECRETKEYBYTES`

The generated public and secret key will be stored in passed in `buffers`.

``` js
crypto_sign(signedMessage, message, secretKey)
```
Sign a message.
* `signedMessage` should be a buffer with length `crypto_sign_BYTES + message.length`
* `message` should be a buffer of any length
* `secretKey` should be a secret key

The generated signed message will be stored in `signedMessage`.

``` js
var bool = crypto_sign_open(message, signedMessage, publicKey)
```
Verify and open a message.
* `message` should be a buffer with length `signedMessage.length - crypto_sign_BYTES`
* `signedMessage` at least `crypto_sign_BYTES` length
* `publicKey` should be a public key

Will return `true` if the message could be verified. Otherwise `false`. If verified, the originally signed message is stored in the `message buffer`.

``` js
crypto_sign_detached(signature, message, secretKey)
```
Same as `crypto_sign` except it only stores the signature.
* `signature` should be a buffer with length `crypto_sign_BYTES`
* `message` should be a buffer of any length
* `secretKey` should be a secret key

The generated signature is stored in `signature`.

``` js
var bool = crypto_sign_verify_detached(signature, message, publicKey)
```
Verify a signature.
* `signature` should be a buffer with length `crypto_sign_BYTES`
* `message` should be a buffer of any length
* `publicKey` should be a public key

Will return `true` if the message could be verified. Otherwise `false`.

``` js
crypto_sign_ed25519_pk_to_curve25519(curve_pk, ed_pk)
```
Convert an ed25519 public key to curve25519 (which can be used with `box` and `scalarmult`).
* `curve_pk` should be a buffer with length `crypto_box_PUBLICKEYBYTES`
* `ed_pk` should be a buffer with length `crypto_sign_PUBLICKEYBYTES`

``` js
crypto_sign_ed25519_sk_to_curve25519(curve_sk, ed_sk)
```
Convert an ed25519 secret key to curve25519 (which can be used with `box` and `scalarmult`).
* `curve_sk` should be a buffer with length `crypto_box_SECRETKEYBYTES`
* `ed_sk` should be a buffer with length `crypto_sign_SECRETKEYBYTES`

``` js
crypto_sign_ed25519_sk_to_pk(pk, sk)
```
Extract an ed25519 public key from an ed25519 secret key.
* `pk` must be `buffer` of at least `crypto_box_PUBLICKEYBYTES` bytes
* `sk` must be `buffer` of at least `crypto_sign_SECRETKEYBYTES` bytes
