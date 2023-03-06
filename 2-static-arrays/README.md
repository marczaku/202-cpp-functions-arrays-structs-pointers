# 2 Static Arrays

```cpp
int numbers[100];
```

Size of the Array must be constant!
- unless you compile with `gcc`/`g++`

## Initialization

```cpp
int array[] = {1, 2, 3, 4};
```

## Index Operator

```cpp
printf("Second element: %d", array[1]);
```

## for

You should not use `int`, since it does not guarantee to be able to store an Array of `MAX_SIZE`:

```cpp
for(int i = 0; i < 10; ++i) {
	printf("%d\n", i);
}
```

Instead use `size_t`, especially, if you want to support any edge case:

```cpp
for(size_t i = 0; i < 5; i++) {
	printf("%d\n", array[i]);
}
```

## Range-Based For

```cpp
for(int number : array) {
	printf("%d\n", number);
}
```

## Number of Elements in Array

The hard, manual way:

```cpp
size_t count = sizeof(array) / sizeof(int);
```

Better:

```cpp
#include <array>
size_t count = std::size(array);
```