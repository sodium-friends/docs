---
id: diffiehellman
title: Diffie-Hellman
sidebar_label: Diffie-Hellman
---

Bindings for the crypto_scalarmult API. [See the libsodium crypto_scalarmult docs for more information](https://download.libsodium.org/doc/advanced/scalar_multiplication).

## Constants
**Buffer lengths (integer)**
* `crypto_scalarmult_BYTES`
* `crypto_scalarmult_SCALARBYTES`

***
## `crypto_scalarmult_base`
![sodium-native][node] ![sodium-javascript][js]
``` js
sodium.crypto_scalarmult_base(q, n)
```
Creates a scalar multiplication public key based on a secret key.
* `q` should be a `buffer` of length `crypto_scalarmult_BYTES`
* `n` should be a `buffer` of length `crypto_scalarmult_SCALARBYTES`

The generated public key is stored in `q`.
***
## `crypto_scalarmult`
![sodium-native][node] ![sodium-javascript][js]
``` js
sodium.crypto_scalarmult(q, n, p)
```
Derives a shared secret from a local secret key and a remote public key.
* `q` should be a `buffer` of length `crypto_scalarmult_BYTES`
* `n` should be a `buffer` of length `crypto_scalarmult_SCALARBYTES`
* `p` should be a `buffer` of length `crypto_scalarmult_BYTES`

The generated shared secret is stored in `q`.


[js]: /docs/img/icon_js.svg
[node]: /docs/img/nodejs-icon.svg
