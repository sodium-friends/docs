---
id: keyexchange
title: Key Exchange
sidebar_label: Key Exchange
---

Bindings for the crypto_kx API. [See the libsodium crypto_kx docs for more information](https://download.libsodium.org/doc/key_exchange/).
***
## `crypto_kx_keypair`
![sodium-node][node]
``` js
sodium.crypto_kx_keypair(publicKey, secretKey)
```
Creates a key exchange key pair.
* `publicKey` should be a `buffer` of length `crypto_kx_PUBLICKEYBYTES`
* `secretKey` should be a `buffer` of length `crypto_kx_SECRETKEYBYTES`
***
## `crypto_kx_seed_keypair`
![sodium-node][node]
``` js
sodium.crypto_kx_seed_keypair(publicKey, secretKey, seed)
```
Creates a key exchange key pair based on a `seed`.
* `publicKey` should be a `buffer` of length `crypto_kx_PUBLICKEYBYTES`
* `secretKey` should be a `buffer` of length `crypto_kx_SECRETKEYBYTES`
* `seed` should be a `buffer` of length `crypto_kx_SEEDBYTES`
***
## `crypto_kx_client_session_keys`
![sodium-node][node]
``` js
sodium.crypto_kx_client_session_keys(rx, tx, clientPublicKey, clientSecretKey, serverPublicKey)
```
Generates a session receive and transmission key for a client. The public / secret keys should be generated using the key pair method above.
* `rx` should be a `buffer` of length `crypto_kx_SESSIONKEYBYTES` or `null`
* `tx` should be a `buffer` of length `crypto_kx_SESSIONKEYBYTES` or `null`

You should use the `rx` to decrypt incoming data and `tx` to encrypt outgoing. If you need to make a one-way or half-duplex channel you can give only one of `rx` or `tx`.
***
## `crypto_kx_server_session_keys`
![sodium-node][node]
``` js
sodium.crypto_kx_server_session_keys(rx, tx, serverPublicKey, serverSecretKey, clientPublicKey)
```
Generates a session receive and transmission key for a server. The public / secret keys should be generated using the key pair method above.

* `rx` should be a `buffer` of length `crypto_kx_SESSIONKEYBYTES` or `null`
* `tx` should be a `buffer` of length `crypto_kx_SESSIONKEYBYTES` or `null`

You should use the `rx` to decrypt incoming data and `tx` to encrypt outgoing. If you need to make a one-way or half-duplex channel you can give only one of `rx` or `tx`.


[js]: /docusaurus/img/icon_js.svg
[node]: /docusaurus/img/nodejs-icon.svg
