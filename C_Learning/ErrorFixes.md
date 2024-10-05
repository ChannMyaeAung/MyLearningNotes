### Incomple type is not allowed (Usually happened in `netdb.h` and `signal.h` header files)

##### Fix 

```C
#define _XOPEN_SOURCE 700
```

Adding `#define _XOPEN_SOURCE 700` at the top of our file can resolve issues with certain headers and definitions in C because it controls the feature test macros that influence which features and functions are exposed by the standard library headers.

### Explanation of `_XOPEN_SOURCE`

The `_XOPEN_SOURCE` macro is used to enable features from the X/Open and POSIX standards. Here's a breakdown of what it does:

- **Feature Test Macros**: These macros allow you to enable or disable features from various standards, such as POSIX, X/Open, and others. By defining these macros, we can make certain functions and definitions available that are otherwise hidden to ensure portability and standard compliance.
- **700**: The value `700` corresponds to POSIX.1-2008 (also known as POSIX.1-2008 with XSI extensions). This means that when you define `_XOPEN_SOURCE` as `700`, you are asking the compiler to expose features that conform to the POSIX.1-2008 standard and its XSI extensions.

### How it Fixed the Issue

- **Exposing Definitions**: By defining `_XOPEN_SOURCE` with an appropriate value, we made sure that the `netdb.h` header file includes the necessary definitions for structures and functions like `struct addrinfo` and others that are part of the POSIX standards.
- **Compiler Behavior**: When you include standard headers like `netdb.h`, the presence of `_XOPEN_SOURCE` with a suitable value ensures that the compiler exposes the required declarations and definitions. Without this macro, the compiler might hide these definitions to maintain compatibility with older or more restricted standards.

### Example of the Fix

```C
#define _XOPEN_SOURCE 700
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <errno.h>
#include <sys/types.h>
#include <sys/wait.h>
#include <time.h>
#include <signal.h>
#include <netdb.h>
```

By adding `#define _XOPEN_SOURCE 700` at the beginning of our code, we instructed the compiler to enable features from the POSIX.1-2008 standard, thereby making sure that the necessary functions and definitions are available and resolving the issues with missing definitions.

### Conclusion

Defining `_XOPEN_SOURCE 700` effectively instructs the compiler to provide the necessary definitions for modern POSIX features, which resolved the issues we were facing with incomplete type definitions and missing functions in the `netdb.h` header. This is a common practice in C programming to ensure that the required standards and features are available during compilation.



### Mistakenly Printing a control code

![Screenshot from 2024-10-05 22-46-49](/home/chan/Pictures/Screenshots/Screenshot from 2024-10-05 22-46-49.png)

![Screenshot from 2024-10-05 22-46-41](/home/chan/Pictures/Screenshots/Screenshot from 2024-10-05 22-46-41.png)

- If we outputted a control code  on purpose or perhaps accidentally, if a control code whacks out the terminal display, issue the **rest** command. Type **reset** and press ENTER, and the terminal attempts to recover itself from however we messed up.