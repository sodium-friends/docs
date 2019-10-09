---
id: memoryprotection
title: Memory Protection
sidebar_label: Memory Protection
---

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
