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
* `b1` must be `buffer`
* `b2` must be `buffer` and must be `b1.length` bytes

``` js
sodium.sodium_add(a, b)
```
* `a` must be `buffer`
* `b` must be `buffer` and must be `a.length` bytes

``` js
sodium.sodium_sub(a, b)
```
* `a` must be `buffer`
* `b` must be `buffer` and must be `a.length` bytes

``` js
sodium.sodium_increment(buf)
```
* `buf` must be `buffer`

``` js
var bool = sodium.sodium_is_zero(buf, len)
```
* `len` must be integer at most the length of `buf`

## Padding

``` js
var paddedLength = sodium.sodium_pad(buf, unpaddedLength, blocksize)
```
* `buf` must be `buffer`
* `unpadded_buflen` must be integer at most `buf.length`
* `blocksize` must be integer greater than 1 but at most `buf.length`

``` js
var unpaddedLength = sodium.sodium_unpad(buf, paddedLength, blocksize)
```
* `buf` must be `buffer`
* `padded_buflen` must be integer at most `buf.length`
* `blocksize` must be integer greater than 1 but at most `buf.length`

## Signing

``` js
crypto_sign_seed_keypair(publicKey, secretKey, seed)
```
* `publicKey` should be a buffer with length `crypto_sign_PUBLICKEYBYTES`
* `secretKey` should be a buffer with length `crypto_sign_SECRETKEYBYTES`
* `seed` should be a buffer with length `crypto_sign_SEEDBYTES`

``` js
crypto_sign_keypair(publicKey, secretKey)
```
* `publicKey` should be a buffer with length `crypto_sign_PUBLICKEYBYTES`
* `secretKey` should be a buffer with length `crypto_sign_SECRETKEYBYTES`

``` js
crypto_sign(signedMessage, message, secretKey)
```
* `signedMessage` should be a buffer with length `crypto_sign_BYTES + message.length`
* `message` should be a buffer of any length
* `secretKey` should be a secret key

``` js
var bool = crypto_sign_open(message, signedMessage, publicKey)
```
* `message` should be a buffer with length `signedMessage.length - crypto_sign_BYTES`
* `signedMessage` at least `crypto_sign_BYTES` length
* `publicKey` should be a public key

``` js
crypto_sign_detached(signature, message, secretKey)
```
* `signature` should be a buffer with length `crypto_sign_BYTES`
* `message` should be a buffer of any length
* `secretKey` should be a secret key

``` js
var bool = crypto_sign_verify_detached(signature, message, publicKey)
```
* `signature` should be a buffer with length `crypto_sign_BYTES`
* `message` should be a buffer of any length
* `publicKey` should be a public key

``` js
crypto_sign_ed25519_pk_to_curve25519(curve_pk, ed_pk)
```
* `curve_pk` should be a buffer with length `crypto_box_PUBLICKEYBYTES`
* `ed_pk` should be a buffer with length `crypto_sign_PUBLICKEYBYTES`

``` js
crypto_sign_ed25519_sk_to_curve25519(curve_sk, ed_sk)
```
* `curve_sk` should be a buffer with length `crypto_box_SECRETKEYBYTES`
* `ed_sk` should be a buffer with length `crypto_sign_SECRETKEYBYTES`

``` js
crypto_sign_ed25519_sk_to_pk(pk, sk)
```
* `pk` must be `buffer` of at least `crypto_box_PUBLICKEYBYTES` bytes
* `sk` must be `buffer` of at least `crypto_sign_SECRETKEYBYTES` bytes

## Generic hashing

``` js
crypto_generichash(output, input, [key])
```
* `output` should be a buffer with length within `crypto_generichash_BYTES_MIN` - `crypto_generichash_BYTES_MAX`
* `input` should be a buffer of any length
* `key` is an optional buffer of length within `crypto_generichash_KEYBYTES_MIN` - `crypto_generichash_KEYBYTES_MAX`

``` js
crypto_generichash_batch(output, inputArray, [key])
```

``` js
var instance = crypto_generichash_instance([key], [outputLength])
```
* `key` is an optional buffer as above
* `outputLength` the buffer size of your output

``` js
instance.update(input)
```
* `input` should be a buffer of any size

``` js
instance.final(output)
```
* `output` should be a buffer as above with the same length you gave when creating the instance

## Public/secret key box encryption

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

### Constants

``` js
crypto_aead_xchacha20poly1305_ietf_keygen(key)
```

``` js
var clen = crypto_aead_xchacha20poly1305_ietf_encrypt(ciphertext, message, [ad], null, npub, key)
```

``` js
var mlen = crypto_aead_xchacha20poly1305_ietf_decrypt(message, null, ciphertext, [ad], npub, key)
```

``` js
var maclen = crypto_aead_xchacha20poly1305_ietf_encrypt_detached(ciphertext, mac, message, [ad], null, npub, key)
```

``` js
crypto_aead_xchacha20poly1305_ietf_decrypt_detached(message, null, ciphertext, mac, [ad], npub, key)
```

## Non-authenticated streaming encryption

``` js
crypto_stream(ciphertext, nonce, key)
```

``` js
crypto_stream_xor(ciphertext, message, nonce, key)
```

``` js
crypto_stream_chacha20_xor(ciphertext, message, nonce, key)
```

