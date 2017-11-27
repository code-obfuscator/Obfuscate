# Obfuscate
Compile-time string literal obfuscation library for C++14.

## Whats the problem?
When plain text string literals are used in C++ programs, they will be compiled as-is into the resultant binary. This causes them to be incredibly easy to find. One can simply open up the binary file in a text editor to see all of the embedded string literals in plain view. A specialy utility called [strings](https://en.wikipedia.org/wiki/Strings_(Unix)) actually exists which can be used to search binary files for plain text strings.

## What does this library do?
This header-only library seeks to make it much much more difficult for embedded string literals in binary files to be found by encrypting them at compile-time, forcing the compiler to store the encrypted string literal instead of the plain text version. This will then be decrypted at runtime to be utilised within the program.

## How do I use this library?
By simply wrapping your string literal `"My String"` with `AY_OBFUSCATE("My String")` it will be encrypted at compile time and stored in an `ay::obfuscated_data` object which you can manipulate at runtime. For convenience it is also implicitly convertable to a `char*`.

For example, the following program will not store the string "Hello World" in plain text anywhere in the compiled executable.
```c++
int main()
{
  std::cout << AY_OBFUSCATE("Hello World") << std::endl;
  return 0;
}
```

## Binary file size overhead
This does come at a small cost. In a very simple program, which only prints out strings the following binary file bloat exists.

| Config | Plain string literals | Obfuscated strings | Bloat |
|:------:|:---------------------:|:------------------:|:-----:|
| Release | 9216 | 9728 | 512 (5.6%) |
| Debug | 37376 | 48640 | 11264 (30.1%) |

This output is generated by running calculate_bloat.py
