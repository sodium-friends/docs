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
sodium.crypto_secretstream_xchacha20poly1305_keygen(k)
```
Generates a new encryption key.
* `k` should be a `buffer` of length `crypto_secretstream_xchacha20poly1305_KEYBYTES`

The generated key is stored in `k`.
***
## Opaque API
__No longer supported from sodium-native v3.0.0, see Stateful API below__

## `crypto_secretstream_xchacha20poly1305_state_new`
![sodium-native][node]
``` js
var state = sodium.crypto_secretstream_xchacha20poly1305_state_new()
```
Creates a new stream state. Returns an opaque object used in the next methods.
***
## `crypto_secretstream_xchacha20poly1305_init_push`
![sodium-native][node]
``` js
sodium.crypto_secretstream_xchacha20poly1305_init_push(state, header, k)
```
Initializes `state` from the writer side with message `header` and encryption `key`. The `header` must be sent or stored with the stream. The `key` must be exchanged securely with the receiving / reading side.
* `state` should be an opaque state object
* `header` should be a `buffer` of length `crypto_secretstream_xchacha20poly1305_HEADERBYTES`
* `k` should be a `buffer` of length `crypto_secretstream_xchacha20poly1305_KEYBYTES`
***
## `crypto_secretstream_xchacha20poly1305_push`
![sodium-native][node]
``` js
var mlen = sodium.crypto_secretstream_xchacha20poly1305_push(state, c, m, [ad], tag)
```
Encrypts a message with a certain tag and optional additional data `ad`.
* `state` should be an opaque state object
* `c` should be a `buffer` of length `m.length + crypto_secretstream_xchacha20poly1305_ABYTES`
* `m` should be a `buffer`
* `ad` is optional and should be `null` or `buffer`. Included in the computation of authentication tag appended to the m
* `tag` should be a `buffer` of length `crypto_secretstream_xchacha20poly1305_TAGBYTES`

Note that `tag` should be one of the `crypto_secretstream_xchacha20poly1305_TAG_*` constants. Returns number of encrypted bytes written to `c`.
***
## `crypto_secretstream_xchacha20poly1305_init_pull`
![sodium-native][node]
``` js
sodium.crypto_secretstream_xchacha20poly1305_init_pull(state, header, k)
```
Initializes `state` from the reader side with message `header` and encryption `key`. The `header` must be retrieved from somewhere. The `key` must be exchanged securely with the sending / writing side.
* `state` should be an opaque state object
* `header` should be a `buffer` of length `crypto_secretstream_xchacha20poly1305_HEADERBYTES`
* `k` should be a `buffer` of length `crypto_secretstream_xchacha20poly1305_KEYBYTES`
***
## `crypto_secretstream_xchacha20poly1305_pull`
![sodium-native][node]
``` js
var clen = sodium.crypto_secretstream_xchacha20poly1305_pull(state, m, tag, c, [ad])
```
Decrypts a message with optional additional data `ad`, and writes message tag to `tag`. Make sure to check this!
* `state` should be an opaque state object
* `m` should be a `buffer` of length `c.length - crypto_secretstream_xchacha20poly1305_ABYTES`
* `tag` should be a `buffer` of length `crypto_secretstream_xchacha20poly1305_TAGBYTES`
* `ad` is optional and should be `null` or `buffer`. Included in the computation of the authentication tag appended to the m

Note that `tag` should be one of the `crypto_secretstream_xchacha20poly1305_TAG_*` constants. Returns number of decrypted bytes written to `m`.
***
## `crypto_secretstream_xchacha20poly1305_rekey`
![sodium-native][node]
``` js
sodium.crypto_secretstream_xchacha20poly1305_rekey(state)
```
Rekeys the opaque `state` object.

## Stateful API
__Replaces the above Opaque implementation from sodium-native v3.0.0__

``` js
var state = Buffer.alloc(sodium.crypto_secretstream_xchacha20poly1305_STATEBYTES)
```
Create a new `buffer` to hold stream state.
***
## `crypto_secretstream_xchacha20poly1305_init_push`
![sodium-native][node]
``` js
sodium.crypto_secretstream_xchacha20poly1305_init_push(state, header, k)
```
Initializes `state` from the writer side with message `header` and encryption `key`. The `header` must be sent or stored with the stream. The `key` must be exchanged securely with the receiving / reading side.
* `state` should be a `buffer` of length `crypto_secretstream_xchacha20_poly1305_STATEBYTES` bytes
* `header` should be a `buffer` of length `crypto_secretstream_xchacha20poly1305_HEADERBYTES`
* `k` should be a `buffer` of length `crypto_secretstream_xchacha20poly1305_KEYBYTES`
***
## `crypto_secretstream_xchacha20poly1305_push`
![sodium-native][node]
``` js
var mlen = sodium.crypto_secretstream_xchacha20poly1305_push(state, c, m, [ad], tag)
```
Encrypts a message with a certain tag and optional additional data `ad`.
* `state` should be a `buffer` of length `crypto_secretstream_xchacha20_poly1305_STATEBYTES` bytes
* `c` should be a `buffer` of length `m.length + crypto_secretstream_xchacha20poly1305_ABYTES`
* `m` should be a `buffer`
* `ad` is optional and should be `null` or `buffer`. Included in the computation of authentication tag appended to the m
* `tag` should be a `buffer` of length `crypto_secretstream_xchacha20poly1305_TAGBYTES`

Note that `tag` should be one of the `crypto_secretstream_xchacha20poly1305_TAG_*` constants. Returns number of encrypted bytes written to `c`.
***
## `crypto_secretstream_xchacha20poly1305_init_pull`
![sodium-native][node]
``` js
sodium.crypto_secretstream_xchacha20poly1305_init_pull(state, header, k)
```
Initializes `state` from the reader side with message `header` and encryption `key`. The `header` must be retrieved from somewhere. The `key` must be exchanged securely with the sending / writing side.
* `state` should be a `buffer` of length `crypto_secretstream_xchacha20_poly1305_STATEBYTES` bytes
* `header` should be a `buffer` of length `crypto_secretstream_xchacha20poly1305_HEADERBYTES`
* `k` should be a `buffer` of length `crypto_secretstream_xchacha20poly1305_KEYBYTES`
***
## `crypto_secretstream_xchacha20poly1305_pull`
![sodium-native][node]
``` js
var clen = sodium.crypto_secretstream_xchacha20poly1305_pull(state, m, tag, c, [ad])
```
Decrypts a message with optional additional data `ad`, and writes message tag to `tag`. Make sure to check this!
* `state` should be a `buffer` of length `crypto_secretstream_xchacha20_poly1305_STATEBYTES` bytes
* `m` should be a `buffer` of length `c.length - crypto_secretstream_xchacha20poly1305_ABYTES`
* `tag` should be a `buffer` of length `crypto_secretstream_xchacha20poly1305_TAGBYTES`
* `ad` is optional and should be `null` or `buffer`. Included in the computation of the authentication tag appended to the m

Note that `tag` should be one of the `crypto_secretstream_xchacha20poly1305_TAG_*` constants. Returns number of decrypted bytes written to `m`.
***
## `crypto_secretstream_xchacha20poly1305_rekey`
![sodium-native][node]
``` js
sodium.crypto_secretstream_xchacha20poly1305_rekey(state)
```
Rekeys the `state` buffer.

[js]: /docs/img/icon_js.svg
[node]: /docs/img/nodejs-icon.svg
