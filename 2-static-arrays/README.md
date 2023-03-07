# 2 Static Arrays

```cpp
int numbers[100];
```

Size of the Array must be constant!
- unless you compile with `gcc`/`g++`

```cpp
int size = 5;
int numbers[size]; // ERROR!
```

```cpp
const int size = 5;
int numbers[size]; // OK!
```

## Uninitialized Arrays
Have random values! Do never forget to initialize:

```cpp
int numbers[5];
for(int number : numbers){
	printf("%d, ", number);
}
```

Output:
```
-1521484208, 72, -538963304, 32758, -538959480, 
```

## Initialization

```cpp
int array[] = {1, 2, 3, 4};
for(int number : numbers){
	printf("%d\n", number);
}
```

Output:
```
1, 2, 3, 4, 
```

## Index Operator

```cpp
printf("Second element: %d", array[1]); // 2
```

## for

You should not use `int`, since it does not guarantee to be able to store an Array of `MAX_SIZE`:

```cpp
for(int i = 0; i < 10; ++i) {
	printf("%d\n", i);
}
```

Instead use `size_t`, especially, if you want to support edge case:

```cpp
for(size_t i = 0; i < 5; i++) {
	printf("%d\n", array[i]);
}
```

## Range-Based For
Like `foreach`:

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

## EXERCISE
Ask the user for 9 numbers. Store them in an array. Return the Average of all numbers.

## BONUS EXERCISE
Return the Median of those numbers.

## EXERCISE
Left-Rotate an Array.
- All elements should move one index to the left (to a lower index)
- The first element should move to the last Index

```
Before: 12 14 16 18 20
After: 14 16 18 20 12
```

## EXERCISE
Implement a Tic-Tac-Toe Game.