``` js
var instance = crypto_stream_xor_instance(nonce, key)
```

``` js
var istance = crypto_stream_chacha20_xor_instance(nonce, key)
```

``` js
instance.update(ciphertext, message)
```

``` js
instance.final()
```

## Authentication

``` js
crypto_auth(output, input, key)
```

``` js
var bool = crypto_auth_verify(output, input, key)
```

## Stream encryption

### Constants

``` js
crypto_secretstream_xchacha20poly1305_keygen(key)
```

``` js
var state = crypto_secretstream_xchacha20poly1305_state_new()
```

``` js
crypto_secretstream_xchacha20poly1305_init_push(state, header, key)
```

``` js
var mlen = crypto_secretstream_xchacha20poly1305_push(state, ciphertext, message, [ad], tag)
```

``` js
crypto_secretstream_xchacha20poly1305_init_pull(state, header, key)
```

``` js
var clen = crypto_secretstream_xchacha20poly1305_pull(state, message, tag, ciphertext, [ad])
```

``` js
crypto_secretstream_xchacha20poly1305_rekey(state)
```

## One-time Authentication

``` js
crypto_onetimeauth(output, input, key)
```

``` js
var bool = crypto_onetimeauth_verify(output, input, key)
```

``` js
var instance = crypto_onetimeauth_instance(key)
```

``` js
instance.update(input)
```

``` js
instance.final(output)
```

## Password Hashing

``` js
crypto_pwhash(output, password, salt, opslimit, memlimit, algorithm)
```

``` js
crypto_pwhash_str(output, password, opslimit, memlimit)
```

``` js
var bool = crypto_pwhash_str_verify(str, password)
```

``` js
var bool = crypto_pwhash_str_needs_rehash(hash, opslimit, memlimit)
```

``` js
crypto_pwhash_async(output, password, salt, opslimit, memlimit, algorithm, callback)
```

``` js
crypto_pwhash_str_async(output, password, opslimit, memlimit, callback)
```

``` js
crypto_pwhash_str_verify_async(str, password, callback)
```

## Password Hashing (Scrypt)

``` js
crypto_pwhash_scryptsalsa208sha256(output, password, salt, opslimit, memlimit)
```

``` js
crypto_pwhash_scryptsalsa208sha256_str(output, password, opslimit, memlimit)
```

``` js
var bool = crypto_pwhash_scryptsalsa208sha256_str_verify(str, password)
```

``` js
var bool = crypto_pwhash_scryptsalsa208sha256_str_needs_rehash(hash, opslimit, memlimit)
```

``` js
crypto_pwhash_scryptsalsa208sha256_async(output, password, salt, opslimit, memlimit, callback)
```

``` js
crypto_pwhash_scryptsalsa208sha256_str_async(output, password, opslimit, memlimit, callback)
```

``` js
crypto_pwhash_scryptsalsa208sha256_str_verify_async(str, password, callback)
```

## Key exchange

``` js
crypto_kx_keypair(publicKey, secretKey)
```

``` js
crypto_kx_seed_keypair(publicKey, secretKey, seed)
```

``` js
crypto_kx_client_session_keys(rx, tx, clientPublicKey, clientSecretKey, serverPublicKey)
```

``` js
crypto_kx_server_session_keys(rx, tx, serverPublicKey, serverSecretKey, clientPublicKey)
```

## Diffie-Hellman

``` js
crypto_scalarmult_base(publicKey, secretKey)
```

``` js
crypto_scalarmult(sharedSecret, secretKey, remotePublicKey)
```

## Finite field arithmetic

``` js
var bool = crypto_core_ed25519_is_valid_point(p)
```

``` js
crypto_core_ed25519_from_uniform(p, r)
```

``` js
crypto_scalarmult_ed25519(q, n, p)
```

``` js
crypto_scalarmult_ed25519_base(q, n)
```

``` js
crypto_scalarmult_ed25519_noclamp(q, n, p)
```

``` js
crypto_scalarmult_ed25519_base_noclamp(q, n)
```

``` js
crypto_core_ed25519_add(r, p, q)
```

``` js
crypto_core_ed25519_sub(r, p, q)
```

``` js
crypto_core_ed25519_scalar_random(r)
```

``` js
crypto_core_ed25519_scalar_reduce(r, s)
```

``` js
crypto_core_ed25519_scalar_invert(recip, s)
```

``` js
crypto_core_ed25519_scalar_negate(neg, s)
```

``` js
crypto_core_ed25519_scalar_complement(comp, s)
```

``` js
crypto_core_ed25519_scalar_add(z, x, y)
```

``` js
crypto_core_ed25519_scalar_sub(z, x, y)
```

## Short hashes

``` js
crypto_shorthash(output, input, key)
```

## Key derivation

``` js
crypto_kdf_keygen(key)
```

``` js
crypto_kdf_derive_from_key(subkey, subkeyId, context, key)
```

## SHA

``` js
crypto_hash_sha256(output, input)
```

``` js
var instance = crypto_hash_sha256_instance()
```

``` js
instance.update(input)
```

``` js
instance.final(output)
```

``` js
crypto_hash_sha512(output, input)
```

``` js
var instance = crypto_hash_sha512_instance()
```

``` js
instance.update(input)
```

``` js
instance.final(output)
```

## License



