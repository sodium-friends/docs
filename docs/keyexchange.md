---
id: keyexchange
title: Key Exchange
sidebar_label: Key Exchange
---

Bindings for the crypto_kx API. [See the libsodium crypto_kx docs for more information](https://download.libsodium.org/doc/key_exchange/).

## Constants
**Buffer lengths (integer)**
* `crypto_kx_PUBLICKEYBYTES`
* `crypto_kx_SECRETKEYBYTES`
* `crypto_kx_SEEDBYTES`
* `crypto_kx_SESSIONKEYBYTES`

**String constants (string)**
* `crypto_kx_PRIMITIVE`

***
## `crypto_kx_keypair`
![sodium-native][node]
``` js
sodium.crypto_kx_keypair(pk, sk)
```
Creates a key exchange key pair.
* `pk` should be a `buffer` of length `crypto_kx_PUBLICKEYBYTES`
* `sk` should be a `buffer` of length `crypto_kx_SECRETKEYBYTES`
***
## `crypto_kx_seed_keypair`
![sodium-native][node]
``` js
sodium.crypto_kx_seed_keypair(pk, sk, seed)
```
Creates a key exchange key pair based on a `seed`.
* `pk` should be a `buffer` of length `crypto_kx_PUBLICKEYBYTES`
* `sk` should be a `buffer` of length `crypto_kx_SECRETKEYBYTES`
* `seed` should be a `buffer` of length `crypto_kx_SEEDBYTES`
***
## `crypto_kx_client_session_keys`
![sodium-native][node]
``` js
sodium.crypto_kx_client_session_keys(rx, tx, clientPk, clientSk, serverPk)
```
Generates a session receive and transmission key for a client. The public / secret keys should be generated using the key pair method above.
* `rx` should be a `buffer` of length `crypto_kx_SESSIONKEYBYTES` or `null`
* `tx` should be a `buffer` of length `crypto_kx_SESSIONKEYBYTES` or `null`

You should use the `rx` to decrypt incoming data and `tx` to encrypt outgoing. If you need to make a one-way or half-duplex channel you can give only one of `rx` or `tx`.
***
## `crypto_kx_server_session_keys`
![sodium-native][node]
``` js
sodium.crypto_kx_server_session_keys(rx, tx, serverPk, serverSk, clientPk)
```
Generates a session receive and transmission key for a server. The public / secret keys should be generated using the key pair method above.

* `rx` should be a `buffer` of length `crypto_kx_SESSIONKEYBYTES` or `null`
* `tx` should be a `buffer` of length `crypto_kx_SESSIONKEYBYTES` or `null`

You should use the `rx` to decrypt incoming data and `tx` to encrypt outgoing. If you need to make a one-way or half-duplex channel you can give only one of `rx` or `tx`.


[js]: /docs/img/icon_js.svg
[node]: /docs/img/nodejs-icon.svg
