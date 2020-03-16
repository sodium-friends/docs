---
id: aead
title: AEAD (Authenticated Encryption with Additional Data)
sidebar_label: AEAD (Authenticated Encryption with Additional Data)
---

Bindings for the crypto_aead_* APIs. [See the libsodium AEAD docs for more information](https://download.libsodium.org/doc/secret-key_cryptography/aead).

Currently, only `crypto_aead_xchacha20poly1305_ietf` is exposed.
***
## Constants
**Buffer lengths (integer)**
* `crypto_aead_xchacha20poly1305_ietf_ABYTES`
* `crypto_aead_xchacha20poly1305_ietf_KEYBYTES`
* `crypto_aead_xchacha20poly1305_ietf_NPUBBYTES`
* `crypto_aead_xchacha20poly1305_ietf_NSECBYTES`
* `crypto_aead_xchacha20poly1305_ietf_MESSAGEBYTES_MAX` - Note this is `Number.MAX_SAFE_INTEGER` for now

***
## `crypto_aead_xchacha20poly1305_ietf_keygen`
![sodium-native][node]
``` js
sodium.crypto_aead_xchacha20poly1305_ietf_keygen(k)
```
Generates a new encryption k.
* `k` should be a `buffer` of length `crypto_aead_xchacha20poly1305_ietf_KEYBYTES`.

The generated key is stored in `k`.
***
## `crypto_aead_xchacha20poly1305_ietf_encrypt`
![sodium-native][node]
``` js
var clen = sodium.crypto_aead_xchacha20poly1305_ietf_encrypt(c, m, [ad], null, npub, k)
```
Encrypts a message with (`npub`, `k`) and optional additional data `ad`.
* `c` should be a `buffer` of size `m.length + crypto_aead_xchacha20poly1305_ietf_ABYTES`
* `m` should be a `buffer`
* `ad` is optional and should be `null` or a `buffer`. Included in the computation of authentication tag appended to the m
* `null` is in the position of the unused `nsec` argument. This should always be `null`
* `npub` should be a `buffer` of length `crypto_aead_xchacha20poly1305_ietf_NPUBBYTES`
* `k` should be a `buffer` of length `crypto_aead_xchacha20poly1305_ietf_KEYBYTES`

Returns how many bytes were written to `c`. Note that in-place encryption is possible.
***
## `crypto_aead_xchacha20poly1305_ietf_decrypt`
![sodium-native][node]
``` js
var mlen = sodium.crypto_aead_xchacha20poly1305_ietf_decrypt(m, null, c, [ad], npub, k)
```
Decrypts a message with (`npub`, `k`) and optional additional data `ad`.
* `m` should be a `buffer` of size `c.length - crypto_aead_xchacha20poly1305_ietf_ABYTES`
* `null` is in the position of the unused `nsec` argument. This should always be `null`
* `c` should be a `buffer`
* `ad` is optional and should be `null` or a `buffer`. Included in the computation of authentication tag appended to the m
* `npub` should be a `buffer` of length `crypto_aead_xchacha20poly1305_ietf_NPUBBYTES`
* `k` should be a `buffer` of length `crypto_aead_xchacha20poly1305_ietf_KEYBYTES`

Returns how many bytes were written to `message`. Note that in-place encryption is possible.
***
## `crypto_aead_xchacha20poly1305_ietf_encrypt_detached`
![sodium-native][node]
``` js
var maclen = sodium.crypto_aead_xchacha20poly1305_ietf_encrypt_detached(c, mac, m, [ad], null, npub, k)
```
Encrypts a message with (`npub`, `k`) and optional additional data `ad`.
* `c` should be a `buffer` of size `m.length`
* `mac` should be a `buffer` of size `crypto_aead_xchacha20poly1305_ietf_ABYTES`
* `m` should be a `buffer`
* `ad` is optional and should be `null` or a `buffer`. Included in the computation of authentication tag appended to the m
* `null` is in the position of the unused `nsec` argument. This should always be `null`
* `npub` should be a `buffer` of length `crypto_aead_xchacha20poly1305_ietf_NPUBBYTES`
* `k` should be a `buffer` of length `crypto_aead_xchacha20poly1305_ietf_KEYBYTES`

Returns how many bytes were written to `mac`. Note that in-place encryption is possible.
***
## `crypto_aead_xchacha20poly1305_ietf_decrypt_detached`
![sodium-native][node]
``` js
sodium.crypto_aead_xchacha20poly1305_ietf_decrypt_detached(m, null, c, mac, [ad], npub, k)
```
Decrypts a message with (`npub`, `k`) and optional additional data `ad`.
* `m` should be a `buffer` of size `c.length`
* `null` is in the position of the unused `nsec` argument. This should always be `null`
* `c` should be a `buffer`
* `mac` should be a `buffer` of size `crypto_aead_xchacha20poly1305_ietf_ABYTES`
* `ad` is optional and should be `null` or a `buffer`. Included in the computation of authentication tag appended to the m
* `npub` should be a `buffer` of length `crypto_aead_xchacha20poly1305_ietf_NPUBBYTES`
* `k` should be a `buffer` of length `crypto_aead_xchacha20poly1305_ietf_KEYBYTES`

Returns nothing, but will throw on in case the MAC cannot be authenticated. Note that in-place encryption is possible.


[js]: /docs/img/icon_js.svg
[node]: /docs/img/nodejs-icon.svg
