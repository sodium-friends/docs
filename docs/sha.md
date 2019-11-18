---
id: sha
title: SHA
sidebar_label: SHA
---

***
## `crypto_hash_sha256`
![sodium-node][node]
``` js
sodium.crypto_hash_sha256(output, input)
```
Hashes a value to a short hash based on a key.
* `output` should be a `buffer` of length `crypto_hash_sha256_BYTES`
* `input` should be a `buffer` of any size

The generated short hash is stored in `output`.
***
## `crypto_hash_sha256_instance`
![sodium-node][node]
``` js
var instance = sodium.crypto_hash_sha256_instance()
```
Creates an instance that has stream of input data to `sha256`.

## `instance.update`
``` js
instance.update(input)
```
Updates the instance with a new piece of data.
* `input` should be a `buffer` of any size

## `instance.final`
``` js
instance.final(output)
```
Finalizes the instance.
* `output` should be a `buffer` of length `crypto_hash_sha256_BYTES`

The generated hash is stored in `output`.
***
## `crypto_hash_sha512`
![sodium-node][node]
``` js
sodium.crypto_hash_sha512(output, input)
```
Hashes a value to a short hash based on a key.
* `output` should be a `buffer` of length `crypto_hash_sha512_BYTES`
* `input` should be a `buffer` of any size

The generated short hash is stored in `output`.
***
## `crypto_hash_sha512_instance`
![sodium-node][node]
``` js
var instance = sodium.crypto_hash_sha512_instance()
```
Creates an instance that has stream of input data to `sha512`.

## `instance.update`
``` js
instance.update(input)
```
Updates the instance with a new piece of data.
* `input` should be a `buffer` of any size

## `instance.final`
``` js
instance.final(output)
```
Finalizes the instance.
* `output` should be a `buffer` of `length crypto_hash_sha512_BYTES`

The generated hash is stored in `output`.


[js]: /docusaurus/img/icon_js.svg
[node]: /docusaurus/img/nodejs-icon.svg