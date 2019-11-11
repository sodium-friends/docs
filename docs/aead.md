---
id: aead
title: AEAD (Authenticated Encryption with Additional Data)
sidebar_label: AEAD (Authenticated Encryption with Additional Data)
---

Bindings for the crypto_aead_* APIs. [See the libsodium AEAD docs for more information](https://download.libsodium.org/doc/secret-key_cryptography/aead).

Currently only `crypto_aead_xchacha20poly1305_ietf` is exposed.

### Constants
**Buffer lengths (Integer)**
* `crypto_aead_xchacha20poly1305_ietf_ABYTES`
* `crypto_aead_xchacha20poly1305_ietf_KEYBYTES`
* `crypto_aead_xchacha20poly1305_ietf_NPUBBYTES`
* `crypto_aead_xchacha20poly1305_ietf_NSECBYTES`
* `crypto_aead_xchacha20poly1305_ietf_MESSAGEBYTES_MAX` - Note this is `Number.MAX_SAFE_INTEGER` for now

***
## `crypto_aead_xchacha20poly1305_ietf_keygen`
![sodium-node][node]
``` js
sodium.crypto_aead_xchacha20poly1305_ietf_keygen(key)
```
Generate a new encryption key.
* `key` should be a `buffer` of length `crypto_aead_xchacha20poly1305_ietf_KEYBYTES`.

The generated key is stored in `key`.
***
## `crypto_aead_xchacha20poly1305_ietf_encrypt`
![sodium-node][node]
``` js
var clen = sodium.crypto_aead_xchacha20poly1305_ietf_encrypt(ciphertext, message, [ad], null, npub, key)
```
Encrypt a message with (`npub`, `key`) and optional additional data `ad`.
* `ciphertext` should be a `buffer` of size `message.length + crypto_aead_xchacha20poly1305_ietf_ABYTES`
* `message` should be a `buffer`
* `ad` is optional and should be `null` or `buffer`. Included in the computation of authentication tag appended to the message
* `null` is in the position of the unused `nsec` argument. This should always be `null`
* `npub` should be a `buffer` of length `crypto_aead_xchacha20poly1305_ietf_NPUBBYTES`
* `key` should be a `buffer` of length `crypto_aead_xchacha20poly1305_ietf_KEYBYTES`

Returns how many bytes were written to `ciphertext`. Note that in-place encryption is possible.
***
## `crypto_aead_xchacha20poly1305_ietf_decrypt`
![sodium-node][node]
``` js
var mlen = sodium.crypto_aead_xchacha20poly1305_ietf_decrypt(message, null, ciphertext, [ad], npub, key)
```
Decrypt a message with (`npub`, `key`) and optional additional data `ad`.
* `message` should be a `buffer` of size `ciphertext.length - crypto_aead_xchacha20poly1305_ietf_ABYTES`
* `null` is in the position of the unused `nsec` argument. This should always be `null`
* `ciphertext` should be a `buffer`
* `ad` is optional and should be `null` or `buffer`. Included in the computation of authentication tag appended to the message
* `npub` should be a `buffer` of length `crypto_aead_xchacha20poly1305_ietf_NPUBBYTES`
* `key` should be a `buffer` of length `crypto_aead_xchacha20poly1305_ietf_KEYBYTES`

Returns how many bytes were written to `message`. Note that in-place encryption is possible.
***
## `crypto_aead_xchacha20poly1305_ietf_encrypt_detached`
![sodium-node][node]
``` js
var maclen = sodium.crypto_aead_xchacha20poly1305_ietf_encrypt_detached(ciphertext, mac, message, [ad], null, npub, key)
```
Encrypt a message with (`npub`, `key`) and optional additional data `ad`.
* `ciphertext` should be a `buffer` of size `message.length`
* `mac` should be a `buffer` of size `crypto_aead_xchacha20poly1305_ietf_ABYTES`
* `message` should be a `buffer`
* `ad` is optional and should be `null` or `buffer`. Included in the computation of authentication tag appended to the message
* `null` is in the position of the unused `nsec` argument. This should always be `null`
* `npub` should be a `buffer` of length `crypto_aead_xchacha20poly1305_ietf_NPUBBYTES`
* `key` should be a `buffer` of length `crypto_aead_xchacha20poly1305_ietf_KEYBYTES`

Returns how many bytes were written to `mac`. Note that in-place encryption is possible.
***
## `crypto_aead_xchacha20poly1305_ietf_decrypt_detached`
![sodium-node][node]
``` js
sodium.crypto_aead_xchacha20poly1305_ietf_decrypt_detached(message, null, ciphertext, mac, [ad], npub, key)
```
Decrypt a message with (`npub`, `key`) and optional additional data `ad`.
* `message` should be a `buffer` of size `ciphertext.length`
* `null` is in the position of the unused `nsec` argument. This should always be `null`
* `ciphertext` should be a `buffer`
* `mac` should be a `buffer` of size `crypto_aead_xchacha20poly1305_ietf_ABYTES`
* `ad` is optional and should be `null` or `buffer`. Included in the computation of authentication tag appended to the message
* `npub` should be a `buffer` of length `crypto_aead_xchacha20poly1305_ietf_NPUBBYTES`
* `key` should be a `buffer` of length `crypto_aead_xchacha20poly1305_ietf_KEYBYTES`

Returns nothing, but will throw on in case the MAC cannot be authenticated. Note that in-place encryption is possible.


[js]: /docusaurus/img/icon_js.svg
[node]: /docusaurus/img/nodejs-icon.svg
