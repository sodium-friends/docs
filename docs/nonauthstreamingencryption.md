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
* `crypto_stream_xor_STATEBYTES`

**String constants (string)**
* `crypto_stream_PRIMITIVE`

***
## `crypto_stream`
![sodium-native][node] ![sodium-javascript][js]
``` js
sodium.crypto_stream(c, n, k)
```
Generates random data based on a nonce `n` and key `k` into the `c`.
* `c` should be a `buffer` of any size
* `n` should be a `buffer` of length `crypto_stream_NONCEBYTES`
* `k` should be a secret k of length `crypto_stream_KEYBYTES`

The generated data is stored in `c`.
***
## `crypto_stream_xor`
![sodium-native][node] ![sodium-javascript][js]
``` js
sodium.crypto_stream_xor(c, m, n, k)
```
Encrypts, but *not* authenticates, a `m` based on a nonce `n` and key `k`
* `c` should be a `buffer` of length `m.length`
* `m` should be a `buffer` of any size
* `n` should be a `buffer` of length `crypto_stream_NONCEBYTES`
* `k` should be a secret k of length `crypto_stream_KEYBYTES`

The encrypted data is stored in `c`. To decrypt, swap `c` and `m`. Also supports in-place encryption where you use the same `buffer` as `c` and `m`.

Encryption defaults to `XSalsa20`, use `crypto_stream_chacha20_xor` if you want to encrypt/decrypt with `ChaCha20` instead.
***
## `crypto_stream_chacha20_xor`
![sodium-native][node]
``` js
sodium.crypto_stream_chacha20_xor(c, m, n, k)
```
Encrypts, but *not* authenticates, a `m` based on a nonce `n` and key `k`
* `c` should be a `buffer` of length `m.length`
* `m` should be a `buffer` of any size
* `n` should be a `buffer` of length `crypto_stream_chacha20_NONCEBYTES`
* `k` should be a secret k of length `crypto_stream_chacha20_KEYBYTES`

The encrypted data is stored in `c`. To decrypt, swap `c` and `m`. Also supports in-place encryption where you use the same `buffer` as `c` and `m`.

Encryption defaults to `XSalsa20`, use `crypto_stream_chacha20_xor` if you want to encrypt/decrypt with `ChaCha20` instead.
***
## Instance API
### `crypto_stream_xor_instance`
![sodium-native][node] ![sodium-javascript][js]
``` js
var instance = sodium.crypto_stream_xor_instance(n, k)
```
A streaming instance to the `crypto_stream_xor` API. Pass a nonce `n` and key `k` in the constructor.

Encryption defaults to `XSalsa20`, use `crypto_stream_chacha20_xor_instance` if you want to encrypt/decrypt with `ChaCha20` instead.

### `crypto_stream_chacha20_xor_instance`
![sodium-native][node]
``` js
var instance = sodium.crypto_stream_chacha20_xor_instance(n, k)
```
A streaming instance to the `crypto_stream_xor` API. Pass a nonce `n` and key `k` in the constructor.

Encryption defaults to `XSalsa20`, use `crypto_stream_chacha20_xor_instance` if you want to encrypt/decrypt with `ChaCha20` instead.

### `instance.update`
``` js
instance.update(c, m)
```
Encrypts the next m.

### `instance.final`
``` js
instance.final()
```
Finalizes the stream. Zeros out internal state.

## Stateful API
Replaces the above instance implementation in the N-API release
### `crypto_stream_xor_init`
![sodium-native][node]
```js
var state = Buffer.alloc(crypto_stream_xor_STATEBYTES)

sodium.crypto_stream_xor_init(state, n, k)
```
Initialise a new xor state with a nonce `n` and key `k`.
* `state` must be a buffer of length `crypto_stream_xor_STATEBYTES` bytes
* `n` is must be buffer of `crypto_stream_NONCEBYTES` bytes
* `k` is must be buffer of `crypto_stream_KEYBYTES` bytes
### `crypto_stream_xor_update`
![sodium-native][node]
```js
sodium.crypto_stream_xor_update(state, c, m)
```
Encrypt a message `m` and xor the result into ciphertext `c`.
Update a hash state with a given input.
* `state` must be a buffer of length `crypto_stream_xor_STATEBYTES` bytes
* `m` should be a `buffer`
* `c` should be a `buffer` of length `m.byteLength` bytes

### `crypto_stream_xor_final(state, out)`
![sodium-native][node]
```js
sodium.crypto_stream_xor_final(state)
```
Finalize a given xor state, zeroing out the state.
* `state` must be a buffer of length `crypto_stream_xor_STATEBYTES` bytes

### `crypto_stream_chacha20_xor_init`
![sodium-native][node]
```js
var state = Buffer.alloc(crypto_stream_chacha20_xor_STATEBYTES)

sodium.crypto_stream_chacha20_xor_init(state, n, k)
```
Initialise a new xor state with a nonce `n` and key `k`.
* `state` must be a buffer of length `crypto_stream_chacha20_xor_STATEBYTES` bytes
* `n` is must be buffer of `crypto_stream_chacha20_NONCEBYTES` bytes
* `k` is must be buffer of `crypto_stream_chacha20_KEYBYTES` bytes
### `crypto_stream_chacha20_xor_update`
![sodium-native][node]
```js
sodium.crypto_stream_chacha20_xor_update(state, c, m)
```
Encrypt a message `m` and xor the result into ciphertext `c`.
Update a hash state with a given input.
* `state` must be a buffer of length `crypto_stream_chacha20_xor_STATEBYTES` bytes
* `m` should be a `buffer`
* `c` should be a `buffer` of length `m.byteLength` bytes

### `crypto_stream_chacha20_xor_final(state, out)`
![sodium-native][node]
```js
sodium.crypto_stream_chacha20_xor_final(state)
```
Finalize a given xor state, zeroing out the state.
* `state` must be a buffer of length `crypto_stream_chacha20_xor_STATEBYTES` bytes

[js]: /docs/img/icon_js.svg
[node]: /docs/img/nodejs-icon.svg
