---
id: sha
title: SHA
sidebar_label: SHA
---

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