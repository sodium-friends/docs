---
id: streamencryption
title: Stream encryption
sidebar_label: Stream encryption
---

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
