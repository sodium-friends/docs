---
id: passwordhashing
title: Password Hashing
sidebar_label: Password Hashing
---

Bindings for the crypto_pwhash API. [See the libsodium crypto_pwhash docs for more information](https://download.libsodium.org/doc/password_hashing/).

## Constants
**Buffer lengths (integer)**
* `crypto_pwhash_BYTES_MIN`
* `crypto_pwhash_BYTES_MAX`
* `crypto_pwhash_SALTBYTES`
* `crypto_pwhash_STRBYTES`
* `crypto_pwhash_OPSLIMIT_MIN`
* `crypto_pwhash_OPSLIMIT_MAX`
* `crypto_pwhash_MEMLIMIT_MIN`
* `crypto_pwhash_MEMLIMIT_MAX`
**String constants (string)**
*

***
## `crypto_pwhash`
![sodium-native][node]
``` js
sodium.crypto_pwhash(output, password, salt, opslimit, memlimit, algorithm)
```
Creates a password hash.
* `output` should be a `buffer` of length within `crypto_pwhash_BYTES_MIN` - `crypto_pwhash_BYTES_MAX`
* `password` should be a `buffer` of any size
* `salt` should be a `buffer` of length `crypto_pwhash_SALTBYTES`
* `opslimit` should be a number containing your ops limit setting in the range `crypto_pwhash_OPSLIMIT_MIN` - `crypto_pwhash_OPSLIMIT_MAX`
* `memlimit` should be a number containing your mem limit setting in the range `crypto_pwhash_MEMLIMIT_MIN` - `crypto_pwhash_OPSLIMIT_MAX`
`algorithm` should be a number specifying the algorithm you want to use

Available default `opslimit`'s and `memlimit`'s are
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
***
## `crypto_pwhash_str`
![sodium-native][node]
``` js
sodium.crypto_pwhash_str(output, password, opslimit, memlimit)
```
Creates a password hash with a random salt.
* `output` should be a `buffer` of length `crypto_pwhash_STRBYTES`
* `password` should be a `buffer` of any size
* `opslimit` should be a number containing your ops limit setting in the range `crypto_pwhash_OPSLIMIT_MIN` - `crypto_pwhash_OPSLIMIT_MAX`
* `memlimit` should be a number containing your mem limit setting in the range `crypto_pwhash_MEMLIMIT_MIN` - `crypto_pwhash_OPSLIMIT_MAX`

The generated hash, settings, salt, version and algorithm will be stored in `output` and the entire `output buffer` will be used.
***
## `crypto_pwhash_str_verify`
![sodium-native][node]
``` js
var bool = sodium.crypto_pwhash_str_verify(str, password)
```
Verifies a password hash generated with the above method.
* `str` should be a `buffer` of length `crypto_pwhash_STRBYTES`
* `password` should be a `buffer` of any size

Returns `true` if the hash could be verified with the settings contained in `str`. Otherwise `false`.
***
## `crypto_pwhash_str_needs_rehash`
![sodium-native][node]
``` js
var bool = sodium.crypto_pwhash_str_needs_rehash(hash, opslimit, memlimit)
```
Checks if a password hash needs rehash, either because the default algorithm changed, `opslimit` or `memlimit` increased, or because the hash is malformed.
* `hash` should be a `buffer` of length `crypto_pwhash_STRBYTES`
* `opslimit` should be a number containing your ops limit setting in the range `crypto_pwhash_OPSLIMIT_MIN` - `crypto_pwhash_OPSLIMIT_MAX`
* `memlimit` should be a number containing your mem limit setting in the range `crypto_pwhash_MEMLIMIT_MIN` - `crypto_pwhash_OPSLIMIT_MAX`

Returns `true` if the hash should be rehashed the settings contained in `str`. Otherwise `false` if it is still good.
***
## `crypto_pwhash_async`
![sodium-native][node]
``` js
sodium.crypto_pwhash_async(output, password, salt, opslimit, memlimit, algorithm, callback)
```
Just like `crypto_pwhash`, but this will run password hashing on a separate worker, so it will not block the event loop. `callback(err)` will receive any errors from the hashing but all argument errors will throw. The resulting hash is written to `output`. This function also supports [`async_hook`s](https://nodejs.org/dist/latest/docs/api/async_hooks.html) as the type `sodium-native:crypto_pwhash_async`.
***
## `crypto_pwhash_str_async`
![sodium-native][node]
``` js
sodium.crypto_pwhash_str_async(output, password, opslimit, memlimit, callback)
```
Just like `crypto_pwhash_str`, but this will run password hashing on a separate worker, so it will not block the event loop. `callback(err)` will receive any errors from the hashing but all argument errors will throw. The resulting hash with parameters is written to `output`. This function also supports [`async_hook`s](https://nodejs.org/dist/latest/docs/api/async_hooks.html) as the type `sodium-native:crypto_pwhash_str_async`.
***
## `crypto_pwhash_str_verify_async`
![sodium-native][node]
``` js
sodium.crypto_pwhash_str_verify_async(str, password, callback)
```
Just like `crypto_pwhash_str_verify`, but this will run password hashing on a separate worker, so it will not block the event loop. `callback(err, bool)` will receive any errors from the hashing but all argument errors will throw. If the verification succeeds bool is `true`, otherwise `false`. Due to an issue with libsodium `err` is currently never set. This function also supports [`async_hook`s](https://nodejs.org/dist/latest/docs/api/async_hooks.html) as the type `sodium-native:crypto_pwhash_str_verify_async`.

***
# Password Hashing (Scrypt)
Bindings for the crypto_pwhash_scryptsalsa208sha256 API. [See the libsodium crypto_pwhash_scryptsalsa208sha256 docs for more information](https://download.libsodium.org/doc/advanced/scrypt).
***
## Constants2
**Buffer lengths (integer)**
* `crypto_pwhash_scryptsalsa208sha256_BYTES_MIN`
* `crypto_pwhash_scryptsalsa208sha256_BYTES_MAX`
* `crypto_pwhash_scryptsalsa208sha256_SALTBYTES`
* `crypto_pwhash_scryptsalsa208sha256_OPSLIMIT_MIN`
* `crypto_pwhash_scryptsalsa208sha256_OPSLIMIT_MAX`
* `crypto_pwhash_scryptsalsa208sha256_MEMLIMIT_MIN`
* `crypto_pwhash_scryptsalsa208sha256_MEMLIMIT_MAX`
* `crypto_pwhash_scryptsalsa208sha256_STRBYTES`

***
## `crypto_pwhash_scryptsalsa208sha256`
![sodium-native][node]
``` js
sodium.crypto_pwhash_scryptsalsa208sha256(output, password, salt, opslimit, memlimit)
```
Creates a password hash.
* `output` should be a `buffer` of length within `crypto_pwhash_scryptsalsa208sha256_BYTES_MIN` - `crypto_pwhash_scryptsalsa208sha256_BYTES_MAX`
* `password` should be a `buffer` of any size
* `salt` should be a `buffer` of length `crypto_pwhash_scryptsalsa208sha256_SALTBYTES`
* `opslimit` should be a number containing your ops limit setting in the range `crypto_pwhash_scryptsalsa208sha256_OPSLIMIT_MIN` - `crypto_pwhash_scryptsalsa208sha256_OPSLIMIT_MAX`
* `memlimit` should be a number containing your mem limit setting in the range `crypto_pwhash_scryptsalsa208sha256_MEMLIMIT_MIN` - `crypto_pwhash_scryptsalsa208sha256_OPSLIMIT_MAX`

Available default `opslimit`'s and `memlimit`'s are
* `crypto_pwhash_scryptsalsa208sha256_OPSLIMIT_INTERACTIVE`
* `crypto_pwhash_scryptsalsa208sha256_OPSLIMIT_SENSITIVE`
* `crypto_pwhash_scryptsalsa208sha256_MEMLIMIT_INTERACTIVE`
* `crypto_pwhash_scryptsalsa208sha256_MEMLIMIT_SENSITIVE`

The generated hash will be stored in `output`, and the entire `output buffer` will be used.
***
## `crypto_pwhash_scryptsalsa208sha256_str`
![sodium-native][node]
``` js
sodium.crypto_pwhash_scryptsalsa208sha256_str(output, password, opslimit, memlimit)
```
Creates a password hash with a random salt.
* `output` should be a `buffer` of length `crypto_pwhash_scryptsalsa208sha256_STRBYTES`
* `password` should be a `buffer` of any size
* `opslimit` should be a number containing your ops limit setting in the range `crypto_pwhash_scryptsalsa208sha256_OPSLIMIT_MIN` - `crypto_pwhash_scryptsalsa208sha256_OPSLIMIT_MAX`
* `memlimit` should be a number containing your mem limit setting in the range `crypto_pwhash_scryptsalsa208sha256_MEMLIMIT_MIN` - `crypto_pwhash_scryptsalsa208sha256_OPSLIMIT_MAX`

The generated hash, settings, salt, version and algorithm will be stored in `output`, and the entire `output buffer` will be used.
***
## `crypto_pwhash_scryptsalsa208sha256_str_verify`
![sodium-native][node]
``` js
var bool = sodium.crypto_pwhash_scryptsalsa208sha256_str_verify(str, password)
```
Verifies a password hash generated with the above method.
* `str` should be a `buffer` of length `crypto_pwhash_scryptsalsa208sha256_STRBYTES`
* `password` should be a `buffer` of any size

Returns `true` if the hash could be verified with the settings contained in `str`. Otherwise `false`.
***
## `crypto_pwhash_scryptsalsa208sha256_str_needs_rehash`
![sodium-native][node]
``` js
var bool = sodium.crypto_pwhash_scryptsalsa208sha256_str_needs_rehash(hash, opslimit, memlimit)
```
Checks if a password hash needs rehash, either because `opslimit` or `memlimit` increased, or because the hash is malformed.
* `hash` should be a `buffer` of length `crypto_pwhash_scryptsalsa208sha256_STRBYTES`
* `opslimit` should be a number containing your ops limit setting in the range `crypto_pwhash_scryptsalsa208sha256_OPSLIMIT_MIN` - `crypto_pwhash_scryptsalsa208sha256_OPSLIMIT_MAX`
* `memlimit` should be a number containing your mem limit setting in the range `crypto_pwhash_scryptsalsa208sha256_MEMLIMIT_MIN` - `crypto_pwhash_scryptsalsa208sha256_OPSLIMIT_MAX`

Returns `true` if the hash should be rehashed the settings contained in `str`. Otherwise `false`, if it is still good.
***
## `crypto_pwhash_scryptsalsa208sha256_async`
![sodium-native][node]
``` js
sodium.crypto_pwhash_scryptsalsa208sha256_async(output, password, salt, opslimit, memlimit, callback)
```
Just like `crypto_pwhash_scryptsalsa208sha256`, but this will run password hashing on a separate worker so it will not block the event loop. `callback(err)` will receive any errors from the hashing but all argument errors will throw. The resulting hash is written to `output`. This function also supports [`async_hook`s](https://nodejs.org/dist/latest/docs/api/async_hooks.html) as the type `sodium-native:crypto_pwhash_scryptsalsa208sha256_async`.
***
## `crypto_pwhash_scryptsalsa208sha256_str_async`
![sodium-native][node]
``` js
sodium.crypto_pwhash_scryptsalsa208sha256_str_async(output, password, opslimit, memlimit, callback)
```
Just like `crypto_pwhash_scryptsalsa208sha256_str`, but this will run password hashing on a separate worker so it will not block the event loop. `callback(err)` will receive any errors from the hashing but all argument errors will throw. The resulting hash with parameters is written to `output`. This function also supports [`async_hook`s](https://nodejs.org/dist/latest/docs/api/async_hooks.html) as the type `sodium-native:crypto_pwhash_scryptsalsa208sha256_str_async`.
***
## `crypto_pwhash_scryptsalsa208sha256_str_verify_async`
![sodium-native][node]
``` js
sodium.crypto_pwhash_scryptsalsa208sha256_str_verify_async(str, password, callback)
```
Just like `crypto_pwhash_scryptsalsa208sha256_str_verify`, but this will run password hashing on a separate worker so it will not block the event loop. `callback(err, bool)` will receive any errors from the hashing but all argument errors will throw. If the verification succeeds bool is `true`, otherwise `false`. Due to an issue with libsodium `err` is currently never set. This function also supports [`async_hook`s](https://nodejs.org/dist/latest/docs/api/async_hooks.html) as the type `sodium-native:crypto_pwhash_scryptsalsa208sha256_str_verify_async`.


[js]: /docusaurus/img/icon_js.svg
[node]: /docusaurus/img/nodejs-icon.svg

