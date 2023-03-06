# 3 C-Style Strings

```cpp
char text[] = "Hello world.";
char16_t chinese[] = u"\u4e66\u4e2d";
char32_t utf32[] = U"abc";
wchar_t unicode[] = L"abc";
```
- Null-Terminated String
- Means, it ends with first occurence of `'\0'` or `0`

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
- Print the string to the console