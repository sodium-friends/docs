---
id: streamencryption
title: Stream Encryption
sidebar_label: Stream Encryption
---

Bindings for the crypto_secretstream API. [See the libsodium crypto_secretstream docs for more information](https://download.libsodium.org/doc/secret-key_cryptography/secretstream).

### Constants
**Buffer lengths (integer)**
* `crypto_secretstream_xchacha20poly1305_ABYTES`
* `crypto_secretstream_xchacha20poly1305_HEADERBYTES`
* `crypto_secretstream_xchacha20poly1305_KEYBYTES`
* `crypto_secretstream_xchacha20poly1305_MESSAGEBYTES_MAX`
* `crypto_secretstream_xchacha20poly1305_TAGBYTES` - NOTE: Unofficial constant
* `crypto_secretstream_xchacha20poly1305_STATEBYTES`

**Message tags (buffer)**
* `crypto_secretstream_xchacha20poly1305_TAG_MESSAGE`
* `crypto_secretstream_xchacha20poly1305_TAG_PUSH`
* `crypto_secretstream_xchacha20poly1305_TAG_REKEY`
* `crypto_secretstream_xchacha20poly1305_TAG_FINAL`

***
## `crypto_secretstream_xchacha20poly1305_keygen`
![sodium-native][node]
``` js
sodium.crypto_secretstream_xchacha20poly1305_keygen(key)
```
Generates a new encryption key.
* `key` should be a `buffer` of length `crypto_secretstream_xchacha20poly1305_KEYBYTES`

The generated key is stored in `key`.
***
## Opaque API
### `crypto_secretstream_xchacha20poly1305_state_new`
![sodium-native][node]
``` js
var state = Buffer.alloc(sodium.crypto_secretstream_xchacha20poly1305_state_new()
```
Creates a new stream state. Returns an opaque object used in the next methods.
***
### `crypto_secretstream_xchacha20poly1305_init_push`
![sodium-native][node]
``` js
sodium.crypto_secretstream_xchacha20poly1305_init_push(state, header, key)
```
Initializes `state` from the writer side with message `header` and encryption `key`. The `header` must be sent or stored with the stream. The `key` must be exchanged securely with the receiving / reading side.
* `state` should be an opaque state object
* `header` should be a `buffer` of length `crypto_secretstream_xchacha20poly1305_HEADERBYTES`
* `key` should be a `buffer` of length `crypto_secretstream_xchacha20poly1305_KEYBYTES`
***
### `crypto_secretstream_xchacha20poly1305_push`
![sodium-native][node]
``` js
var mlen = sodium.crypto_secretstream_xchacha20poly1305_push(state, ciphertext, message, [ad], tag)
```
Encrypts a message with a certain tag and optional additional data `ad`.
* `state` should be an opaque state object
* `ciphertext` should be a `buffer` of length `message.length + crypto_secretstream_xchacha20poly1305_ABYTES`
* `message` should be a `buffer`
* `ad` is optional and should be `null` or `buffer`. Included in the computation of authentication tag appended to the message
* `tag` should be a `buffer` of length `crypto_secretstream_xchacha20poly1305_TAGBYTES`

Note that `tag` should be one of the `crypto_secretstream_xchacha20poly1305_TAG_*` constants. Returns number of encrypted bytes written to `ciphertext`.
***
### `crypto_secretstream_xchacha20poly1305_init_pull`
![sodium-native][node]
``` js
sodium.crypto_secretstream_xchacha20poly1305_init_pull(state, header, key)
```
Initializes `state` from the reader side with message `header` and encryption `key`. The `header` must be retrieved from somewhere. The `key` must be exchanged securely with the sending / writing side.
* `state` should be an opaque state object
* `header` should be a `buffer` of length `crypto_secretstream_xchacha20poly1305_HEADERBYTES`
* `key` should be a `buffer` of length `crypto_secretstream_xchacha20poly1305_KEYBYTES`
***
### `crypto_secretstream_xchacha20poly1305_pull`
![sodium-native][node]
``` js
var clen = sodium.crypto_secretstream_xchacha20poly1305_pull(state, message, tag, ciphertext, [ad])
```
Decrypts a message with optional additional data `ad`, and writes message tag to `tag`. Make sure to check this!
* `state` should be an opaque state object
* `message` should be a `buffer` of length `ciphertext.length - crypto_secretstream_xchacha20poly1305_ABYTES`
* `tag` should be a `buffer` of length `crypto_secretstream_xchacha20poly1305_TAGBYTES`
* `ad` is optional and should be `null` or `buffer`. Included in the computation of the authentication tag appended to the message

Note that `tag` should be one of the `crypto_secretstream_xchacha20poly1305_TAG_*` constants. Returns number of decrypted bytes written to `message`.
***
### `crypto_secretstream_xchacha20poly1305_rekey`
![sodium-native][node]
``` js
sodium.crypto_secretstream_xchacha20poly1305_rekey(state)
```
Rekeys the opaque `state` object.

## Stateful API
Replaces the above instance implementation in the N-API release.
``` js
var state = Buffer.alloc(sodium.crypto_secretstream_xchacha20poly1305_STATEBYTES)
```
Create a new `buffer` to hold stream state.
***
### `crypto_secretstream_xchacha20poly1305_init_push`
![sodium-native][node]
``` js
sodium.crypto_secretstream_xchacha20poly1305_init_push(state, header, key)
```
Initializes `state` from the writer side with message `header` and encryption `key`. The `header` must be sent or stored with the stream. The `key` must be exchanged securely with the receiving / reading side.
* `state` should be a `buffer` of length `crypto_secretstream_xchacha20_poly1305_STATEBYTES` bytes
* `header` should be a `buffer` of length `crypto_secretstream_xchacha20poly1305_HEADERBYTES`
* `key` should be a `buffer` of length `crypto_secretstream_xchacha20poly1305_KEYBYTES`
***
### `crypto_secretstream_xchacha20poly1305_push`
![sodium-native][node]
``` js
var mlen = sodium.crypto_secretstream_xchacha20poly1305_push(state, ciphertext, message, [ad], tag)
```
Encrypts a message with a certain tag and optional additional data `ad`.
* `state` should be a `buffer` of length `crypto_secretstream_xchacha20_poly1305_STATEBYTES` bytes
* `ciphertext` should be a `buffer` of length `message.length + crypto_secretstream_xchacha20poly1305_ABYTES`
* `message` should be a `buffer`
* `ad` is optional and should be `null` or `buffer`. Included in the computation of authentication tag appended to the message
* `tag` should be a `buffer` of length `crypto_secretstream_xchacha20poly1305_TAGBYTES`

Note that `tag` should be one of the `crypto_secretstream_xchacha20poly1305_TAG_*` constants. Returns number of encrypted bytes written to `ciphertext`.
***
### `crypto_secretstream_xchacha20poly1305_init_pull`
![sodium-native][node]
``` js
sodium.crypto_secretstream_xchacha20poly1305_init_pull(state, header, key)
```
Initializes `state` from the reader side with message `header` and encryption `key`. The `header` must be retrieved from somewhere. The `key` must be exchanged securely with the sending / writing side.
* `state` should be a `buffer` of length `crypto_secretstream_xchacha20_poly1305_STATEBYTES` bytes
* `header` should be a `buffer` of length `crypto_secretstream_xchacha20poly1305_HEADERBYTES`
* `key` should be a `buffer` of length `crypto_secretstream_xchacha20poly1305_KEYBYTES`
***
### `crypto_secretstream_xchacha20poly1305_pull`
![sodium-native][node]
``` js
var clen = sodium.crypto_secretstream_xchacha20poly1305_pull(state, message, tag, ciphertext, [ad])
```
Decrypts a message with optional additional data `ad`, and writes message tag to `tag`. Make sure to check this!
* `state` should be a `buffer` of length `crypto_secretstream_xchacha20_poly1305_STATEBYTES` bytes
* `message` should be a `buffer` of length `ciphertext.length - crypto_secretstream_xchacha20poly1305_ABYTES`
* `tag` should be a `buffer` of length `crypto_secretstream_xchacha20poly1305_TAGBYTES`
* `ad` is optional and should be `null` or `buffer`. Included in the computation of the authentication tag appended to the message

Note that `tag` should be one of the `crypto_secretstream_xchacha20poly1305_TAG_*` constants. Returns number of decrypted bytes written to `message`.
***
### `crypto_secretstream_xchacha20poly1305_rekey`
![sodium-native][node]
``` js
sodium.crypto_secretstream_xchacha20poly1305_rekey(state)
```
Rekeys the `state` buffer.

[js]: /docs/img/icon_js.svg
[node]: /docs/img/nodejs-icon.svg
