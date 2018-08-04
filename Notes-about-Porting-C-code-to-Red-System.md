Red/System and C have roughly analogous functionality although they have very different syntax. Here are some notes about porting C/C++ to Red/System.
### Types
| C/C++ | Red/System| Notes
| --- | ---- | ---
| `char` | `byte!` | The signedness of a C++ char can be signed or unsigned - the assumption here is signed but it varies by target system.<br>A Red/System `byte!` is unsigned.
| `unsigned char` | `byte!` |
| `signed char` | `byte!` |
| `short int` | `N/A` | No corresponding type in Red/System.
| `unsigned short int` | `N/A` |
| `(signed) int` | `integer!` | In C/C++, `int` can be 16-bit on some old platforms (e.g DOS / Windows 3.1) [<sup>1</sup>](#data-model)
| `unsigned int` | `integer!` | No `unsigned integer!` in Red/System.
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