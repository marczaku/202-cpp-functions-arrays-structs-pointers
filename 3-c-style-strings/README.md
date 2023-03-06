# 3 C-Style Strings
- Null-Terminated String
- Ends with first occurrence of `'\0'` or `0`

```cpp
char text[] = "Hello";
```

Array Index|0|1|2|3|4|5
-|-|-|-|-|-|-
char|H|e|l|l|o|\0
int|72|101|108|108|111|0

## UTF / Unicode

```cpp
char16_t chinese[] = u"\u4e66\u4e2d";
char32_t utf32[] = U"abc";
wchar_t unicode[] = L"abc";
```

## Multiline

```cpp
char text[] = "hello "
	"world "
	"this is " "c++.";
```

## Printf

```cpp
printf("Here you go: %s", text);
```

## EXERCISE: STRING WITH ALL LETTERS
- Create a String with all letters A-Z
  - don't type in the characters manually
  - instead fill them in the string using a loop
- Then, Print the string to the console