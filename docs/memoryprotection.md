---
id: memoryprotection
title: Memory Protection
sidebar_label: Memory Protection
---

Bindings for the secure memory API. [See the libsodium "Securing memory allocations" docs for more information](https://download.libsodium.org/doc/memory_management).
***
## `sodium_memzero` 
![sodium-node][node]
``` js
sodium.sodium_memzero(buffer)
```
Zeros out the data in `buffer`.
***
## `sodium_mlock`
![sodium-node][node]
``` js
sodium.sodium_mlock(buffer)
```
Locks the memory contained in `buffer`.
***
## `sodium_munlock`
![sodium-node][node]
``` js
sodium.sodium_munlock(buffer)
```
Unlocks previously `sodium_mlock`'ed memory contained in `buffer`. This will also `sodium_memzero` of `buffer`.
***
## `sodium_malloc`
![sodium-node][node]
``` js
var buffer = sodium.sodium_malloc(size)
```
Allocates a `buffer` of `size` which is memory protected. See [libsodium docs](https://download.libsodium.org/doc/memory_management#guarded-heap-allocations) for details. Be aware that many `buffer`-methods may break the security guarantees of `sodium.sodium_malloc`'ed memory. To check if a `buffer` is a "secure" `buffer`, you can call access the getter `buffer.secure` which will be `true`.
***
## `sodium_mprotect_noaccess`
![sodium-node][node]
``` js
sodium.sodium_mprotect_noaccess(buffer)
```
Makes `buffer` which was allocated using `sodium.sodium_malloc` inaccessible, crashing the process if any access is attempted. Note that this will have no effect for normal `buffer`'s.
***
## `sodium_mprotect_readonly`
![sodium-node][node]
``` js
sodium.sodium_mprotect_readonly(buffer)
```
Makes `buffer` which was allocated using `sodium.sodium_malloc` read-only, crashing the process if any writing is attempted. Note that this will have no effect for normal `buffer`'s.
***
## `sodium_mprotect_readwrite`
![sodium-node][node]
``` js
sodium.sodium_mprotect_readwrite(buffer)
```
Makes `buffer` which was allocated using `sodium.sodium_malloc` read-write, undoing `sodium_mprotect_noaccess` or `sodium_mprotect_readonly`. Note that this will have no effect for normal `buffer`'s.


[js]: /docusaurus/img/icon_js.svg
[node]: /docusaurus/img/nodejs-icon.svg