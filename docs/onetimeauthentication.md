---
id: onetimeauthentication
title: One-Time Authentication
sidebar_label: One-Time Authentication
---

Bindings for the crypto_onetimeauth API. [See the libsodium crypto_onetimeauth docs for more information](https://download.libsodium.org/doc/advanced/poly1305).
***
## `crypto_onetimeauth`
![sodium-node][node]
``` js
sodium.crypto_onetimeauth(output, input, key)
```
Create a authentication token based on a onetime key.
* `output` should be a `buffer` of length `crypto_onetimauth_BYTES`
* `input` should be a `buffer` of any size
* `key` should be a `buffer` of length `crypto_onetimeauth_KEYBYTES`

The generated token is stored in `output`.
***
## `crypto_onetimeauth_verify`
![sodium-node][node]
``` js
var bool = sodium.crypto_onetimeauth_verify(output, input, key)
```
Verify a token.
* `output` should be a `buffer` of length `crypto_onetimeauth_BYTES`
* `input` should be a `buffer` of any size
* `key` should be a `buffer` of length `crypto_onetimeauth_KEYBYTES`

Returns `true` if the token could be verified. Otherwise `false`.
***
## `crypto_onetimeauth_instance`
![sodium-node][node]
``` js
var instance = sodium.crypto_onetimeauth_instance(key)
```
Create an instance that creates a token from a onetime key and a stream of input data.
* `key` should be a `buffer` of length `crypto_onetimeauth_KEYBYTES`

## `instance.update`
``` js
instance.update(input)
```
Update the instance with a new piece of data.
* `input` should be a `buffer` of any size

## `instance.final`
``` js
instance.final(output)
```
Finalize the instance.
* `output` should be a `buffer` of length `crypto_onetimeauth_BYTES`

The generated hash is stored in `output`.


[js]: /docusaurus/img/icon_js.svg
[node]: /docusaurus/img/nodejs-icon.svg
