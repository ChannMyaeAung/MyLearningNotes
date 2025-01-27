#### Functions vs. macros

A macro is used to rewrite your code before it's compiled. The macros we are using here(`va_start`, `va_arg`, and `va_end`) might look like functions, but they actually hide secret instructions that tell the preprocessor how to generate lots of extra smart code inside our program, just before compiling it.



Q: Why are `va_end` and `va_start` called macros? Aren't they just normal functions?

A: No, they are designed to look like ordinary functions, but they actually are replaced by the preprocessor with other code.



Q: And the preprocessor is?

A: The preprocessor runs just before the compilation step. Among other things, the preprocessor includes the headers into the code.



Q: Can I have a function with just variable arguments, and no fixed arguments at all?

A: No, you need to have at least one fixed arguments in order to pass its name to `va_start`.



Q: What happens if I try to read more arguments from `va_arg` than have been passed in?

A: Random errors will occur.



Q: What if I try to read an `int` argument as a double, or something?

A: Random errors will occur.


