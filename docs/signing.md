---
id: signing
title: Signing
sidebar_label: Signing
---

Bindings for the crypto_sign API. [See the libsodium crypto_sign docs for more information](https://download.libsodium.org/doc/public-key_cryptography/public-key_signatures).

## Constants
**Buffer lengths (integer)**
* `crypto_sign_PUBLICKEYBYTES`
* `crypto_sign_SECRETKEYBYTES`
* `crypto_sign_SEEDBYTES`
* `crypto_sign_BYTES`
* `crypto_box_PUBLICKEYBYTES`
* `crypto_box_SECRETKEYBYTES`

***
## `crypto_sign_seed25519_keypair`
![sodium-native][node] ![sodium-javascript][js]
``` js
sodium.crypto_sign_seed25519_keypair(pk, sk, seed)
```
Creates a new keypair based on a `seed`.
* `pk` should be a `buffer` of length `crypto_sign_PUBLICKEYBYTES`
* `sk` should be a `buffer` of length `crypto_sign_SECRETKEYBYTES`
* `seed` should be a `buffer` of length `crypto_sign_SEEDBYTES`

The generated public and secret key will be stored in `buffers`.
***
## `crypto_sign_keypair`
![sodium-native][node] ![sodium-javascript][js]
``` js
sodium.crypto_sign_keypair(pk, sk)
```
Creates a new keypair.
* `pk` should be a `buffer` of length `crypto_sign_PUBLICKEYBYTES`
* `sk` should be a `buffer` of length `crypto_sign_SECRETKEYBYTES`

The generated public and secret key will be stored in `buffers`.
***
## `crypto_sign`
![sodium-native][node] ![sodium-javascript][js]
``` js
sodium.crypto_sign(sm, m, sk)
```
Signs a message.
* `sm` should be a `buffer` of length `crypto_sign_BYTES + m.length`
* `m` should be a `buffer` of any length
* `sk` should be a secret key

The generated signed message will be stored in `sm`.
***
## `crypto_sign_open`
![sodium-native][node] ![sodium-javascript][js]
``` js
var bool = sodium.crypto_sign_open(m, sm, pk)
```
Verifies and opens a message.
* `m` should be a `buffer` of length `sm.length - crypto_sign_BYTES`
* `sm` of length at least `crypto_sign_BYTES`
* `pk` should be a public key

Will return `true` if the message could be verified. Otherwise `false`. If verified, the originally signed message is stored in the `message buffer`.
***
## `crypto_sign_detached`
![sodium-native][node] ![sodium-javascript][js]
``` js
sodium.crypto_sign_detached(sig, m, sk)
```
Same as `crypto_sign`, except that it only stores the signature.
* `sig` should be a `buffer` of length `crypto_sign_BYTES`
* `m` should be a `buffer` of any length
* `sk` should be a secret key

The generated signature is stored in `signature`.
***
## `crypto_sign_verify_detached`
![sodium-native][node] ![sodium-javascript][js]
``` js
var bool = sodium.crypto_sign_verify_detached(sig, m, pk)
```
Verifies a signature.
* `sig` should be a `buffer` of length `crypto_sign_BYTES`
* `m` should be a `buffer` of any length
* `pk` should be a public key

Will return `true` if the message could be verified. Otherwise `false`.
***
## `crypto_sign_ed25519_pk_to_curve25519`
![sodium-native][node]
``` js
sodium.crypto_sign_ed25519_pk_to_curve25519(x25519_pk, ed25519_pk)
```
Converts an `ed25519` public key to `curve25519` (which can be used with `box` and `scalarmult`).
* `x25519_pk` should be a `buffer` of length `crypto_box_PUBLICKEYBYTES`
* `ed25519_pk` should be a `buffer` of length `crypto_sign_PUBLICKEYBYTES`
***
## `crypto_sign_ed25519_sk_to_curve25519`
![sodium-native][node]
``` js
sodium.crypto_sign_ed25519_sk_to_curve25519(x25519_sk, ed25519_sk)
```
Converts an `ed25519` secret key to `curve25519` (which can be used with `box` and `scalarmult`).
* `x25519_sk` should be a `buffer` of length `crypto_box_SECRETKEYBYTES`
* `ed25519_sk` should be a `buffer` of length `crypto_sign_SECRETKEYBYTES`
***
## `crypto_sign_ed25519_sk_to_pk`
![sodium-native][node]
``` js
sodium.crypto_sign_ed25519_sk_to_pk(pk, sk)
```
Extracts an `ed25519` public key from an `ed25519` secret key.
* `pk` must be a `buffer` of at least `crypto_box_PUBLICKEYBYTES` bytes
* `sk` must be a `buffer` of at least `crypto_sign_SECRETKEYBYTES` bytes


[js]: /docs/img/icon_js.svg
[node]: /docs/img/nodejs-icon.svg
