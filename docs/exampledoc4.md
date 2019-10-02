---
id: doc4
title: API
sidebar_label: API
---

[Go to docs (Github) for the latest release](https://github.com/sodium-friends/sodium-native/tree/v2.3.0) (The following docs may be for an unreleased version)

``` js
var sodium = require('sodium-native')
```
Loads the bindings. If you get a module version error, you probably need to reinstall the module because you switched node versions.

## Memory Protection
Bindings to the secure memory API. [See the libsodium "Securing memory allocations" docs for more information](https://download.libsodium.org/doc/memory_management).

``` js
sodium.sodium_memzero(buffer)
```
Zero out the data in `buffer`.

``` js
sodium.sodium_mlock(buffer)
```
Lock the memory contained in `buffer`.

``` js
sodium.sodium_munlock(buffer)
```
Unlock previously `sodium_mlock`'ed memory contained in `buffer`. This will also `sodium_memzero` `buffer`.

``` js
var buffer = sodium.sodium_malloc(size)
```
Allocate a buffer of `size` which is memory protected. See [libsodium docs](https://download.libsodium.org/doc/memory_management#guarded-heap-allocations) for details. Be aware that many `buffer`-methods may break the security guarantees of `sodium.sodium_malloc`'ed memory. To check if a `buffer` is a "secure" buffer, you can call access the getter `buffer.secure` which will be `true`.

``` js
sodium.sodium_mprotect_noaccess(buffer)
```
Make `buffer` allocated using `sodium.sodium_malloc` inaccessible, crashing the process if any access is attempted. Note that this will have no effect for normal `buffer`'s.

``` js
sodium.sodium_mprotect_readonly(buffer)
```
Make `buffer` allocated using `sodium.sodium_malloc` read-only, crashing the process if any writing is attempted. Note that this will have no effect for normal `buffer`'s.

``` js
sodium.sodium_mprotect_readwrite(buffer)
```
Make `buffer` allocated using `sodium.sodium_malloc` read-write, undoing `sodium_mprotect_noaccess` or `sodium_mprotect_readonly`. Note that this will have no effect for normal `buffer`'s.

## Generating random data
Bindings to the random data generation API. [See the libsodium randombytes docs for more information](https://download.libsodium.org/doc/generating_random_data/).

``` js
var uint32 = sodium.randombytes_random()
```
Generate a random 32-bit unsigned integer `[0, 0xffffffff]` (both inclusive).

``` js
var uint = sodium.randombytes_uniform(upper_bound)
```
Generate a random 32-bit unsigned integer `[0, upper_bound)` (last exclusive). `upper_bound` must be `0xffffffff` at most.

``` js
sodium.randombytes_buf(buffer)
```
Fill `buffer` with random data.

``` js
sodium.randombytes_buf_deterministic(buffer, seed)
```
Fill `buffer` with random data, generated from `seed`. `seed` must be a buffer of at least `sodium.randombytes_SEEDBYTES` length.

## Helpers
Bindings to various helper functions. [See the libsodium helpers docs for more information](https://download.libsodium.org/doc/helpers/).

``` js
var bool = sodium.sodium_memcmp(b1, b2)
```
Compare `b1` with `b2`, in **constant-time** for `b1.length`.
* `b1` must be `buffer`
* `b2` must be `buffer` and must be `b1.length` bytes

Returns `true` when equal, otherwise `false`.

``` js
var direction = sodium.sodium_compare(b1, b2)
```
Compare `b1` with `b2`, regarding either as little-endian encoded number.
* `b1` must be `buffer`
* `b2` must be `buffer` and must be `b1.length` bytes

Returns `1`, `0`, or `-1` on whether `b1`is greater, equal, or less than `b2`. This is the same scheme as `Array.prototype.sort` expect.

``` js
sodium.sodium_add(a, b)
```
Adds `b` to `a` (wrapping), regarding either as little-endian encoded number, and writing the result into `a`.
* `a` must be `buffer`
* `b` must be `buffer` and must be `a.length` bytes

``` js
sodium.sodium_sub(a, b)
```
Subtracts `b` from `a` (wrapping), regarding either as little-endian encoded number, and writing the result into `a`.
* `a` must be `buffer`
* `b` must be `buffer` and must be `a.length` bytes

``` js
sodium.sodium_increment(buf)
```
Increments `buf` as a little-endian number. This operation is **constant-time** for the length of `buf`.
* `buf` must be `buffer`

``` js
var bool = sodium.sodium_is_zero(buf, len)
```
Tests whether `buf` is all zero for `len` bytes. This operation is **constant-time** for `len`.
* `len` must be integer at most the length of `buf`

Returns `true` if all `len` bytes are zero, otherwise `false`.

## Padding
Bindings to the padding API. [See the libsodium padding docs for more information](https://download.libsodium.org/doc/padding).

``` js
var paddedLength = sodium.sodium_pad(buf, unpaddedLength, blocksize)
```
Pad `buf` with random data from index `unpaddedLength` up to closest multiple of `blocksize`.
* `buf` must be `buffer`
* `unpaddedLength` must be integer at most `buf.length`
* `blocksize` must be integer greater than 1 but at most `buf.length`

Returns the length of the padded data (so you may `.slice` the buffer to here).

``` js
var unpaddedLength = sodium.sodium_unpad(buf, paddedLength, blocksize)
```
Calculate `unpaddedLength` from a padded `buf` with `blocksize`.
* `buf` must be `buffer`
* `paddedLength` must be integer at most `buf.length`
* `blocksize` must be integer greater than 1 but at most `buf.length`

Returns the length of the unpadded data (so you may `.slice` the buffer to here).

## Signing
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

## Generic hashing
Bindings for the crypto_generichash API. [See the libsodium crypto_generichash docs for more information](https://download.libsodium.org/doc/hashing/generic_hashing).

``` js
crypto_generichash(output, input, [key])
```
Hash a value with an optional key using the generichash method.
* `output` should be a buffer with length within `crypto_generichash_BYTES_MIN` - `crypto_generichash_BYTES_MAX`
* `input` should be a buffer of any length
* `key` is an optional buffer of length within `crypto_generichash_KEYBYTES_MIN` - `crypto_generichash_KEYBYTES_MAX`

The generated hash is stoed in `output`.

Also exposes `crypto_generichash_BYTES` and `crypto_generichash_KEYBYTES` that can be used as "default" buffer sizes.

``` js
crypto_generichash_batch(output, inputArray, [key])
```
Same as `crypto_generichash` except this hashes an array of buffers instead of a single one.

``` js
var instance = crypto_generichash_instance([key], [outputLength])
```
Create a generichash instance that can hash a stream of input `buffer`'s.
* `key` is an optional buffer as above
* `outputLength` the buffer size of your output

``` js
instance.update(input)
```
Update the instance with a new piece of data.
* `input` should be a buffer of any size

``` js
instance.final(output)
```
Finalize the instance.
* `output` should be a buffer as above with the same length you gave when creating the instance

The generated hash is stoed in `output`.

## Public/secret key box encryption
Bindings for the crypto_box API. [See the libsodium crypto_box docs for more information](https://download.libsodium.org/doc/public-key_cryptography/authenticated_encryption).

``` js
crypto_box_seed_keypair(publicKey, secretKey, seed)
```
Create a new keypair based on a seed.
* `publicKey` should be a buffer with length `crypto_box_PUBLICKEYBYTES`
* `secretKey` should be a buffer with length `crypto_box_SECRETKEYBYTES`
* `seed` should be a buffer with length `crypto_box_SEEDBYTES`

The generated public and secret key will be stored in passed in buffers.

``` js
crypto_box_keypair(publicKey, secretKey)
```
Create a new keypair.
* `publicKey` should be a buffer with length `crypto_box_PUBLICKEYBYTES`
* `secretKey` should be a buffer with length `crypto_box_SECRETKEYBYTES`

The generated public and secret key will be stored in passed in buffers.

``` js
crypto_box_detached(ciphertext, mac, message, nonce, publicKey, secretKey)
```
Encrypt a message.
* `ciphertext` should be a buffer with length `message.length`
* `mac` should be a buffer with length `crypto_box_MACBYTES`
* `message` should be a buffer of any length
* `nonce` should be a buffer with length `crypto_box_NONCEBYTES`
* `publicKey` should be a public key
* `secretKey` should be a secret key

The encrypted message will be stored in `ciphertext` and the authentification code will be stored in `mac`.

``` js
crypto_box_easy(ciphertext, message, nonce, publicKey, secretKey)
```
Same as `crypto_box_detached` except it encodes the `mac` in the message.
* `ciphertext` should be a buffer with length `message.length + crypto_box_MACBYTES`
* `message` should be a buffer of any length
* `nonce` should be a buffer with length `crypto_box_NONCEBYTES`
* `publicKey` should be a public key
* `secretKey` should be a secret key

The encrypted message and authentification code will be stored in `ciphertext`.

``` js
var bool = crypto_box_open_detached(message, ciphertext, mac, nonce, publicKey, secretKey)
```
Decrypt a message.
* `message` should be a buffer with length `ciphertext.length`
* `mac` should be a buffer with length `crypto_box_MACBYTES`
* `ciphertext` should be a buffer of any length
* `nonce` should be a buffer with length `crypto_box_NONCEBYTES`
* `publicKey` should be a public key
* `secretKey` should be a secret key

Returns `true` if the message could be decrypted. Otherwise `false`.

The decrypted message will be stored in `message`.

``` js
var bool = crypto_box_open_easy(message, ciphertext, nonce, publicKey, secretKey)
```
Decrypt a message encoded with the easy method.
* `message` should be a buffer with length `ciphertext.length - crypto_box_MACBYTES`
* `ciphertext` should be a buffer with length at least `crypto_box_MACBYTES`
* `nonce` should be a buffer with length `crypto_box_NONCEBYTES`
* `publicKey` should be a public key
* `secretKey` should be a secret key

Returns `true` if the message could be decrypted. Otherwise `false`.

The decrypted message will be stored in `message`.

## Sealed box encryption
Bindings for the crypto_box_seal API. [See the libsodium crypto_box_seal docs for more information](https://download.libsodium.org/doc/public-key_cryptography/sealed_boxes).

Keypairs can be generated with `crypto_box_keypair()` or `crypto_box_seed_keypair()`.

``` js
crypto_box_seal(ciphertext, message, publicKey)
```
Encrypt a message in a sealed box using a throwaway keypair. The ciphertext cannot be associated with the sender due to the sender's key being a single use keypair that is overwritten during encryption.
* `ciphertext` should be a buffer with length at least `message.length + crypto_box_SEALBYTES`
* `message` should be a buffer with any length
* `publicKey` should be the receipent's public key.

``` js
var bool = crypto_box_seal_open(message, ciphertext, publicKey, secretKey)
```
Decrypt a message encoded with the sealed box method.
* `message` should be a buffer with length at least `ciphertext.length - crypto_box_SEALBYTES`
* `ciphertext` should be a buffer with length at least `crypto_box_SEALBYTES`
* `publicKey` should be the receipient's public key
* `secretKey` should be the receipient's secret key

Note: the keypair of the recipient is required here, both public and secret key. This is because during encryption the recipient's public key is used to generate the nonce. The throwaway public key generated by the sender is stored in the first `crypto_box_PUBLICKEYBYTE`'s of the ciphertext.

## Secret key box encryption
Bindings for the crypto_secretbox API. [See the libsodium crypto_secretbox docs for more information](https://download.libsodium.org/doc/public-key_cryptography/authenticated_encryption).

``` js
crypto_secretbox_detached(ciphertext, mac, message, nonce, secretKey)
```
Encrypt a message.
* `ciphertext` should be a buffer with length `message.length`
* `mac` should be a buffer with length `crypto_secretbox_MACBYTES`
* `message` should be a buffer of any length
* `nonce` should be a buffer with length `crypto_secretbox_NONCEBYTES`
* `secretKey` should be a secret key with length `crypto_secretbox_KEYBYTES`

The encrypted message will be stored in `ciphertext` and the authentification code will be stored in `mac`.

``` js
crypto_secretbox_easy(ciphertext, message, nonce, secretKey)
```
Same as `crypto_secretbox_detached` except it encodes the mac in the message.
* `ciphertext` should be a buffer with length `message.length + crypto_secretbox_MACBYTES`
* `message` should be a buffer of any length
* `nonce` should be a buffer with length `crypto_secretbox_NONCEBYTES`
* `secretKey` should be a secret key with length `crypto_secretbox_KEYBYTES`

``` js
var bool = crypto_secretbox_open_detached(message, ciphertext, mac, nonce, secretKey)
```
Decrypt a message.
* `message` should be a buffer with length `ciphertext.length`
* `mac` should be a buffer with length `crypto_secretbox_MACBYTES`
* `ciphertext` should be a buffer of any length
* `nonce` should be a buffer with length `crypto_secretbox_NONCEBYTES`
* `secretKey` should be a secret key

Returns `true` if the message could be decrypted. Otherwise `false`.

The decrypted message will be stored in `message`.

``` js
var bool = crypto_secretbox_open_easy(message, ciphertext, nonce, secretKey)
```
Decrypt a message encoded with the easy method.
* `message` should be a buffer with length `ciphertext.length - crypto_secretbox_MACBYTES`
* `ciphertext` should be a buffer with length at least `crypto_secretbox_MACBYTES`
* `nonce` should be a buffer with length `crypto_secretbox_NONCEBYTES`
* `secretKey` should be a secret key

Returns `true` if the message could be decrypted. Otherwise `false`.

The decrypted message will be stored in `message`.

## AEAD (Authenticated Encryption with Additional Data)
Bindings for the crypto_aead_* APIs. [See the libsodium AEAD docs for more information](https://download.libsodium.org/doc/secret-key_cryptography/aead).

Currently only `crypto_aead_xchacha20poly1305_ietf` is exposed.

### Constants
**Buffer lengths (Integer)**
* `crypto_aead_xchacha20poly1305_ietf_ABYTES`
* `crypto_aead_xchacha20poly1305_ietf_KEYBYTES`
* `crypto_aead_xchacha20poly1305_ietf_NPUBBYTES`
* `crypto_aead_xchacha20poly1305_ietf_NSECBYTES`
* `crypto_aead_xchacha20poly1305_ietf_MESSAGEBYTES_MAX` - Note this is `Number.MAX_SAFE_INTEGER` for now

``` js
crypto_aead_xchacha20poly1305_ietf_keygen(key)
```
Generate a new encryption key.
* `key` should be a buffer of length `crypto_aead_xchacha20poly1305_ietf_KEYBYTES`.

The generated key is stored in `key`.

``` js
var clen = crypto_aead_xchacha20poly1305_ietf_encrypt(ciphertext, message, [ad], null, npub, key)
```
Encrypt a message with (`npub`, `key`) and optional additional data `ad`.
* `ciphertext` should be a `buffer` of size `message.length + crypto_aead_xchacha20poly1305_ietf_ABYTES`
* `message` should be a `buffer`
* `ad` is optional and should be null or `buffer`. Included in the computation of authentication tag appended to the message
* `null` is in the position of the unused `nsec` argument. This should always be `null`
* `npub` should be `buffer` of length `crypto_aead_xchacha20poly1305_ietf_NPUBBYTES`
* `key` should be a `buffer` of length `crypto_aead_xchacha20poly1305_ietf_KEYBYTES`

Returns how many bytes were written to `ciphertext`. Note that in-place encryption is possible.

``` js
var mlen = crypto_aead_xchacha20poly1305_ietf_decrypt(message, null, ciphertext, [ad], npub, key)
```
Decrypt a message with (`npub`, `key`) and optional additional data `ad`.
* `message` should be a `buffer` of size `ciphertext.length - crypto_aead_xchacha20poly1305_ietf_ABYTES`
* `null` is in the position of the unused `nsec` argument. This should always be `null`
* `ciphertext` should be a `buffer`
* `ad` is optional and should be null or `buffer`. Included in the computation of authentication tag appended to the message
* `npub` should be a `buffer` of length `crypto_aead_xchacha20poly1305_ietf_NPUBBYTES`
* `key` should be a `buffer` of length `crypto_aead_xchacha20poly1305_ietf_KEYBYTES`

Returns how many bytes were written to `message`. Note that in-place encryption is possible.

``` js
var maclen = crypto_aead_xchacha20poly1305_ietf_encrypt_detached(ciphertext, mac, message, [ad], null, npub, key)
```
Encrypt a message with (`npub`, `key`) and optional additional data `ad`.
* `ciphertext` should be a `buffer` of size `message.length`
* `mac` should be a `buffer` of size `crypto_aead_xchacha20poly1305_ietf_ABYTES`
* `message` should be a `buffer`
* `ad` is optional and should be `null` or `buffer`. Included in the computation of authentication tag appended to the message
* `null` is in the position of the unused `nsec` argument. This should always be null
* `npub` should be a `buffer` of length `crypto_aead_xchacha20poly1305_ietf_NPUBBYTES`
* `key` should be a `buffer` of length `crypto_aead_xchacha20poly1305_ietf_KEYBYTES`

Returns how many bytes were written to `mac`. Note that in-place encryption is possible.

``` js
crypto_aead_xchacha20poly1305_ietf_decrypt_detached(message, null, ciphertext, mac, [ad], npub, key)
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

## Non-authenticated streaming encryption
Bindings for the crypto_stream API. [See the libsodium crypto_stream docs for more information](https://download.libsodium.org/doc/advanced/stream_ciphers/xsalsa20).

``` js
crypto_stream(ciphertext, nonce, key)
```
Generate random data based on a nonce and key into the ciphertext.
* `ciphertext` should be a `buffer` of any size
* `nonce` should be a `buffer` with length `crypto_stream_NONCEBYTES`
* `key` should be a secret key with length `crypto_stream_KEYBYTES`

The generated data is stored in `ciphertext`.

``` js
crypto_stream_xor(ciphertext, message, nonce, key)
```
or
``` js
crypto_stream_chacha20_xor(ciphertext, message, nonce, key)
```
Encrypt, but not authenticate, a message based on a nonce and key
* `ciphertext` should be a `buffer` with length `message.length`
* `message` should be a `buffer` of any size
* `nonce` should be a `buffer` with length `crypto_stream_NONCEBYTES`
* `key` should be a secret key with length `crypto_stream_KEYBYTES`

The encrypted data is stored in `ciphertext`. To decrypt, swap `ciphertext` and `message`. Also supports in-place encryption where you use the same buffer as `ciphertext` and `message`.

Encryption defaults to XSalsa20, use `crypto_stream_chacha20_xor` if you want to encrypt/decrypt with ChaCha20 instead.

``` js
var instance = crypto_stream_xor_instance(nonce, key)
```
or
``` js
var istance = crypto_stream_chacha20_xor_instance(nonce, key)
```
A streaming instance to the `crypto_stream_xor` API. Pass a nonce and key in the constructor.

Encryption defaults to XSalsa20, use `crypto_stream_chacha20_xor_instance` if you want to encrypt/decrypt with ChaCha20 instead.

``` js
instance.update(ciphertext, message)
```
Encrypt the next message.

``` js
instance.final()
```
Finalize the stream. Zeros out internal state.

## Authentication
Bindings for the crypto_auth API. [See the libsodium crypto_auth docs for more information](https://download.libsodium.org/doc/secret-key_cryptography/secret-key_authentication).

``` js
crypto_auth(output, input, key)
```
Create an authentication token.
* `output` should be a `buffer` of length `crypto_auth_BYTES`
* `input` should be a `buffer` of any size
* `key` should be a `buffer` of lenght `crypto_auth_KEYBYTES`

The generated token is stored in `output`.

``` js
var bool = crypto_auth_verify(output, input, key)
```
Verify a token.
* `output` should be a `buffer` of length `crypto_auth_BYTES`
* `input` should be a `buffer` of any size
* `key` should be a `buffer` of lenght `crypto_auth_KEYBYTES`

Returns `true` if the token could be verified. Otherwise `false`.

## Stream encryption
Bindings for the crypto_secretstream API. [See the libsodium crypto_secretstream docs for more information](https://download.libsodium.org/doc/secret-key_cryptography/secretstream).

### Constants
**Buffer lengths (Integer)**
* `crypto_secretstream_xchacha20poly1305_ABYTES`
* `crypto_secretstream_xchacha20poly1305_HEADERBYTES`
* `crypto_secretstream_xchacha20poly1305_KEYBYTES`
* `crypto_secretstream_xchacha20poly1305_MESSAGEBYTES_MAX`
* `crypto_secretstream_xchacha20poly1305_TAGBYTES` - NOTE: Unofficial constant

**Message tags (Buffer)**
* `crypto_secretstream_xchacha20poly1305_TAG_MESSAGE`
* `crypto_secretstream_xchacha20poly1305_TAG_PUSH`
* `crypto_secretstream_xchacha20poly1305_TAG_REKEY`
* `crypto_secretstream_xchacha20poly1305_TAG_FINAL`

``` js
crypto_secretstream_xchacha20poly1305_keygen(key)
```
Generate a new encryption key.
* `key` should be a `buffer` of length `crypto_secretstream_xchacha20poly1305_KEYBYTES`

The generated key is stored in `key`.

``` js
var state = crypto_secretstream_xchacha20poly1305_state_new()
```
Create a new stream state. Returns an opaque object used in the next methods.

``` js
crypto_secretstream_xchacha20poly1305_init_push(state, header, key)
```
Initialise `state` from the writer side with message `header` and encryption key `key`. The `header` must be sent or stored with the stream. The `key` must be exchanged securely with the receiving / reading side.
* `state` should be an opaque state object
* `header` should be a `buffer` of size `crypto_secretstream_xchacha20poly1305_HEADERBYTES`
* `key` should be a `buffer` of length `crypto_secretstream_xchacha20poly1305_KEYBYTES`

``` js
var mlen = crypto_secretstream_xchacha20poly1305_push(state, ciphertext, message, [ad], tag)
```
Encrypt a message with a certain tag and optional additional data `ad`.
* `state` should be an opaque state object
* `ciphertext` should be a `buffer` of size `message.length + crypto_secretstream_xchacha20poly1305_ABYTES`
* `message` should be a `buffer`
* `ad` is optional and should be `null` or `buffer`. Included in the computation of authentication tag appended to the message
* `tag` should be a `buffer` of length `crypto_secretstream_xchacha20poly1305_TAGBYTES`

Note that `tag` should be one of the `crypto_secretstream_xchacha20poly1305_TAG_*` constants. Returns number of encrypted bytes written to `ciphertext`.

``` js
crypto_secretstream_xchacha20poly1305_init_pull(state, header, key)
```
Initialise `state` from the reader side with message `header` and encryption key `key`. The `header` must be retrieved from somewhere. The `key` must be exchanged securely with the sending / writing side.
* `state` should be an opaque state object
* `header` should be a `buffer` of size `crypto_secretstream_xchacha20poly1305_HEADERBYTES`
* `key` should be a `buffer` of length `crypto_secretstream_xchacha20poly1305_KEYBYTES`

``` js
var clen = crypto_secretstream_xchacha20poly1305_pull(state, message, tag, ciphertext, [ad])
```
Decrypt a message with optional additional data `ad`, and write message tag to `tag`. Make sure to check this!
* `state` should be an opaque state object
* `message` should be a `buffer` of size `ciphertext.length - crypto_secretstream_xchacha20poly1305_ABYTES`
* `tag` should be a `buffer` of `crypto_secretstream_xchacha20poly1305_TAGBYTES`
* `ad` is optional and should be `null` or `buffer`. Included in the computation of the authentication tag appended to the message

Note that `tag` should be one of the `crypto_secretstream_xchacha20poly1305_TAG_*` constants. Returns number of decrypted bytes written to `message`.

``` js
crypto_secretstream_xchacha20poly1305_rekey(state)
```
Rekey the opaque `state` object.

## One-time Authentication
Bindings for the crypto_onetimeaith API. [See the libsodium crypto_onetimeauth docs for more information](https://download.libsodium.org/doc/advanced/poly1305).

``` js
crypto_onetimeauth(output, input, key)
```
Create a authentication token based on a onetime key.
* `output` should be a `buffer` of length `crypto_onetimauth_BYTES`
* `input` should be a `buffer` of any size
* `key` should be a `buffer` of length `crypto_onetimeauth_KEYBYTES`

The generated token is stored in `output`.

``` js
var bool = crypto_onetimeauth_verify(output, input, key)
```
Verify a token.
* `output` should be a `buffer` of length `crypto_onetimeauth_BYTES`
* `input` should be a `buffer` of any size
* `key` should be a `buffer` of length `crypto_onetimeauth_KEYBYTES`

Returns `true` if the token could be verified. Otherwise `false`.

``` js
var instance = crypto_onetimeauth_instance(key)
```
Create an instance that creates a token from a onetime key and a stream of input data.
* `key` should be a `buffer` of length `crypto_onetimeauth_KEYBYTES`

``` js
instance.update(input)
```
Update the instance with a new piece of data.
* `input` should be a `buffer` of any size

``` js
instance.final(output)
```
Finalize the instance.
* `output` should be a `buffer` of length `crypto_onetimeauth_BYTES`

The generated hash is stored in `output`.

## Password Hashing
Bindings for the crypto_pwhash API. [See the libsodium crypto_pwhash docs for more information](https://download.libsodium.org/doc/password_hashing/).

``` js
crypto_pwhash(output, password, salt, opslimit, memlimit, algorithm)
```
Create a password hash.
* `output` should be a `buffer` with length within `crypto_pwhash_BYTES_MIN` - `crypto_pwhash_BYTES_MAX`
* `password` should be a `buffer` of any size
* `salt` should be a `buffer` with length `crypto_pwhash_SALTBYTES`
* `opslimit` should be a number containing your ops limit setting in the range `crypto_pwhash_OPSLIMIT_MIN` - `crypto_pwhash_OPSLIMIT_MAX`
* `memlimit` should be a number containing your mem limit setting in the range `crypto_pwhash_MEMLIMIT_MIN` - `crypto_pwhash_OPSLIMIT_MAX`
`algorithm` should be a number specifying the algorithm you want to use

Available default ops and mem limits are
* `crypto_pwhash_OPSLIMIT_INTERACTIVE`
* `crypto_pwhash_OPSLIMIT_MODERATE`
* `crypto_pwhash_OPSLIMIT_SENSITIVE`
* `crypto_pwhash_MEMLIMIT_INTERACTIVE`
* `crypto_pwhash_MEMLIMIT_MODERATE`
* `crypto_pwhash_MEMLIMIT_SENSITIVE`

The available algorithms are
* `crypto_pwhash_ALG_DEFAULT`
* `crypto_pwhash_ALG_ARGON2ID13`
* `crypto_pwhash_ALG_ARGON2I13`

The generated hash will be stored in `output` and the entire `output buffer` will be used.

``` js
crypto_pwhash_str(output, password, opslimit, memlimit)
```
Create a password hash with a random salt.
* `output` should be a `buffer` with length `crypto_pwhash_STRBYTES`
* `password` should be a `buffer` of any size
* `opslimit` should be a number containing your ops limit setting in the range `crypto_pwhash_OPSLIMIT_MIN` - `crypto_pwhash_OPSLIMIT_MAX`
* `memlimit` should be a number containing your mem limit setting in the range `crypto_pwhash_MEMLIMIT_MIN` - `crypto_pwhash_OPSLIMIT_MAX`

The generated hash, settings, salt, version and algorithm will be stored in `output` and the entire `output buffer` will be used.

``` js
var bool = crypto_pwhash_str_verify(str, password)
```
Verify a password hash generated with the above method.
* `str` should be a `buffer` with length `crypto_pwhash_STRBYTES`
* `password` should be a `buffer` of any size

Returns `true` if the hash could be verified with the settings contained in `str`. Otherwise `false`.

``` js
var bool = crypto_pwhash_str_needs_rehash(hash, opslimit, memlimit)
```
Check if a password hash needs rehash, either because the default algorithm changed, `opslimit` or `memlimit` increased or because the hash is malformed.
* `hash` should be a `buffer` with length `crypto_pwhash_STRBYTES`
* `opslimit` should be a number containing your ops limit setting in the range `crypto_pwhash_OPSLIMIT_MIN` - `crypto_pwhash_OPSLIMIT_MAX`
* `memlimit` should be a number containing your mem limit setting in the range `crypto_pwhash_MEMLIMIT_MIN` - `crypto_pwhash_OPSLIMIT_MAX`

Returns `true` if the hash should be rehashed the settings contained in `str`. Otherwise `false` if it is still good.

``` js
crypto_pwhash_async(output, password, salt, opslimit, memlimit, algorithm, callback)
```
Just like `crypto_pwhash` but will run password hashing on a seperate worker so it will not block the event loop. `callback(err)` will receive any errors from the hashing but all argument errors will throw. The resulting hash is written to `output`. This function also supports [`async_hook`s](https://nodejs.org/dist/latest/docs/api/async_hooks.html) as the type `sodium-native:crypto_pwhash_async`.

``` js
crypto_pwhash_str_async(output, password, opslimit, memlimit, callback)
```
Just like `crypto_pwhash_str` but will run password hashing on a seperate worker so it will not block the event loop. `callback(err)` will receive any errors from the hashing but all argument errors will throw. The resulting hash with parameters is written to `output`. This function also supports [`async_hook`s](https://nodejs.org/dist/latest/docs/api/async_hooks.html) as the type `sodium-native:crypto_pwhash_str_async`.

``` js
crypto_pwhash_str_verify_async(str, password, callback)
```
Just like `crypto_pwhash_str_verify` but will run password hashing on a seperate worker so it will not block the event loop. `callback(err, bool)` will receive any errors from the hashing but all argument errors will throw. If the verification succeeds bool is `true`, otherwise `false`. Due to an issue with libsodium `err` is currently never set. This function also supports [`async_hook`s](https://nodejs.org/dist/latest/docs/api/async_hooks.html) as the type `sodium-native:crypto_pwhash_str_verify_async`.

## Password Hashing (Scrypt)
Bindings for the crypto_pwhash_scryptsalsa208sha256 API. [See the libsodium crypto_pwhash_scryptsalsa208sha256 docs for more information](https://download.libsodium.org/doc/advanced/scrypt).

``` js
crypto_pwhash_scryptsalsa208sha256(output, password, salt, opslimit, memlimit)
```
Create a password hash.
* `output` should be a `buffer` with length within `crypto_pwhash_scryptsalsa208sha256_BYTES_MIN` - `crypto_pwhash_scryptsalsa208sha256_BYTES_MAX`
* `password` should be a `buffer` of any size
* `salt` should be a `buffer` with length `crypto_pwhash_scryptsalsa208sha256_SALTBYTES`
* `opslimit` should be a number containing your ops limit setting in the range `crypto_pwhash_scryptsalsa208sha256_OPSLIMIT_MIN` - `crypto_pwhash_scryptsalsa208sha256_OPSLIMIT_MAX`
* `memlimit` should be a number containing your mem limit setting in the range `crypto_pwhash_scryptsalsa208sha256_MEMLIMIT_MIN` - `crypto_pwhash_scryptsalsa208sha256_OPSLIMIT_MAX`

Available default ops and mem limits are
* `crypto_pwhash_scryptsalsa208sha256_OPSLIMIT_INTERACTIVE`
* `crypto_pwhash_scryptsalsa208sha256_OPSLIMIT_SENSITIVE`
* `crypto_pwhash_scryptsalsa208sha256_MEMLIMIT_INTERACTIVE`
* `crypto_pwhash_scryptsalsa208sha256_MEMLIMIT_SENSITIVE`

The generated hash will be stored in `output` and the entire `output buffer` will be used.

``` js
crypto_pwhash_scryptsalsa208sha256_str(output, password, opslimit, memlimit)
```
Create a password hash with a random salt.
* `output` should be a `buffer` with length `crypto_pwhash_scryptsalsa208sha256_STRBYTES`
* `password` should be a `buffer` of any size
* `opslimit` should be a number containing your ops limit setting in the range `crypto_pwhash_scryptsalsa208sha256_OPSLIMIT_MIN` - `crypto_pwhash_scryptsalsa208sha256_OPSLIMIT_MAX`
* `memlimit` should be a number containing your mem limit setting in the range `crypto_pwhash_scryptsalsa208sha256_MEMLIMIT_MIN` - `crypto_pwhash_scryptsalsa208sha256_OPSLIMIT_MAX`

The generated hash, settings, salt, version and algorithm will be stored in `output` and the entire `output buffer` will be used.

``` js
var bool = crypto_pwhash_scryptsalsa208sha256_str_verify(str, password)
```
Verify a password hash generated with the above method.
* `str` should be a `buffer` with length `crypto_pwhash_scryptsalsa208sha256_STRBYTES`
* `password` should be a `buffer` of any size

Returns `true` if the hash could be verified with the settings contained in `str`. Otherwise `false`.

``` js
var bool = crypto_pwhash_scryptsalsa208sha256_str_needs_rehash(hash, opslimit, memlimit)
```
Check if a password hash needs rehash, either because `opslimit` or `memlimit` increased or because the hash is malformed.
* `hash` should be a `buffer` with length `crypto_pwhash_scryptsalsa208sha256_STRBYTES`
* `opslimit` should be a number containing your ops limit setting in the range `crypto_pwhash_scryptsalsa208sha256_OPSLIMIT_MIN` - `crypto_pwhash_scryptsalsa208sha256_OPSLIMIT_MAX`
* `memlimit` should be a number containing your mem limit setting in the range `crypto_pwhash_scryptsalsa208sha256_MEMLIMIT_MIN` - `crypto_pwhash_scryptsalsa208sha256_OPSLIMIT_MAX`

Returns `true` if the hash should be rehashed the settings contained in `str`. Otherwise `false` if it is still good.

``` js
crypto_pwhash_scryptsalsa208sha256_async(output, password, salt, opslimit, memlimit, callback)
```
Just like `crypto_pwhash_scryptsalsa208sha256` but will run password hashing on a seperate worker so it will not block the event loop. `callback(err)` will receive any errors from the hashing but all argument errors will throw. The resulting hash is written to `output`. This function also supports [`async_hook`s](https://nodejs.org/dist/latest/docs/api/async_hooks.html) as the type `sodium-native:crypto_pwhash_scryptsalsa208sha256_async`.

``` js
crypto_pwhash_scryptsalsa208sha256_str_async(output, password, opslimit, memlimit, callback)
```
Just like `crypto_pwhash_scryptsalsa208sha256_str` but will run password hashing on a seperate worker so it will not block the event loop. `callback(err)` will receive any errors from the hashing but all argument errors will throw. The resulting hash with parameters is written to `output`. This function also supports [`async_hook`s](https://nodejs.org/dist/latest/docs/api/async_hooks.html) as the type `sodium-native:crypto_pwhash_scryptsalsa208sha256_str_async`.

``` js
crypto_pwhash_scryptsalsa208sha256_str_verify_async(str, password, callback)
```
Just like `crypto_pwhash_scryptsalsa208sha256_str_verify` but will run password hashing on a seperate worker so it will not block the event loop. `callback(err, bool)` will receive any errors from the hashing but all argument errors will throw. If the verification succeeds bool is `true`, otherwise `false`. Due to an issue with libsodium `err` is currently never set. This function also supports [`async_hook`s](https://nodejs.org/dist/latest/docs/api/async_hooks.html) as the type `sodium-native:crypto_pwhash_scryptsalsa208sha256_str_verify_async`.

## Key exchange
Bindings for the crypto_kx API. [See the libsodium crypto_kx docs for more information](https://download.libsodium.org/doc/key_exchange/).

``` js
crypto_kx_keypair(publicKey, secretKey)
```
Create a key exchange key pair.
* `publicKey` should be a `buffer` of length `crypto_kx_PUBLICKEYBYTES`
* `secretKey` should be a `buffer` of length `crypto_kx_SECRETKEYBYTES`

``` js
crypto_kx_seed_keypair(publicKey, secretKey, seed)
```
Create a key exchange key pair based on a seed.
* `publicKey` should be a `buffer` of length `crypto_kx_PUBLICKEYBYTES`
* `secretKey` should be a `buffer` of length `crypto_kx_SECRETKEYBYTES`
* `seed` should be a `buffer` of length `crypto_kx_SEEDBYTES`

``` js
crypto_kx_client_session_keys(rx, tx, clientPublicKey, clientSecretKey, serverPublicKey)
```
Generate a session receive and transmission key for a client. The public / secret keys should be generated using the key pair method above.
* `rx` should be a `buffer` of length `crypto_kx_SESSIONKEYBYTES` or `null`
* `tx` should be a `buffer` of length `crypto_kx_SESSIONKEYBYTES` or `null`

You should use the `rx` to decrypt incoming data and `tx` to encrypt outgoing. If you need to make a one-way or half-duplex channel you can give only one of `rx` or `tx`.

``` js
crypto_kx_server_session_keys(rx, tx, serverPublicKey, serverSecretKey, clientPublicKey)
```
Generate a session receive and transmission key for a server. The public / secret keys should be generated using the key pair method above.

* `rx` should be a `buffer` of length `crypto_kx_SESSIONKEYBYTES` or `null`
* `tx` should be a `buffer` of length `crypto_kx_SESSIONKEYBYTES` or `null`

You should use the `rx` to decrypt incoming data and `tx` to encrypt outgoing. If you need to make a one-way or half-duplex channel you can give only one of `rx` or `tx`.

## Diffie-Hellman
Bindings for the crypto_scalarmult API. [See the libsodium crypto_scalarmult docs for more information](https://download.libsodium.org/doc/advanced/scalar_multiplication).

``` js
crypto_scalarmult_base(publicKey, secretKey)
```
Create a scalar multiplication public key based on a secret key
* `publicKey` should be a `buffer` of length `crypto_scalarmult_BYTES`
* `secretKey` should be a `buffer` of length `crypto_scalarmult_SCALARBYTES`

The generated public key is stored in `publicKey`.

``` js
crypto_scalarmult(sharedSecret, secretKey, remotePublicKey)
```
Derive a shared secret from a local secret key and a remote public key.
* `sharedSecret` should be a `buffer` of length `crypto_scalarmult_BYTES`
* `secretKey` should be a `buffer` of length `crypto_scalarmult_SCALARBYTES`
* `remotePublicKey` should be a `buffer` of length `crypto_scalarmult_BYTES`

The generated shared secret is stored in `sharedSecret`.

## Finite field aritmhetic
Bindings for the crypto_scalarmult_ed25519 and crypto_core_ed25519 API. [See the libsodium docs for more information](https://download.libsodium.org/doc/advanced/point-arithmetic).

### Constants
* `crypto_scalarmult_ed25519_BYTES`
* `crypto_scalarmult_ed25519_SCALARBYTES`
* `crypto_core_ed25519_BYTES`
* `crypto_core_ed25519_UNIFORMBYTES`
* `crypto_core_ed25519_SCALARBYTES`
* `crypto_core_ed25519_NONREDUCEDSCALARBYTES`

``` js
var bool = crypto_core_ed25519_is_valid_point(p)
```
>The crypto_core_ed25519_is_valid_point() function checks that p represents a point on the edwards25519 curve, in canonical >form, on the main subgroup, and that the point doesn't have a small order.

* `p` must be a `buffer` of at least `crypto_core_ed25519_BYTES` bytes

Returns `true` or `false``

``` js
crypto_core_ed25519_from_uniform(p, r)
```
Maps a `crypto_core_ed25519_UNIFORMBYTES` bytes vector (usually the output of a hash function) to a a valid curve point and stores its compressed representation in `p`.

The point is guaranteed to be on the main subgroup.
* `p` must be a `buffer` of at least `crypto_core_ed25519_BYTES` bytes
* `r` must be a `buffer` of at least `crypto_core_ed25519_UNIFORMBYTES` bytes

``` js
crypto_scalarmult_ed25519(q, n, p)
```
Multiply point `p` by scalar `n` and store its compressed representation in `q`.
* `q` must be a `buffer` of at least `crypto_scalarmult_ed25519_BYTES` bytes
* `n` must be a `buffer` of at least `crypto_scalarmult_ed25519_SCALARBYTES` bytes
* `p` must be a `buffer` of at least `crypto_scalarmult_ed25519_BYTES` bytes

Note this function will throw if `n` is zero or `p` is an invalid curve point.

``` js
crypto_scalarmult_ed25519_base(q, n)
```
Multiply the basepoint by scalar `n` and store its compressed representation in `q`. Note that `n` will be clamped.
* `q` must be a `buffer` of at least `crypto_scalarmult_ed25519_BYTES` bytes
* `n` must be a `buffer` of at least `crypto_scalarmult_ed25519_SCALARBYTES` bytes

Note this function will throw if `n` is zero.

``` js
crypto_scalarmult_ed25519_noclamp(q, n, p)
```
Multiply point `p` by scalar `n` and store its compressed representation in `q`. This version does not clamp.
* `q` must be a `buffer` of at least `crypto_scalarmult_ed25519_BYTES` bytes
* `n` must be a `buffer` of at least `crypto_scalarmult_ed25519_SCALARBYTES` bytes
* `p` must be a `buffer` of at least `crypto_scalarmult_ed25519_BYTES` bytes

Note this function will throw if `n` is zero or `p` is an invalid curve point.

``` js
crypto_scalarmult_ed25519_base_noclamp(q, n)
```
Multiply the basepoint by scalar `n` and store its compressed representation in `q`. This version does not clamp.
* `q` must be a `buffer` of at least `crypto_scalarmult_ed25519_BYTES` bytes
* `n` must be a `buffer` of at least `crypto_scalarmult_ed25519_SCALARBYTES` bytes

Note this function will throw if `n` is zero.

``` js
crypto_core_ed25519_add(r, p, q)
```
Add point `q` to `p`, storing the result to `r`.
* `r` must be a `buffer` of at least `crypto_core_ed25519_BYTES` bytes
* `p` must be a `buffer` of at least `crypto_core_ed25519_BYTES` bytes
* `q` must be a `buffer` of at least `crypto_core_ed25519_BYTES` bytes

Will throw if `p`, `q` are not valid curve points

``` js
crypto_core_ed25519_sub(r, p, q)
```
Subtract point `q` to `p`, storing the result to `r`.
* `r` must be a `buffer` of at least `crypto_core_ed25519_BYTES` bytes
* `p` must be a `buffer` of at least `crypto_core_ed25519_BYTES` bytes
* `q` must be a `buffer` of at least `crypto_core_ed25519_BYTES` bytes

Will throw if `p`, `q` are not valid curve points

``` js
crypto_core_ed25519_scalar_random(r)
```
Generate random scalar in `]0..L[`, storing it in `r`.
* `r` must be a `buffer` of at least `crypto_core_ed25519_SCALARBYTES` bytes

``` js
crypto_core_ed25519_scalar_reduce(r, s)
```
Reduce `s mod L`, storing it in `r`.
* `r` must be a `buffer` of at least `crypto_core_ed25519_SCALARBYTES` bytes
* `s` must be a `buffer` of at least `crypto_core_ed25519_NONREDUCEDSCALARBYTES` bytes

``` js
crypto_core_ed25519_scalar_invert(recip, s)
```
Find `recip` such that `s * recip = 1 (mod L)`, storing it in `recip`.
* `recip` must be a `buffer` of at least `crypto_core_ed25519_SCALARBYTES` bytes
* `s` must be a `buffer` of at least `crypto_core_ed25519_SCALARBYTES` bytes

``` js
crypto_core_ed25519_scalar_negate(neg, s)
```
Find `neg` such that `s + neg = 0 (mod L)`, storing it in `recip`.
* `recip` must be a `buffer` of at least `crypto_core_ed25519_SCALARBYTES` bytes
* `s` must be a `buffer` of at least `crypto_core_ed25519_SCALARBYTES` bytes

``` js
crypto_core_ed25519_scalar_complement(comp, s)
```
Find `comp` such that `s + comp = 1 (mod L)`, storing it in `recip`.
* `comp` must be a `buffer` of at least `crypto_core_ed25519_SCALARBYTES` bytes
* `s` must be a `buffer` of at least `crypto_core_ed25519_SCALARBYTES` bytes

``` js
crypto_core_ed25519_scalar_add(z, x, y)
```
Add `x` and `y` such that `x + y = z (mod L)`, storing it in `z`.
* `x` must be a `buffer` of at least `crypto_core_ed25519_SCALARBYTES` bytes
* `y` must be a `buffer` of at least `crypto_core_ed25519_SCALARBYTES` bytes
* `z` must be a `buffer` of at least `crypto_core_ed25519_SCALARBYTES` bytes

``` js
crypto_core_ed25519_scalar_sub(z, x, y)
```
Subtract `x` and `y` such that `x - y = z (mod L)`, storing it in `z`.
* `x` must be a `buffer` of at least `crypto_core_ed25519_SCALARBYTES` bytes
* `y` must be a `buffer` of at least `crypto_core_ed25519_SCALARBYTES` bytes
* `z` must be a `buffer` of at least `crypto_core_ed25519_SCALARBYTES` bytes

## Short hashes
Bindings for the crypto_shorthash API. [See the libsodium crypto_shorthash docs for more information](https://download.libsodium.org/doc/hashing/short-input_hashing).

``` js
crypto_shorthash(output, input, key)
```
Hash a value to a short hash based on a key.
* `output` should be a `buffer` of length `crypto_shorthash_BYTES`
* `input` should be a `buffer` of any size
* `key` should be a `buffer` of length `crypto_shorthash_KEYBYTES`

The generated short hash is stored in `output`.

## Key derivation
Bindings for the crypto_kdf API. [See the libsodium crypto_kdf docs for more information](https://download.libsodium.org/doc/key_derivation/).

``` js
crypto_kdf_keygen(key)
```
Generate a new master key.
* `key` should be a `buffer` of length `crypto_kdf_KEYBYTES`

``` js
crypto_kdf_derive_from_key(subkey, subkeyId, context, key)
```
Derive a new key from a master key.
* `subkey` should be a `buffer` between `crypto_kdf_BYTES_MIN` and `crypto_kdf_BYTES_MAX`
* `subkeyId` should be an integer
* `context` should be a `buffer` of length `crypto_kdf_CONTEXTBYTES`
* `key` should be a `buffer` of length `crypto_kdf_KEYBYTES`

## SHA
``` js
crypto_hash_sha256(output, input)
```
Hash a value to a short hash based on a key.
* `output` should be a `buffer` of length `crypto_hash_sha256_BYTES`
* `input` should be a `buffer` of any size

The generated short hash is stored in `output`.

``` js
var instance = crypto_hash_sha256_instance()
```
Create an instance that has stream of input data to sha256.

``` js
instance.update(input)
```
Update the instance with a new piece of data.
* `input` should be a `buffer` of any size

``` js
instance.final(output)
```
Finalize the instance.
* `output` should be a `buffer` of length `crypto_hash_sha256_BYTES`

The generated hash is stored in `output`.

``` js
crypto_hash_sha512(output, input)
```
Hash a value to a short hash based on a key.
* `output` should be a `buffer` of length `crypto_hash_sha512_BYTES`
* `input` should be a `buffer` of any size

The generated short hash is stored in `output`.

``` js
var instance = crypto_hash_sha512_instance()
```
Create an instance that has stream of input data to sha512.

``` js
instance.update(input)
```
Update the instance with a new piece of data.
* `input` should be a `buffer` of any size

``` js
instance.final(output)
```
Finalize the instance.
* `output` should be a `buffer` of `length crypto_hash_sha512_BYTES`

The generated hash is stored in `output`.

## License



