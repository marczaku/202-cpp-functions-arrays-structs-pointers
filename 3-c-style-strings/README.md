# 3 C-Style Strings
- Null-Terminated Character Array
- Ends with first occurrence of `'\0' (0)` 

```c++
char text[6] = "Hello";
```

| Array Index | 0  | 1   | 2   | 3   | 4   | 5  |
|-------------|----|-----|-----|-----|-----|----|
| char        | H  | e   | l   | l   | o   | \0 |
| int         | 72 | 101 | 108 | 108 | 111 | 0  |

## UTF / Unicode

```c++
char16_t chinese[] = u"\u4e66\u4e2d";
char32_t utf32[] = U"abc";
wchar_t unicode[] = L"abc";
```

Demo Code:

```c++
#include <iostream>
#include <windows.h>

int main() {

    // Print UTF-8 characters using wide characters
    wchar_t utf8String[] = L"Hello, 你好, 🌍!";
    WriteConsoleW(GetStdHandle(STD_OUTPUT_HANDLE), utf8String, wcslen(utf8String), NULL, NULL);
    std::wcout << std::endl;

    return 0;
}

```

## Multiline

```c++
char text[] = "hello "
	"world "
	"this is " "c++.";
```

## Printf

```c++
printf("Here you go: %s", text);
```

## EXERCISE: STRING WITH ALL LETTERS
- Create a String with all letters A-Z
  - don't type in the characters manually
  - instead fill them in the string using a loop
- >>THEN<< (AFTER FILLING, NOT WHILE FILLING) Print the string to the console
