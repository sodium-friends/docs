---
id: sealedboxencryption
title: Sealed Box Encryption
sidebar_label: Sealed Box Encryption
---

Bindings for the crypto_box_seal API. [See the libsodium crypto_box_seal docs for more information](https://download.libsodium.org/doc/public-key_cryptography/sealed_boxes).

## Constants
**Buffer lengths (integer)**
* `crypto_box_SEALBYTES`

***
## `crypto_box_seal`
![sodium-native][node]

Keypairs can be generated with `crypto_box_keypair()` or `crypto_box_seed_keypair()`.

``` js
sodium.crypto_box_seal(c, m, pk)
```
Encrypts a message in a sealed box using a throwaway keypair. The c cannot be associated with the sender due to the sender's key being a single use keypair that is overwritten during encryption.
* `c` should be a `buffer` of length at least `m.length + crypto_box_SEALBYTES`
* `m` should be a `buffer` of any length
* `pk` should be the recipient's public key
***
## `crypto_box_seal_open`
![sodium-native][node]
``` js
var bool = sodium.crypto_box_seal_open(m, c, pk, sk)
```
Decrypts a message encoded with the sealed box method.
* `m` should be a `buffer` of length at least `c.length - crypto_box_SEALBYTES`
* `c` should be a `buffer` of length at least `crypto_box_SEALBYTES`
* `pk` should be the recipient's public key
* `sk` should be the recipient's secret key

Note: The keypair of the recipient is required here, both the public and the secret key. The reason is that, during encryption, the recipient's public key is used to generate the nonce. The throwaway public key generated by the sender is stored in the first `crypto_box_PUBLICKEYBYTE`'s of the c.


[js]: /docs/img/icon_js.svg
[node]: /docs/img/nodejs-icon.svg
