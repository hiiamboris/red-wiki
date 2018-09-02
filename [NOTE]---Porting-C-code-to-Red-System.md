Red/System and C have roughly analogous functionality although they have very different syntax. Here are some notes about porting C/C++ to Red/System.
## Types
| C/C++ | Red/System| Notes
| --- | ---- | ---
| `char` | `byte!` | The signedness of a C++ char can be signed or unsigned - the assumption here is signed but it varies by target system.<br>A Red/System `byte!` is unsigned.
| `unsigned char` | `byte!` |
| `signed char` | `N/A` |
| `short int` | `N/A` | No corresponding type in Red/System.
| `unsigned short int` | `N/A` |
| `(signed) int` | `integer!` | In C/C++, `int` can be 16-bit on some old platforms (e.g DOS / Windows 3.1) [<sup>1</sup>](#data-model)
| `unsigned int` | `N/A` | No `unsigned integer!` in Red/System yet.
| `(signed) long int` | `integer!` or `N/A` | In C/C++ this is data model dependent, can be 32-bit or 64-bit [<sup>1</sup>](#data-model)
| `unsigned long int` | `integer!` or `N/A` | In C/C++ this is data model dependent, can be 32-bit or 64-bit [<sup>1</sup>](#data-model)
| `(signed) long long int` | `N/A` |
| `unsigned long long int` | `N/A` |
| `size_t` | `N/A` | 
| `float` | `float32!` |
| `double` | `float!` |
| `long double` | `N/A` |
| `bool` | `logic!` |

<sup>[1]</sup> See the next section to for a discussion on data models.

### Data model

The four common data models in C++ are:

* LP32 - `int` is 16-bit, `long` and pointers are 32-bit. This is an uncommon model, a throw-back to DOS / Windows 3.1
* ILP32 - `int`, `long` and pointers are 32-bit. Used by Win32, Linux, OS X
* LLP64 - `int` and `long` are 32-bit, `long long` and pointers are 64-bit. Used by Win64
* LP64 - `int` is 32-bit, `long` / `long long` and pointers are 64-bit. Used by Linux, OS X

### stdint.h / cstdint

C provides a `<stdint.h>` header that provides unambigious typedefs with length and signedess, e.g. `uint32_t`. The equivalent in C++ is `<cstdlib>`.

| C/C++ | Red/System
| --- | ----
| `int8_t` | `N/A`
| `uint8_t` | `byte!`
| `int16_t` | `N/A`
| `uint16_t` | `N/A`
| `uint32_t` | `N/A`
| `int32_t` | `integer!`
| `int64_t` | `N/A`
| `uint64_t` | `N/A`

### Pointer

| C/C++ | Red/System
| --- | ----
| `int *` | `int-ptr!` or `pointer! [integer!]`
| `char *` | `byte-ptr!` or `pointer! [byte!]` or `c-string!`
| `float *` | `float32-ptr!` or `pointer! [float32!]`
| `double *` | `float-ptr!` or `pointer! [float!]`
| `void *` | `int-ptr!` or `byte-ptr!`

### Arrays

An array is a fixed size list of elements. Only literal arrays are supported in Red/System.

E.g to create a 100 element array of `double` values in C++:

```c++
// Stack
int values[100];
// Heap
double *values = new double[100];
delete []values;
// Data segment
static int values[] = {1, 2, 3, 4}
```

And in Red/System:

```red/system
// Stack
values: system/stack/allocate 100    ;@@ No need to 
// Heap
values: as float-ptr! allocate 100 * size? float!
free as byte-ptr! values
// Data segment
values: [1 2 3 4]
```

Example: Convert GUID to R/S array.
```c++
const IID IID_IWinHttpRequest =
{
  0x06f29373,
  0x5c5a,
  0x4b54,
  {0xb0, 0x25, 0x6e, 0xf1, 0xbf, 0x8a, 0xbf, 0x0e}
};
```
Convert it to Red/System.
```red/system
IID_IWinHttpRequest: [06F29373h 4B545C5Ah F16E25B0h 0EBF8ABFh]
```