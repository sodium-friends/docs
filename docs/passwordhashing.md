---
id: passwordhashing
title: Password hashing
sidebar_label: Password hashing
---

Bindings for the crypto_pwhash API. [See the libsodium crypto_pwhash docs for more information](https://download.libsodium.org/doc/password_hashing/).

``` js
crypto_pwhash(output, password, salt, opslimit, memlimit, algorithm)
```
Create a password hash.
* `output` should be a `buffer` of length within `crypto_pwhash_BYTES_MIN` - `crypto_pwhash_BYTES_MAX`
* `password` should be a `buffer` of any size
* `salt` should be a `buffer` of length `crypto_pwhash_SALTBYTES`
* `opslimit` should be a number containing your ops limit setting in the range `crypto_pwhash_OPSLIMIT_MIN` - `crypto_pwhash_OPSLIMIT_MAX`
* `memlimit` should be a number containing your mem limit setting in the range `crypto_pwhash_MEMLIMIT_MIN` - `crypto_pwhash_OPSLIMIT_MAX`
`algorithm` should be a number specifying the algorithm you want to use

Available default `ops` and `memlimit`'s are
* `crypto_pwhash_OPSLIMIT_INTERACTIVE`
* `crypto_pwhash_OPSLIMIT_MODERATE`
* `crypto_pwhash_OPSLIMIT_SENSITIVE`
* `crypto_pwhash_MEMLIMIT_INTERACTIVE`
* `crypto_pwhash_MEMLIMIT_MODERATE`
* `crypto_pwhash_MEMLIMIT_SENSITIVE`

The available `algorithm`'s are
* `crypto_pwhash_ALG_DEFAULT`
* `crypto_pwhash_ALG_ARGON2ID13`
* `crypto_pwhash_ALG_ARGON2I13`

The generated hash will be stored in `output` and the entire `output buffer` will be used.

``` js
crypto_pwhash_str(output, password, opslimit, memlimit)
```
Create a password hash with a random salt.
* `output` should be a `buffer` of length `crypto_pwhash_STRBYTES`
* `password` should be a `buffer` of any size
* `opslimit` should be a number containing your ops limit setting in the range `crypto_pwhash_OPSLIMIT_MIN` - `crypto_pwhash_OPSLIMIT_MAX`
* `memlimit` should be a number containing your mem limit setting in the range `crypto_pwhash_MEMLIMIT_MIN` - `crypto_pwhash_OPSLIMIT_MAX`

The generated hash, settings, salt, version and algorithm will be stored in `output` and the entire `output buffer` will be used.

``` js
var bool = crypto_pwhash_str_verify(str, password)
```
Verify a password hash generated with the above method.
* `str` should be a `buffer` of length `crypto_pwhash_STRBYTES`
* `password` should be a `buffer` of any size

Returns `true` if the hash could be verified with the settings contained in `str`. Otherwise `false`.

``` js
var bool = crypto_pwhash_str_needs_rehash(hash, opslimit, memlimit)
```
Check if a password hash needs rehash, either because the default algorithm changed, `opslimit` or `memlimit` increased or because the hash is malformed.
* `hash` should be a `buffer` of length `crypto_pwhash_STRBYTES`
* `opslimit` should be a number containing your ops limit setting in the range `crypto_pwhash_OPSLIMIT_MIN` - `crypto_pwhash_OPSLIMIT_MAX`
* `memlimit` should be a number containing your mem limit setting in the range `crypto_pwhash_MEMLIMIT_MIN` - `crypto_pwhash_OPSLIMIT_MAX`

Returns `true` if the hash should be rehashed the settings contained in `str`. Otherwise `false` if it is still good.

``` js
crypto_pwhash_async(output, password, salt, opslimit, memlimit, algorithm, callback)
```
Just like `crypto_pwhash` but will run password hashing on a separate worker so it will not block the event loop. `callback(err)` will receive any errors from the hashing but all argument errors will throw. The resulting hash is written to `output`. This function also supports [`async_hook`s](https://nodejs.org/dist/latest/docs/api/async_hooks.html) as the type `sodium-native:crypto_pwhash_async`.

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
* `output` should be a `buffer` of length within `crypto_pwhash_scryptsalsa208sha256_BYTES_MIN` - `crypto_pwhash_scryptsalsa208sha256_BYTES_MAX`
* `password` should be a `buffer` of any size
* `salt` should be a `buffer` of length `crypto_pwhash_scryptsalsa208sha256_SALTBYTES`
* `opslimit` should be a number containing your ops limit setting in the range `crypto_pwhash_scryptsalsa208sha256_OPSLIMIT_MIN` - `crypto_pwhash_scryptsalsa208sha256_OPSLIMIT_MAX`
* `memlimit` should be a number containing your mem limit setting in the range `crypto_pwhash_scryptsalsa208sha256_MEMLIMIT_MIN` - `crypto_pwhash_scryptsalsa208sha256_OPSLIMIT_MAX`

Available default `ops` and `memlimit`'s are
* `crypto_pwhash_scryptsalsa208sha256_OPSLIMIT_INTERACTIVE`
* `crypto_pwhash_scryptsalsa208sha256_OPSLIMIT_SENSITIVE`
* `crypto_pwhash_scryptsalsa208sha256_MEMLIMIT_INTERACTIVE`
* `crypto_pwhash_scryptsalsa208sha256_MEMLIMIT_SENSITIVE`

The generated hash will be stored in `output` and the entire `output buffer` will be used.

``` js
crypto_pwhash_scryptsalsa208sha256_str(output, password, opslimit, memlimit)
```
Create a password hash with a random salt.
* `output` should be a `buffer` of length `crypto_pwhash_scryptsalsa208sha256_STRBYTES`
* `password` should be a `buffer` of any size
* `opslimit` should be a number containing your ops limit setting in the range `crypto_pwhash_scryptsalsa208sha256_OPSLIMIT_MIN` - `crypto_pwhash_scryptsalsa208sha256_OPSLIMIT_MAX`
* `memlimit` should be a number containing your mem limit setting in the range `crypto_pwhash_scryptsalsa208sha256_MEMLIMIT_MIN` - `crypto_pwhash_scryptsalsa208sha256_OPSLIMIT_MAX`

The generated hash, settings, salt, version and algorithm will be stored in `output` and the entire `output buffer` will be used.

``` js
var bool = crypto_pwhash_scryptsalsa208sha256_str_verify(str, password)
```
Verify a password hash generated with the above method.
* `str` should be a `buffer` of length `crypto_pwhash_scryptsalsa208sha256_STRBYTES`
* `password` should be a `buffer` of any size

Returns `true` if the hash could be verified with the settings contained in `str`. Otherwise `false`.

``` js
var bool = crypto_pwhash_scryptsalsa208sha256_str_needs_rehash(hash, opslimit, memlimit)
```
Check if a password hash needs rehash, either because `opslimit` or `memlimit` increased or because the hash is malformed.
* `hash` should be a `buffer` of length `crypto_pwhash_scryptsalsa208sha256_STRBYTES`
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

